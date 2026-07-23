# Brief: Generate a Full-Scale Production Release Plan for ModelHub

## Role and Objective

You are acting as a release engineer and solution architect for ModelHub, a payment message standards platform covering ISO 8583, ISO 20022, and NCM/Nexo. Your task is to analyse the codebase, existing release documents, and any reference files provided, and produce a complete, execution-ready production release plan. Do not produce a generic template. Every section must be grounded in what you find in the actual code and configuration.

## Context

ModelHub uses a two-tier data model: a Data Dictionary layer (shared element definitions) and a Message-level properties layer (message definitions, mappings, and message-specific overrides). The release includes backend services, frontend, database changes, and reference data reseeding for the Data Dictionary, Messages, and Mappings.

Reference materials provided alongside this brief may include: prior release plans, deployment runbooks, environment config files, DB scripts, and repository access. Read them before writing anything. Where a prior release plan exists, identify what it covered, what it missed, and what has changed since.

## Phase 1: Discovery and Artifact Inventory

Before writing the plan, analyse the repositories and produce a complete inventory of everything that will go to production. For each artifact, state: name, repo and path, type, version or tag, build pipeline that produces it, and target environment/infrastructure.

Cover at minimum:

1. **Backend services**: every deployable service or API, its runtime, its build artifact (jar, container image, etc.), and its deployment target (PCF, AWS, Kubernetes, etc.). Identify inter-service dependencies and required deployment order.
2. **Frontend**: build artifacts, static asset hosting, CDN or app server target, cache-busting strategy, API base URL configuration per environment.
3. **Database**:
   - Schema changes: enumerate every migration script (Flyway/Liquibase/manual SQL) that must run, in order. Flag any destructive change (drop, rename, type change) explicitly.
   - Reference data reseeding for Data Dictionary, Messages, and Mappings: locate the seed scripts or data load mechanism in the code. For each dataset, explicitly evaluate **rewrite (truncate and reload) versus incremental update (upsert/delta)** and recommend one, with reasoning based on: whether user-created or runtime data lives in the same tables, foreign key dependencies between Data Dictionary and Message/Mapping tables, table sizes, downtime tolerance, and referential integrity risk. If rewrite is chosen, define the exact truncation order and reload order respecting FK constraints. If update is chosen, define the delta identification method and conflict handling.
   - Sequences, indexes, triggers, or stored procedures affected.
4. **Configuration**: every config change required, per environment. Enumerate application properties, environment variables, feature flags, secrets (names only, never values), connection strings, and external endpoint references. Diff current prod config against what the new release expects and list every delta.
5. **Infrastructure and platform**: any changes to service bindings, scaling parameters, routes, network policies, certificates, or messaging infrastructure (Kafka topics, queues, schemas) that the release depends on.
6. **Third-party and internal dependencies**: upstream/downstream systems that consume or feed ModelHub. Identify any contract changes (API versions, message formats, event schemas) that require coordination.

Output of Phase 1: a single artifact inventory table plus a dependency graph (text form is fine) showing deployment order constraints.

## Phase 2: The Release Plan

Using the Phase 1 inventory, write the release plan with these sections:

### 1. Release Summary
Scope, features included, features explicitly excluded, target release date, release version/tags per repo.

### 2. Pre-Release Checklist
Code freeze date, branch/tag strategy, required approvals, completed test evidence (unit, integration, regression, UAT sign-off), performance baseline, security scan status, change ticket references.

### 3. Environment Promotion Path
The exact sequence (e.g. dev, stage, prod) with validation gates at each stage. State what must pass before promotion.

### 4. Database Execution Plan
Step-by-step DB runbook: pre-checks, backup point, schema migration order, reseed execution (per the rewrite vs update decision from Phase 1), post-load validation queries with expected row counts or checksums for Data Dictionary, Messages, and Mappings tables. Include estimated execution time per step and whether the application must be down or can stay up.

### 5. Deployment Sequence
Ordered steps across backend, frontend, and config, with the specific commands or pipeline jobs, who executes each step, and expected duration. State whether the deployment is blue-green, rolling, or big-bang, and justify based on what the platform supports. Identify any step where downtime occurs and its expected window.

### 6. Backup and Rollback Plan
This section must be concrete:
- What is backed up before the release (DB dump/snapshot, current artifact versions, current config), where backups are stored, and how long restore takes.
- Rollback procedure per artifact type: how to revert backend (redeploy previous version), frontend, config, and DB. For DB, distinguish rollback of schema changes from rollback of reseeded data, and state explicitly whether reseed rollback means restoring from backup or re-running the previous seed set. Identify any migration that is not reversible and the mitigation for it.
- Rollback decision criteria: who decides, based on what signals, and the point of no return after which roll-forward is the only option.

### 7. Validation and Smoke Tests
Post-deployment verification: health checks per service, critical user journeys on the frontend, API smoke tests, DB data validation queries, and message parsing/mapping verification for at least one message per standard (ISO 8583, ISO 20022, NCM/Nexo). Include expected results.

### 8. Monitoring and Hypercare
What to watch in the first 24 to 72 hours: logs, dashboards, error rates, queue depths, and who is on point.

### 9. Communication Plan
Stakeholder notifications, downtime announcements if any, and the escalation chain.

### 10. Risks and Mitigations
Ranked list of release-specific risks derived from the actual analysis (not generic risks), each with likelihood, impact, and mitigation. Pay specific attention to: reseed data integrity, FK violations during reload, config drift between environments, and dependency on external systems.

### 11. Timeline
A minute-level or hour-level schedule for release day, with owners per step and go/no-go checkpoints.

## Working Rules

- Read the code before asserting anything. If you claim a migration exists or a config key is needed, cite the file path.
- Where information is genuinely unavailable in the provided materials, list it under an "Open Questions / Inputs Needed" section rather than inventing it. Keep this list short and specific.
- Flag anything in the codebase that looks like a release blocker (hardcoded environment values, missing migrations, untagged artifacts, config referenced in code but absent from config files).
- Output format: a single markdown document. Use tables for the artifact inventory, config deltas, and timeline. Keep prose tight.
- Do not include placeholder text like "TBD" without stating exactly what input is needed and from whom.
