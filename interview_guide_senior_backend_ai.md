# Interview Guide: Senior Backend / AI Engineer
### Candidate: ~6 yrs experience (Tresvista 2018-21, Walmart Benefits 2023-26, Walmart Customer Identity 2026-present)

**How to use this:** Each question has what a strong answer contains and what should worry you. Questions marked **[FILTER]** are the highest-signal ones for separating lived experience from resume keywords. You do not need all of these. Pick 6-8 per round.

**Core interview thesis:** The Java/Spring/Kafka/GCP thread is consistent across all three roles and is probably real. The AI/agent layer and the frontend claims appear suddenly and read as recently added. Test the second hard, and don't let a weak result there disqualify a genuinely strong backend engineer if backend is what you need.

---

## Round 1: Agent tooling and MCP
*This is the newest and least verifiable claim on the resume. It is also the easiest to test, because the details are specific and recent.*

### Q1. [FILTER] Explain MCP at the protocol level. What primitives does a server expose?

**Looking for:** MCP is a client-server protocol built on JSON-RPC 2.0. A server exposes three main primitives:
- **Tools** — model-controlled, executable functions the LLM decides to call
- **Resources** — application-controlled, read-only context (documents, records, schemas) the host application chooses to include
- **Prompts** — user-controlled templates, usually surfaced as slash commands or menu entries

Strong candidates will also mention the `initialize` handshake with capability negotiation, transports (stdio for local, HTTP for remote), and possibly sampling (the server asking the client to run an LLM completion) or roots.

**Red flags:** Describing MCP as "just function calling" or "an API wrapper" with no protocol detail. Confusing it with the OpenAI function-calling schema. Inability to name the primitives. Someone who genuinely built server endpoints seven months ago knows the word "tools" versus "resources."

### Q2. [FILTER] You exposed internal identity APIs as agent-callable tools. Whose identity was the agent acting under?

**Looking for:** Recognition that this is the hard problem. Good answers cover: the agent should not hold a superuser credential; token exchange or on-behalf-of flows so the tool call inherits the calling user's scope; per-tool authorisation rather than per-server; audit logging that records the agent invocation and the human principal separately. For an identity system specifically, the blast radius of a confused-deputy bug is severe.

**Red flags:** "It ran with a service account." That is a real answer many teams give, but a senior candidate should at least name it as a known risk with a mitigation plan.

### Q3. What happened when the model called your tools with malformed or wrong arguments?

**Looking for:** Concrete war stories. JSON schema validation at the boundary; returning structured errors the model can recover from rather than stack traces; the observation that overly terse tool descriptions cause bad calls; tool-count bloat degrading selection accuracy; retry loops that need a hard cap or the agent spins.

**Red flags:** "It didn't really happen." It always happens.

### Q4. How do you decide what becomes a tool versus what stays in the prompt or a resource?

**Looking for:** Tools for actions with side effects or data too large/dynamic to inline. Resources for stable context. Awareness that every additional tool dilutes selection accuracy, so coarse-grained tools that do one meaningful unit of work usually beat many thin CRUD wrappers.

---

## Round 2: RAG, LLM integration, streaming

### Q5. Walk through your RAG pipeline for the identity knowledge base. Chunking through response.

**Looking for:** A real pipeline has opinions at each stage. Chunking strategy and why (fixed-size with overlap, semantic, or structure-aware for docs with headings). Embedding model choice and dimensionality. Retrieval — ideally hybrid, combining BM25/keyword with vector similarity, because pure vector search fails badly on exact identifiers, error codes, and proper nouns. A reranking step. Top-k tuning. Context assembly and citation.

**Red flags:** "We embedded the docs and did cosine similarity." That is a tutorial, not a production pipeline.

### Q6. [FILTER] How did you evaluate whether RAG was actually working?

**Looking for:** This question catches people who demoed rather than shipped. Strong answers mention a labelled eval set, retrieval metrics separate from generation metrics (recall@k and MRR for retrieval; faithfulness/groundedness for generation), regression testing when the index or prompt changes, and human review of a sample. Bonus for noting that retrieval failures and generation failures need different fixes and conflating them wastes weeks.

**Red flags:** "Users said it was good." No offline eval at all.

### Q7. You used pgvector. Which index type, and what did you trade away?

**Looking for:** HNSW versus IVFFlat. HNSW gives better recall-latency but has higher build time and memory; IVFFlat is cheaper to build but needs the lists parameter tuned and needs data present before indexing. Both are approximate, so you are trading recall for speed. Distance operator should match the embedding model's training (cosine for most).

**Red flags:** No awareness that ANN search is approximate at all.

### Q8. Why SSE for streaming instead of WebSockets?

**Looking for:** SSE is unidirectional server-to-client, which is exactly the shape of token streaming; it runs over plain HTTP so it survives proxies and load balancers that mangle WebSocket upgrades; it has built-in reconnection with `Last-Event-ID`. Real-world gotchas worth hearing: intermediate proxies buffering the stream so nothing renders until completion (needs `X-Accel-Buffering: no` or equivalent), the HTTP/1.1 six-connections-per-origin limit, and handling client disconnect so you stop burning tokens on an abandoned request.

**Red flags:** Naming the libraries but no transport-level reasoning.

### Q9. How did you handle a partial failure mid-stream, 300 tokens into a response?

**Looking for:** Honest discussion of an ugly problem. Options include emitting an error event the UI renders inline, buffering server-side to allow a clean retry, or idempotency keys so a retried request doesn't double-charge or double-execute a tool call.

---

## Round 3: Identity resolution and graph
*Most relevant if the role touches customer data unification. The resume puts this work in both Walmart roles, which is worth probing.*

### Q10. [FILTER] Deterministic versus probabilistic matching — what were your actual rules and how did you set the threshold?

**Looking for:** Deterministic: normalise then exact-match on strong identifiers (email lowercased, phone to E.164, government ID). Probabilistic: field-level agreement scores combined into a match weight, classically Fellegi-Sunter with m and u probabilities per field. Crucially, **two** thresholds, not one: auto-merge above the upper bound, auto-reject below the lower, and a human review queue in between. Threshold set from labelled pairs by choosing an acceptable precision, since a false merge is far more costly than a false split.

**Red flags:** A single threshold, or a number with no derivation ("we used 0.8").

### Q11. [FILTER] What is identity collapse and how did you prevent it?

**Looking for:** This is the question that proves someone worked on a real identity system. Transitive closure over pairwise matches means A~B and B~C merges A and C even though A and C were never compared and are not the same person. One shared household phone number can chain thousands of profiles into a single monster cluster. Mitigations: cluster size caps with alerting, connected-component review before commit, weighting shared-use attributes lower, requiring two independent strong signals, and an unmerge path that preserves lineage so a bad merge is reversible.

**Red flags:** Never heard of it, or no unmerge capability. Merges are easy; unmerges are what senior people build.

### Q12. How did you keep the pairwise comparison tractable across millions of records?

**Looking for:** Blocking or candidate generation. Naive all-pairs is O(n²) and impossible at Walmart scale. Blocking keys (e.g. postcode + surname soundex), sorted neighbourhood, LSH/MinHash, or vector-based candidate retrieval. Then the expensive scoring runs only within blocks. Awareness that blocking choice determines your recall ceiling, since anything not co-blocked can never match.

### Q13. Why TigerGraph over Neo4j, or over Postgres with recursive CTEs?

**Looking for:** Honest reasoning. TigerGraph's case is native parallel processing across a distributed cluster and deep multi-hop traversal performance. A good candidate might admit the decision predated them, or that Postgres would have sufficed for two-hop queries and the graph DB was justified by traversal depth or write throughput. Willingness to say "I'd choose differently now" is a positive signal.

**Red flags:** Vendor marketing recited back. No comparison considered.

### Q14. Explain GSQL accumulators.

**Looking for:** Accumulators are GSQL's state mechanism during traversal. Vertex-local (`@`) attach to each vertex; global (`@@`) are single shared values. Types define the aggregation semantics — `SumAccum`, `MaxAccum`, `SetAccum`, `MapAccum`, `OrAccum`. They are what makes GSQL's accumulate-and-traverse model work across parallel execution.

**Red flags:** Cannot explain the `@` versus `@@` distinction. This is GSQL's single most distinctive feature; anyone who wrote real queries knows it.

---

## Round 4: Multi-tenancy and data isolation

### Q15. [FILTER] How was tenant isolation enforced? Walk me down the stack.

**Looking for:** Where the boundary lives: separate databases, separate schemas, or shared tables with a tenant discriminator plus row-level security. For Postgres RLS: `ALTER TABLE ... ENABLE ROW LEVEL SECURITY`, policies referencing a session variable, and `FORCE ROW LEVEL SECURITY` because the table owner bypasses policies by default. That last detail is a strong tell for hands-on experience.

**Red flags:** "We added a tenant_id column and filtered in the service layer." That is one forgotten `WHERE` clause away from a data breach, and a senior candidate should say so unprompted.

### Q16. You have a connection pool. How does the tenant context survive it?

**Looking for:** The classic RLS bug. `SET` persists on the pooled connection and leaks to the next request's tenant. Correct approach is `SET LOCAL` inside an explicit transaction so it resets on commit, or resetting on connection return. Candidates who have been bitten by this answer instantly.

### Q17. How would you test that isolation actually holds?

**Looking for:** Automated cross-tenant probes in CI — seed two tenants, run every endpoint as tenant A, assert zero tenant B rows ever appear. Negative tests as first-class citizens. Ideally a test that fails if a new table is added without an RLS policy.

---

## Round 5: Distributed systems, Kafka, JVM
*The most likely area of genuine strength. Push for depth rather than testing existence.*

### Q18. Consumer group rebalances during a rolling deploy. What happens to in-flight messages, and what did you do about it?

**Looking for:** Stop-the-world rebalancing under the eager assignor pauses the whole group; cooperative sticky assignment reduces that. Static group membership (`group.instance.id`) plus a tuned `session.timeout.ms` avoids rebalancing at all for a quick restart. In-flight messages get redelivered if offsets weren't committed, which is why consumers must be idempotent. `max.poll.interval.ms` versus slow processing is the classic livelock.

### Q19. You claim exactly-once semantics on identity events. Did you actually have it?

**Looking for:** Precision. Kafka transactions give exactly-once within Kafka-to-Kafka processing, but the moment you write to an external database the guarantee is at-least-once plus idempotent writes. Honest answers say "effectively once via idempotency keys" rather than claiming true EOS.

**Red flags:** Confident claims of exactly-once across system boundaries.

### Q20. Sub-second latency on millions of events — what was p99, and where did it break?

**Looking for:** Real numbers with a real bottleneck story. GC pauses, a downstream graph write, partition skew from a hot key, batch size versus latency tuning. The bottleneck detail matters more than the number.

**Red flags:** Only averages. Anyone who ran production talks in percentiles.

### Q21. You cite JVM tuning. What did you change and what did you measure?

**Looking for:** Collector choice and why (G1 default; ZGC or Shenandoah if pause time dominated), heap sizing relative to container limits, and awareness of container-aware JVM flags. Measurement via GC logs and allocation profiling rather than guesswork.

**Red flags:** "I increased the heap size." Often that makes pauses worse.

### Q22. Reactive Spring — where does it stop paying off?

**Looking for:** Reactive helps when you are I/O-bound with high concurrency and the whole chain is non-blocking. One blocking JDBC call inside a WebFlux chain starves the event loop and is worse than plain MVC. Debuggability and stack traces get materially harder. A current-thinking candidate will note that virtual threads have removed much of the reason to reach for reactive in new services.

---

## Round 6: System design (45 min, whiteboard)

### Q23. Design a real-time customer identity resolution platform.

*Deliberately the candidate's stated domain. If they built it, this should be their best round of the day.*

**Probe on:**
- Ingestion from N source systems with different schemas and trust levels
- Candidate generation and blocking strategy
- Match scoring, thresholds, review queue
- Cluster storage, and what happens when a merge decision is later reversed
- Read path SLA — is the resolved identity served from a materialised view or computed live?
- Backfill: a match rule changes and you must re-resolve history without downtime
- Deletion requests propagating across the graph (GDPR/CCPA)

**Strong signals:** Raises reversibility and audit lineage unprompted. Separates the online read path from the offline resolution path. Treats the review queue as a product with a real operational cost, not a footnote.

**Weak signals:** Draws boxes matching the resume bullets with no data-flow reasoning. Cannot say what happens on a bad merge.

---

## Round 7: Ownership and calibration
*These matter more than usual here, because the resume claims roughly thirty substantial accomplishments in a seven-month tenure.*

### Q24. [FILTER] Pick one thing you personally designed end to end. Walk me through an option you rejected and why.

**Looking for:** Depth on one thing beats breadth on ten. The rejected-alternative framing is what separates designers from implementers.

### Q25. [FILTER] What on your resume did you contribute to but wouldn't claim as yours?

**Looking for:** A calibrated, comfortable narrowing. "I was one of four on the graph work; I owned the ingestion side, not the matching algorithm." This is a strong positive signal, not a weakness.

**Red flags:** Defending everything equally. That is the single most informative failure in this interview.

### Q26. Of the languages listed, rank your top two and tell me where the rest really sit.

**Looking for:** Honest tiering. Java/Kotlin deep, Python working, Scala touched on Spark jobs, TypeScript/React functional but not deep, is a perfectly respectable and believable answer.

**Red flags:** "I'm strong in all of them."

### Q27. Your current role started in January. What has actually reached production versus what is prototype?

**Looking for:** A clear line. Seven months is enough to ship meaningful work, and a candid split is expected and fine.

### Q28. Tell me about a production incident you caused.

**Looking for:** Specificity, ownership without theatrics, and a systemic fix rather than "I was more careful afterwards."

### Q29. You've used Copilot heavily. Where has it made your code worse?

**Looking for:** Plausible-but-wrong code passing review, over-confident test generation that tests the implementation rather than the behaviour, subtle API misuse in unfamiliar libraries. Someone who only praises the tool has not used it seriously.

### Q30. You moved from Benefits Tech to Customer Identity in January. Was that your choice?

**Looking for:** Context on motivation and how they ramp into an unfamiliar domain. Also naturally clarifies the overlapping identity/graph bullets across both roles.

---

## Scoring guide

| Area | Weight | Pass bar |
|---|---|---|
| Java / Spring / concurrency | High | Fluent, unprompted depth |
| Kafka / event-driven | High | Real failure stories with numbers |
| System design | High | Reversibility and read/write path separation raised unprompted |
| Identity resolution | Medium-High | Knows identity collapse and two-threshold matching |
| Multi-tenancy / RLS | Medium | Knows the boundary lives below the application layer |
| MCP / agents | Medium | Names the primitives correctly |
| RAG / LLM | Medium | Has an eval story |
| Ownership calibration | High | Narrows claims voluntarily |

**Hire signal:** Strong on backend fundamentals and system design, calibrated about the AI layer, narrows resume claims without prompting.

**No-hire signal:** Defends every bullet, cannot go one level below any resume line, or gives protocol-level answers on MCP that are actually descriptions of OpenAI function calling.
