# Debug Brief: Metrics Not Reaching Dynatrace (NSVS OpenTelemetry POC)

## Your role
You are debugging why application metrics are recorded inside the NSVS Spring Boot service but never appear in Dynatrace. Work through the investigation below, produce a diagnosis report with evidence, and propose a fix as a diff. **Do not commit, push, or apply changes until the diagnosis has been reviewed.**

## Context
- Service: NSVS (Spring Boot), on the OpenTelemetry POC feature branch.
- Goal: export custom business metrics and JVM metrics to **Dynatrace** (the only target backend) and visualise them on a Dynatrace dashboard built with DQL.
- Metrics are captured without touching business logic: custom annotations (e.g. `@RecordEventConsume`) backed by Spring AOP aspects record metrics around existing methods.
- Custom metrics include: records persisted (tagged by acquirer ICA), file notifications received (tagged by file type, e.g. R111, TU4Q), batch statuses, prod/SMB pairing, idle unpaired settlement pairs, and error counts (missing TLC ID, missing data elements, Kafka errors).
- JVM metrics are enabled via `management.metrics.enable.jvm/logback/process/system`.
- The team plans to adopt the Mastercard internal OTel library (Artifactory/Confluence) later. **Out of scope for this task** — fix the current setup first, but note anything that the internal library would change.

## Symptom
- Metric values (custom and JVM) are visible in the ITF environment through the Spring Boot actuator / Prometheus endpoint.
- **Nothing** appears in Dynatrace, including JVM gauges (heap, GC pauses, CPU).
- No one has yet confirmed whether Dynatrace receives any data, or what response the export returns.

## Known facts (verify each, don't assume)
- Declared dependencies include:
  - `io.opentelemetry:opentelemetry-api`
  - `io.micrometer:micrometer-registry-otlp`
  - `io.micrometer:micrometer-registry-prometheus`
  - **`io.micrometer:micrometer-registry-dynatrace` appears to be absent.**
- Config contains a Dynatrace export block: `management.dynatrace.metrics.export.*` with an export URI, a **device ID**, an **API token**, and `enabled=true`.
- The Dynatrace API token is hardcoded in the ITF app config (`values.yaml`). Its owner, expiry, and scopes are unknown.

## Hypotheses (ranked)
1. **Config/dependency mismatch:** the `management.dynatrace.metrics.export.*` properties only take effect with `micrometer-registry-dynatrace` on the classpath. Without it, Spring Boot silently ignores the whole block.
2. **OTLP registry unconfigured:** `micrometer-registry-otlp` is active but has no `management.otlp.metrics.export.url`, so it pushes to the default `http://localhost:4318/v1/metrics` and the data goes nowhere.
3. **Property prefix vs Spring Boot version:** Spring Boot 2.x uses `management.metrics.export.dynatrace.*`; Spring Boot 3.x uses `management.dynatrace.metrics.export.*`. A prefix that doesn't match the Boot version is ignored.
4. **Deprecated v1 API:** setting a device ID makes the Micrometer Dynatrace registry use the deprecated v1 API instead of v2 (`/api/v2/metrics/ingest`).
5. **Token:** expired, revoked, or missing the `metrics.ingest` scope (returns 401/403).
6. **Temporality:** Dynatrace only accepts **delta** temporality over OTLP; cumulative counters and histograms are dropped (gauges still ingest). This would explain missing counters but not missing JVM gauges.
7. **Network:** egress from ITF to the Dynatrace endpoint is blocked or needs a proxy.
8. **Dashboard query:** data arrives but the DQL uses the wrong metric keys (Dynatrace may add suffixes such as `.count` / `.gauge` and lowercases dimension keys).

## Investigation steps
1. **Versions and dependencies**
   - Report the Spring Boot and Micrometer versions.
   - Run the full dependency tree (`mvn dependency:tree` or `gradle dependencies`) and list every `micrometer-registry-*` and `io.opentelemetry*` artifact, including transitive ones.
2. **Config inventory**
   - Find every `management.*` property across `application*.yml/properties`, profile files, `values.yaml`, and environment overrides. Show the effective values for the ITF profile.
   - Mask any secret in your output (show only the first 4 characters of a token).
3. **Which registries are actually active**
   - From the code and auto-configuration conditions, determine which `MeterRegistry` beans will be created: `OtlpMeterRegistry`, `PrometheusMeterRegistry`, `DynatraceMeterRegistry`.
   - Give the runtime checks to confirm: `/actuator/conditions` (look for `DynatraceMetricsExportAutoConfiguration` and `OtlpMetricsExportAutoConfiguration`, and whether each matched) and `/actuator/beans`.
4. **Draw the export path**
   - State, hop by hop: annotation/aspect → `MeterRegistry` → registry exporter → destination URL. For Prometheus, state whether anything actually scrapes the endpoint.
5. **Export visibility**
   - Provide the logging config to surface exporter results (e.g. DEBUG for `io.micrometer.registry.otlp` and the Dynatrace registry package), and explain what success and failure lines look like.
   - Note that a 200 response can still be a partial success with rejected data points.
6. **Temporality**
   - Report the configured aggregation temporality for any OTLP export. If it's not delta, flag it.
7. **Confirm the root cause**
   - Map the evidence to the hypotheses above. Mark each one as confirmed, ruled out, or unverified (with what is needed to verify it).

## Proposed fix (as a diff, for review)
Recommend **one** export route and remove the unused one, so that config and dependencies match.

- **Option A — OTLP to Dynatrace (preferred, vendor-neutral):**
  - `management.otlp.metrics.export.url` → the Dynatrace OTLP metrics endpoint (`.../api/v2/otlp/v1/metrics`; the exact base URL differs between SaaS, Managed, and ActiveGate, so leave a TODO).
  - `management.otlp.metrics.export.headers.Authorization` → `Api-Token ${DT_API_TOKEN}` loaded from the environment.
  - `management.otlp.metrics.export.aggregation-temporality` → `delta`.
  - Remove the `management.dynatrace.*` block.
- **Option B — Micrometer Dynatrace registry:**
  - Add `io.micrometer:micrometer-registry-dynatrace`.
  - Use the v2 ingest URI (`/api/v2/metrics/ingest`), remove the device ID, and load the token from the environment.
- In both cases: say whether `micrometer-registry-prometheus` should stay (local debugging only) or be removed.

## Secondary review (report only, don't fix)
- **Transaction timing:** do aspects on `@Transactional` methods record metrics before commit, so rollbacks are over-counted? Check aspect ordering and suggest a fix (e.g. record after commit).
- **AOP blind spots:** self-invocation, private methods, or non-Spring-managed objects that the aspects won't intercept on the key flows.
- **Cardinality:** list every metric tag and its possible values. Flag unbounded tags (IDs, file names, raw error messages). Error tags should be a fixed set of error types.
- **Multi-instance gauges:** the idle-pairs gauge, if it comes from a DB query, will be reported by every pod. Flag whether dashboards would sum duplicates.
- **Traces:** `opentelemetry-api` without the OTel SDK or Java agent is a no-op. Confirm that no traces are currently produced.

## Constraints
- Never print, log, or commit token values. Recommend moving the token to Vault / the approved secrets store and rotating the existing one.
- Do not change business logic.
- Do not invent endpoint URLs, environment IDs, or token scopes. Use clearly marked TODOs.
- Keep changes minimal and reviewable.

## Expected output
A single report (`docs/results/dynatrace-export-diagnosis.md`) containing:
1. Versions, dependencies, and effective config (secrets masked).
2. Active registries and the export path diagram.
3. Each hypothesis with status and evidence (file and line references).
4. Root cause, with a confidence level.
5. The recommended fix as a diff.
6. Verification steps after the fix: exporter log shows success; one counter and one JVM gauge visible in a Dynatrace Notebook query; the dashboard DQL uses the correct metric keys.
7. Findings from the secondary review.
