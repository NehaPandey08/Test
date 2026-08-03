# Interview Script: Lokesh Bathineni
### Light → Heavy, with model answers and gloss-detection cues

How to run this: each section escalates. If he answers a "light" question fluently but stumbles on the very next "heavier" follow-up in the same section, that's the gloss-over signal — the light layer was memorized, the depth wasn't lived.

---

## PART 1: Opening — Tell me about your work experience

**Q1. Walk me through your work experience, in your own words — not resume bullets. What have you actually been building day to day, most recently?**

*What to listen for:* Does he talk in terms of concrete systems/problems ("we had a claims processing service that needed to summarize documents faster") or does he recite the resume's keyword list back at you ("I have extensive experience in Java, Spring Boot, microservices, RAG, Kafka...")? The keyword-list response is your first gloss-over flag.

*Strong answer example:* "At Goldman I've spent the last year mostly on a backend platform that helps internal ops teams query financial documents using natural language — I own the retrieval layer and the agent orchestration that decides which tools to call. Before that at Ascension it was clinical document summarization on top of a fairly standard Spring Boot microservices base."

*Weak/gloss answer:* "I have 12+ years across Java, Spring Boot, microservices, cloud, and AI, working on enterprise-scale banking, healthcare, retail and government applications." (This is literally the resume overview restated — a candidate reciting their own resume from memory rather than describing lived work.)

---

## PART 2: Pick one module, go end-to-end (depth vs. gloss test)

**Q2. Pick one module or feature you built recently that you're most proud of. Walk me through it end to end — the problem, your design choices, and what you'd do differently now.**

*Why this works:* Let *him* pick. If the depth is real, he'll gravitate to something he actually owns and the story will have texture — false starts, tradeoffs, a mistake. If it's fabricated/templated, the answer stays high-level no matter which module he picks, because there's no real memory underneath it.

*Strong answer example:* "I built the document ingestion and retrieval pipeline for the Goldman internal knowledge assistant. Problem was ops teams manually searching PDFs. I chunked documents at ~500 tokens with 50-token overlap after testing a few sizes, used [specific embedding model] because it was already approved by our security team, stored in Pinecone with metadata filters for document type and date, and the tricky part was tuning the retrieval to not pull in outdated policy docs — we ended up adding a recency boost on top of pure cosine similarity."

*Weak/gloss answer:* "I designed a RAG-based document retrieval system using LangChain and vector databases to enable semantic search across enterprise documents, improving query response time and accuracy." (Sounds like resume-bullet phrasing spoken aloud — no numbers, no named tradeoff, no mistake.)

**Follow-ups to escalate within this module (don't skip these — this is where gloss-over gets exposed):**

- "What was the actual chunk size and why that number?"
- "What broke the first version of this? What did you have to redo?"
- "If I gave you 10x the document volume tomorrow, what breaks first?"
- "Who else on the team owned parts of this, and what exactly was your piece vs theirs?"

*A candidate who genuinely built it answers all four without hesitation, often with a number or a name. A candidate who observed it or inflated their role starts hedging ("the team decided," "it was more of a collaborative effort") once you ask for specifics.*

---

## PART 3: Authentication / Authorization (light → heavy)

**Q3 (light). In your own words, what's the difference between authentication and authorization, and where did you implement this most recently?**

*Strong:* Quick, confident distinction (who you are vs. what you can do), immediately grounds it in a real system — "we used Spring Security with JWT for auth, and RBAC via custom annotations for authorization on top of it."

*Weak:* Correct definition, but can't name what they actually used, or gives a textbook answer with no system attached.

**Q4 (medium). Which OAuth2 grant type did you implement, and why that one and not another?**

*Strong:* Names it specifically — authorization code with PKCE for user-facing apps, client credentials for service-to-service calls — and explains *why*, e.g. implicit grant is deprecated/insecure, PKCE protects against interception on public clients.

*Weak:* "We used OAuth2 for security" — no grant type named. This is the single fastest tell for auth questions; anyone who's actually configured Spring Security OAuth2 has an opinion on grant type.

**Q5 (heavier). Where did you actually store and validate the JWT — was validation happening at the gateway, or in each service? What's in the token payload, and how did you handle token revocation before expiry?**

*Strong:* Concrete architecture answer — e.g., validation at Spring Cloud Gateway with a filter, claims like roles/tenant ID in the payload, and acknowledges revocation is the hard part with JWTs (short expiry + refresh tokens, or a blocklist/Redis check as a compromise since JWTs are stateless by design).

*Weak:* Doesn't know revocation is even a problem with JWTs — treats them as if they can just be "invalidated" like a session, which reveals they haven't actually operated one in production.

**Q6 (heaviest). Tell me about a real security bug or near-miss you caught or caused around authN/authZ — not a hypothetical, something that actually happened.**

*Strong:* Has a specific story — an endpoint missing `@PreAuthorize`, a token accidentally logged in plaintext, CSRF protection disabled during testing and left off, a role check that only validated on the frontend.

*Weak:* No story, or a generic "we always follow best practices" non-answer. Real production security work almost always has at least one "we caught this in review before it shipped" or "this got through and we had to patch it" story.

---

## PART 4: Move to Kafka (light → heavy)

**Q7 (light). Where did you use Kafka, and what was flowing through the topics?**

*Strong:* Names actual topics/events — "order-created," "payment-confirmed" — and the producer/consumer services involved.

*Weak:* "We used Kafka for event-driven architecture and asynchronous communication" — the resume phrase, no actual topic or message content named.

**Q8 (medium). How did you handle message ordering and partitioning? What was your partition key and why?**

*Strong:* Explains the tradeoff — same partition key (e.g., order ID) guarantees ordering per key but means all messages for that key hit one partition, discusses partition count decisions relative to consumer parallelism.

*Weak:* Doesn't know what a partition key is or why it matters — treats Kafka as a generic message queue with no distinction from RabbitMQ/SQS.

**Q9 (heavier). What happened when a consumer failed halfway through processing a batch — how did you avoid double-processing or message loss? Walk me through your offset commit strategy.**

*Strong:* Concrete answer on manual vs auto-commit, at-least-once vs exactly-once semantics, idempotent consumer design (e.g., dedupe by message ID, idempotency keys in the DB), dead-letter topic for poison messages.

*Weak:* Assumes Kafka guarantees exactly-once by default, or has never thought about the failure case at all — big red flag for someone claiming "event-driven Kafka processing" as core expertise across 6 jobs.

**Q10 (heaviest). Describe a real Kafka production incident — consumer lag spike, rebalancing storm, or a broker issue — and how you diagnosed and fixed it.**

*Strong:* Names actual tools (Kafka consumer lag monitoring, `kafka-consumer-groups.sh`, or a dashboard), a real root cause (slow downstream call blocking the consumer thread, too-frequent rebalances from short session timeout, etc.).

*Weak:* No incident story — this is the AI/RAG pattern repeating: broad claims, zero operational scar tissue.

---

## PART 5: Move to RAG (light → heavy — the highest-value section given the resume)

**Q11 (light). What does RAG actually solve that a fine-tuned model or plain prompting doesn't?**

*Strong:* Clear, own-words explanation — RAG lets you ground answers in current/proprietary data without retraining the model, reduces hallucination on domain-specific facts, and is cheaper/faster to update than fine-tuning.

*Weak:* Circular definition ("RAG retrieves relevant documents and augments the prompt with them") with no sense of *why* that's the right tool vs alternatives.

**Q12 (light-medium). Walk me through your actual chunking strategy — chunk size, overlap, and why those numbers.**

*Strong:* Specific numbers with reasoning (e.g., "500 tokens with 50 overlap, because our source docs were policy PDFs with dense paragraphs and going smaller fragmented context across sentences").

*Weak:* "We chunked the documents and stored embeddings" — no numbers, no reasoning. This is the exact gap in the current resume language.

**Q13 (medium). Which embedding model did you use, and what would make you switch to a different one?**

*Strong:* Names a model, and has real tradeoff criteria — dimension size vs storage cost, latency, whether it's multilingual, domain fit (e.g., a general-purpose embedding model underperforming on financial jargon).

*Weak:* Can't name the embedding model actually used, or thinks "embedding model" and "vector database" are the same thing.

**Q14 (medium-heavy). How did you evaluate whether retrieval was actually *good* — not just "it returned something," but measurably relevant?**

*Strong:* Has an actual method — a golden set of question/answer pairs with expected source docs, precision@k, human eval on a sample, or at minimum a documented process for catching bad retrievals before they shipped.

*Weak:* No evaluation method at all — "it worked well in testing" with nothing concrete. Given how much the resume leans on RAG across 6 different jobs, the absence of any evaluation methodology is a serious tell.

**Q15 (heavy). Tell me about a time RAG retrieval pulled in the *wrong* context and the model confidently gave a wrong answer. How did you catch it and what did you change?**

*Strong:* A real failure story — stale document ranked highly, near-duplicate chunks confusing retrieval, or a query that needed multi-hop reasoning across chunks that pure similarity search couldn't handle — and a concrete fix (metadata filtering, reranking step, hybrid search).

*Weak:* No failure story. RAG in production always breaks in interesting ways early on; someone who's genuinely built and operated it will have a war story ready immediately.

**Q16 (heaviest). Describe how your agent decided *when* to retrieve vs when to answer directly, and what happened when the agent called a tool with a malformed or hallucinated argument.**

*Strong:* Discusses routing logic (a classifier or prompt-based decision on retrieval necessity), structured output validation (JSON schema enforcement) before executing a tool call, and a guardrail for bad/hallucinated arguments (retry with error feedback, fallback to human-in-the-loop, max iteration cap to avoid loops).

*Weak:* Never considered this — assumes the agent "just works," which is the single strongest signal that the extensive "Agentic AI" claims across the resume are inflated or borrowed language rather than lived engineering.

---

## PART 6: Personality-based questions

**Q17. Tell me about a technical disagreement you had with a teammate or lead. How was it resolved, and were you right?**

*Strong:* Specific disagreement, genuine self-awareness about being wrong at least once, describes how it was actually resolved (data, a spike/prototype, escalation, compromise) rather than "we just talked it out and agreed."

*Weak:* Can't recall a single disagreement in 12 years, or one where they were "obviously right" with no nuance — suggests either low engagement with peers or an unwillingness to show vulnerability.

**Q18. What's something you shipped that you now think was the wrong call in hindsight?**

*Strong:* A real regret with a concrete lesson — e.g., over-engineering a CQRS setup for a simple CRUD service, or picking a vector DB that turned out to be the wrong fit for their scale/cost profile.

*Weak:* "I can't think of anything" or a humble-brag disguised as a flaw ("I work too hard").

**Q19. How do you stay current given how fast the AI/agent tooling space is moving right now? What have you changed your mind about in the last 6 months?**

*Strong:* Names something specific they've updated their view on — a framework they dropped, an approach that stopped scaling, a paper or release that changed their architecture.

*Weak:* Generic "I read blogs and follow AI news" with nothing concrete they've actually changed based on it.

**Q20. Describe the worst production incident you were personally responsible for. Walk me through the 3am version of that story.**

*Strong:* A specific, slightly uncomfortable story told with ownership — what broke, what they did in the moment, what changed afterward (a new alert, a process change, a postmortem action item).

*Weak:* Deflects blame entirely to "legacy code" or "another team," or claims to have never caused a real incident in 12 years — implausible at senior level and a values/honesty flag, not just a skills flag.

---

## Quick scoring guide

| Section | Real depth looks like | Gloss-over looks like |
|---|---|---|
| Work experience | Names a real problem, not the resume overview | Recites keyword list |
| Module deep-dive | Numbers, a mistake, a redo | Stays at "designed a system that improved X" |
| Auth/AuthZ | Names grant type, revocation strategy | "We used OAuth2 and JWT" with no mechanics |
| Kafka | Partition key logic, failure handling, an incident | "Used Kafka for event-driven architecture" |
| RAG | Chunk size + reasoning, eval method, a real failure | Vector DB name-dropping, no numbers |
| Personality | Owns a mistake/disagreement/incident specifically | Claims a flawless 12-year record |

Recommend weighting Part 5 (RAG) and Part 4 (Kafka) heaviest in the actual interview — these are the areas where the resume's language is most repeated verbatim across unrelated jobs, so real vs. borrowed depth will separate fastest there.
