# 30-Minute Screening — 7 YOE Candidate
## Ansari Mohammed Taqiuddin

~3-4 min per question, 8 questions, leaves buffer for intro/wrap-up.

---

**1. Ownership check (ask this first)**
Q: Pick one bullet from your resume and go deep — what exactly did *you* build vs. what did the team build?
A: (Open-ended) Look for specific technical detail volunteered unprompted — exact tools, decisions, tradeoffs, problems hit. A generic restatement of the bullet is a red flag, especially given the near-identical phrasing across all three jobs.

---

**2. RAG pipeline (their strongest claimed skill)**
Q: Walk me through a RAG pipeline you built end-to-end — chunking strategy, vector DB, and how you evaluated retrieval quality.
A: Good answer covers: chunking approach (fixed-size vs semantic, overlap), embedding model, vector DB choice and why (Pinecone, pgvector, FAISS, etc.), retrieval method (top-k, hybrid search), and an evaluation method (relevance scoring, hallucination rate, human eval).

---

**3. Spring Boot / transactions**
Q: What does `@Transactional` actually do, and what's a common mistake people make with it?
A: Wraps the method in a proxy that commits on success / rolls back on runtime exception. Common gotcha: calling a `@Transactional` method from within the same class (self-invocation) bypasses the proxy, so it silently doesn't work.

---

**4. Kafka**
Q: How do you guarantee message ordering when a topic has multiple partitions?
A: Kafka only guarantees order within a single partition, not across the topic. You key messages by an identifier (e.g. customer ID) so related events always land on the same partition.

---

**5. Production judgment**
Q: How do you handle a case where the LLM hallucinates or returns a bad response in production?
A: Ground responses in retrieved source docs (RAG) rather than free generation, validate outputs against source, add fallback/low-confidence handling, and human review for high-stakes cases.

---

**6. Cloud tradeoffs**
Q: EC2 vs Lambda vs ECS — how do you decide which to use for a given service?
A: EC2 = full control, more ops overhead, good for long-running stateful services. Lambda = event-driven, short-lived, no server management, cold-start tradeoff. ECS/Fargate = containerized apps needing to scale without managing raw instances. Decision hinges on traffic pattern, statefulness, cold-start tolerance.

---

**7. Debugging under pressure (behavioral)**
Q: Tell me about a hard-to-track-down production bug — how did you find it?
A: (Open-ended) Look for a clear method: reproduce, narrow scope with logs/traces, form and test a hypothesis. Vague "I just fixed it" answers suggest limited hands-on debugging experience.

---

**8. Cost/scale awareness**
Q: How do you manage LLM API costs and rate limits in a high-traffic service?
A: Caching repeat queries, batching, tiering models (cheap model for simple tasks, larger model reserved for complex ones), usage monitoring/alerts, per-user rate limits.

---

*Weight your read most heavily on Q1 and Q2 — they're the fastest signal on whether the AI/ML claims (the resume's core sell) are real hands-on work or template language.*
