# SAIРЕДDY INTERVIEW QUESTION BANK
## Senior Full Stack AI Engineer | Wells Fargo / Walgreens

**Candidate:** Sai Reddy | **Roles:** Senior Full Stack AI Engineer (Wells Fargo, Walgreens, Walmart, UPS)  
**Interview Date:** ___________  
**Panel Members:** ___________  
**Resume Focus:** Spec-First Development, RAG/Semantic Search (2023+), AI Agent Orchestration

---

# QUICK INDEX (JUMP TO SECTION)

## Section 1: Spec-First Workflow
- [1.1 Primary](#11-primary-spec-first) | [1.2 Artifact Deep-Dive](#12-artifact-deep-dive) | [1.3 Scope Challenge](#13-scope-challenge)
- [1.4 Team Adoption](#14-team-adoption) | [1.5 Failure Case](#15-failure-case) | [1.6 AWS Integration](#16-aws-integration)

## Section 2: RAG Evaluation & Hallucination
- [2.1 Primary](#21-primary-production-hallucination) | [2.2 Precision/Recall](#22-precisionrecall-target) | [2.3 Retrieval Failure](#23-retrieval-failure)
- [2.4 System Prompt](#24-system-prompt-tradeoff) | [2.5 LangSmith](#25-langsmith-monitoring) | [2.6 Eval Cost](#26-cost-of-evaluation)
- [2.7 Chunking](#27-chunking-strategy-size--strategy) | [2.8 Embedding Model](#28-embedding-model-choice) | [2.9 Hybrid Search](#29-hybrid-search)
- [2.10 Bedrock](#210-aws-niche-bedrock--rag) | [2.11 Context Window](#211-context-window-limits) | [2.12 Vector Store](#212-vector-store-selection)
- [2.13 Embedding Dimensionality](#213-embedding-dimensionality) | [2.14 Semantic vs BM25](#214-semantic-vs-keyword-search-bm25)
- [2.15 Metadata Filtering](#215-metadata-filtering) | [2.16 Re-ranking](#216-re-ranking-strategy) | [2.17 Cold Start](#217-cold-start-problem)
- [2.18 Query Expansion](#218-query-expansion) | [2.19 Staleness](#219-staleness--updates) | [2.20 Embedding Model Deep](#220-embedding-model-selection-deep)
- [2.21 Hallucination Type](#221-hallucination-mitigation-retrieval-vs-prompt) | [2.22 Multi-Modal](#222-multi-modal-rag) | [2.23 Cost Breakdown](#223-cost-breakdown)
- [2.24 Eval Methodology](#224-evaluation-methodology)

## Section 3: Agent Design Decisions
- [3.1 Primary](#31-primary-rules--agent) | [3.2 Tool Calling](#32-tool-calling-implementation) | [3.3 Reasoning Chain](#33-reasoning-chain-length)
- [3.4 Failure Mode](#34-failure-mode) | [3.5 Deterministic](#35-deterministic-vs-probabilistic) | [3.6 Cost Comparison](#36-cost-comparison)
- [3.7 Latency Tradeoff](#37-latency-tradeoff) | [3.8 Framework Choice](#38-agent-framework-choice) | [3.9 Lambda Timeout](#39-aws-niche-lambda--agent)
- [3.10 Model Selection](#310-model-selection-for-agents)

## Section 4: RAG/Semantic Search Deep Grilling (Resume Saturation 2023+)
- [4.1 Vector Store Selection](#41-vector-store-selection) | [4.2 Embedding Dimensionality](#42-embedding-dimensionality) | [4.3 Semantic vs BM25](#43-semantic-vs-keyword-search-bm25)
- [4.4 Chunking Strategy](#44-document-chunking-size--strategy) | [4.5 Metadata Filtering](#45-metadata-filtering) | [4.6 Re-ranking](#46-re-ranking-strategy)
- [4.7 Cold Start](#47-cold-start-problem) | [4.8 Query Expansion](#48-query-expansion) | [4.9 Staleness](#49-staleness--updates)
- [4.10 Embedding Model](#410-embedding-model-selection-deep) | [4.11 Hallucination Type](#411-hallucination-mitigation-retrieval-vs-prompt)
- [4.12 Multi-Modal](#412-multi-modal-rag) | [4.13 RAG + Agent](#413-rag--agent-interaction) | [4.14 Cost Breakdown](#414-cost-breakdown)
- [4.15 Eval Methodology](#415-evaluation-methodology)

---

---

# SECTION 1: SPEC-FIRST WORKFLOW

## 1.1 Primary: Spec-First

**QUESTION:**
> Walk me through a specific feature you shipped using /specify → /plan → /tasks. What did the actual output of each phase look like, and what decision changed because you had written the spec first instead of just starting to code?

**Expected Strong Answer:**
> "At Walgreens, we were building a drug interaction checker. In /specify, I wrote that the agent needed to:
> - Accept patient medication list as input
> - Query the FDA database for interactions
> - Return severity levels (minor, moderate, severe)
> - Flag any contraindications
> 
> In /plan, we identified that the naive approach (query FDA for every pair of meds) would timeout at 5+ medications. So the plan called for:
> - Pre-compute interaction matrix for top 500 drugs
> - Fall back to LLM semantic search for rare drugs
> 
> The /tasks broke that into: build schema, seed matrix, write retriever, test fallback edge cases.
> 
> What actually changed: without the spec, the initial PR just queried FDA directly. The spec forced us to talk through the scale problem before writing code. Saved maybe 2 weeks of refactoring."

**Padding Answer (Red Flag):**
> "We used GitHub Spec Kit to write detailed specs. The /specify command helped us clarify requirements, /plan broke down the architecture, and /tasks generated action items. It reduced scope creep by improving team alignment."

**Scoring:** Real Expert (9-10) vs. Padding (5-6)

**Red Flags to Listen For:**
- Vague language ("it improved alignment")
- No specific artifact or decision
- No mention of a real constraint discovered
- Sounds rehearsed/marketing-speak

**Green Flags:**
- Specific spec content
- Concrete constraint mentioned (scale, latency, cost)
- Decision changed based on spec
- Quantified impact (weeks saved)

---

## 1.2 Artifact Deep-Dive

**QUESTION:**
> Show me an example spec you wrote. What did you put in the Acceptance Criteria section? Did you ever disagree with your PM on the spec text itself, if so, what was the disagreement and how did you resolve it?

**Expected Strong Answer:**
> "I don't have it right here, but the acceptance criteria looked like:
> - 'Given a patient on warfarin and ibuprofen, system must return severity level = MAJOR'
> - 'System must respond in <500ms for up to 10 medications'
> - 'System must not hallucinate interactions not in FDA database'
> 
> The PM wanted to include 'System must handle drug synonyms (e.g., acetaminophen = Tylenol)' but that would've required building a synonym resolver. I pushed back: 'That's out of scope for v1; let's do exact matches first.' We compromised: built a lookup table for top 50 OTC drugs, deferred the rest."

**Padding Answer (Red Flag):**
> "I don't remember the exact criteria, but we made sure to align with the team on requirements."

**Follow-Ups to This Answer:**
1. *"Did your PM ever reject part of the spec you wrote? If so, why and how did you resolve it?"*
2. *"Walk me through the Acceptance Criteria for the edge case: what happens if the system can't find a drug in the database?"*
3. *"Who owned the spec document—you alone, or collaborative with PM and tech lead?"*

**Scoring Adjustment:**
- Specific AC examples + real disagreement story = 9/10 (Real Expert)
- Vague memory + no disagreement = 5/10 (Padding)
- Can't recall any spec structure = 2/10 (Fabrication)

---

## 1.3 Scope Challenge

**QUESTION:**
> You claim this reduced scope creep. Give me a concrete example: what was a scope item the team wanted to add during development that you *rejected* because it wasn't in the spec? Why did you say no, and did the PM agree?

**Expected Strong Answer:**
> "Sprint 2, QA raised: 'Can we detect drug interactions from compound medications (e.g., cold medicine with 5 active ingredients)?' It's a real need, but the spec only covered single-drug queries. I said, 'That's a new requirement, not in our spec. It's a 3-sprint effort (decompose compound meds, run matrix lookups, aggregate results). Let's track it as a future epic.' PM agreed because we had a laser-focused delivery date (Sept 15)."

**Padding Answer:**
> "We had good scope discipline because we used specs."

**Follow-Ups:**
1. *"Did that scope rejection cause friction with the team or PM? How did you handle it?"*
2. *"What was the delivery date you were targeting? Why was it critical?"*
3. *"Did that feature (compound drug interactions) ever get built after v1?"*

**Scoring:**
- Specific rejected feature + date + PM agreement = 9/10
- Vague reference to "scope discipline" = 4/10

---

## 1.4 Team Adoption

**QUESTION:**
> You mention 'established spec-first workflow adoption across pharmacy/banking engineering teams.' What percentage of your team was actually using /specify → /plan → /tasks by the time you left? What was the adoption curve? Who resisted, and what did you do?

**Expected Strong Answer:**
> "At Walgreens, I had 6 engineers. By month 3, 5/6 were using the workflow (one contractor resistant). The curve looked like:
> - Month 1: 20% (just me)
> - Month 2: 50% (2 devs tried it on features)
> - Month 3: 80% (most features spec'd first)
> - Month 6: 85% (one dev still writing specs after code)
> 
> The contractor said, 'I code faster without writing specs.' I didn't force it, but showed him a comparison: his feature took 40 hours including refactoring; the spec-first feature took 30 hours total (10 spec, 20 code). He came around. Wells Fargo: different story. It's a big org, lots of legacy inertia. I influenced 2 squads (12 engineers) to try spec-first, but didn't see org-wide adoption."

**Padding Answer:**
> "I established spec-first workflows and the team adopted them as engineering best practices."

**Red Flags:**
- No numbers
- No names of resisters or how conflict resolved
- Sounds like marketing copy

**Follow-Ups:**
1. *"Who was the one person who resisted at Walgreens? What specifically did you do to convince them?"*
2. *"At Wells Fargo, why did large-scale adoption not happen? Was it leadership support, tool friction, or culture?"*
3. *"If you were to do team adoption again, what would you do differently?"*

---

## 1.5 Failure Case

**QUESTION:**
> Tell me about a time the spec-first approach *failed* or slowed you down. When would you *not* use this workflow?

**Expected Strong Answer:**
> "Spike work doesn't benefit from spec-first. One time we needed to evaluate 3 different vector databases (Pinecone, pgvector, Weaviate) in 2 days. Writing detailed specs for each would've delayed the decision. We did POC code instead. Also, very small bugs or refactoring: 'Fix typo in variable name' doesn't need a /specify.
> 
> And honestly, for well-defined, repetitive work (e.g., 'Add pagination to endpoint X'), spec-first felt like overhead."

**Padding Answer:**
> "Spec-first is always beneficial and improves outcomes."

**Red Flags:**
- No failure story or limitation admitted
- Overconfidence in methodology

**Follow-Ups:**
1. *"How did the POC evaluation change based on what you learned vs. the plan?"*
2. *"Did you ever ship a feature where you skipped the spec and regretted it? What happened?"*

---

## 1.6 AWS Integration

**QUESTION:**
> In that feature (drug interaction checker), you mentioned Lambda and API Gateway. How did your /plan phase account for Lambda cold starts and API throttling? Did the spec change based on those constraints?

**Expected Strong Answer:**
> "The initial /specify didn't mention infrastructure. But in /plan, I added a 'Technical Constraints' section:
> - Lambda: 15-minute timeout (agent workflows fit within that)
> - Cold start: 3-5 seconds if idle (acceptable for async calls, not for sync)
> - API Gateway: 10K request/second burst, 5K sustained
> 
> This forced us to decide: should the interaction checker be sync (fast response) or async (tolerates cold starts)? We chose async with SQS queue for reliability. The spec would've been different if I'd ignored infra constraints."

**Padding Answer:**
> "We deployed on AWS Lambda and API Gateway for scalability."

**Follow-Ups:**
1. *"How did you handle the 3-5 second cold start? Did users wait, or did you pre-warm Lambda?"*
2. *"Did the API Gateway throttle ever hit you in production? What did you do?"*
3. *"If you had to redesign today, would you use Lambda, EC2, or something else? Why?"*

---

---

# SECTION 2: RAG EVALUATION & HALLUCINATION

## 2.1 Primary: Production Hallucination

**QUESTION:**
> Tell me about a time your RAG pipeline hallucinated or returned wrong information in production. Walk me through: (a) how you detected it, (b) what you think caused it, (c) what you changed, and (d) how you measured the fix actually worked.

**Expected Strong Answer:**
> "At Walgreens, the drug interaction checker sometimes said 'Warfarin + Ibuprofen is safe' when it's actually a major interaction. We caught it in UAT because a pharmacist flagged it.
> 
> Root cause: our RAG retriever was pulling a general article about 'drug interactions' that mentioned both drugs but in unrelated paragraphs. The LLM stitched them together and concluded they were compatible.
> 
> We fixed it in three ways:
> 1. Added a reranker step: after retrieval, use a cross-encoder to score chunks by relevance to the specific query.
> 2. Changed our system prompt to say: 'If you do not find a direct interaction listed, respond "Unknown" rather than inferring.'
> 3. Added a validation step: after the LLM response, check if the stated interaction exists in our source documents.
> 
> We measured it with a test set of 200 known drug interactions (half real, half noise). Before: 73% precision. After: 94% precision. We also set up LangSmith to log every query and manually reviewed 5% monthly."

**Padding Answer:**
> "We integrated LangSmith for LLM observability and used prompt engineering to reduce hallucinations. Datadog dashboards monitored token usage and response quality."

**Scoring Rubric:**
- Specific hallucination example = +3 points
- Identified root cause (retrieval vs. generation) = +2 points
- Multi-layered fix = +2 points
- Quantified before/after metric = +2 points
- Ongoing monitoring plan = +1 point
- **Total: 9-10 = Real Expert**

**Red Flags:**
- No specific example
- "We reduced hallucinations" (vague)
- No metrics or measurement
- No root cause identified

**Follow-Up Questions:**
1. *"That 73% precision baseline—how did you measure that initially? Did you have labeled data?"*
2. *"The validation step: if the LLM's claimed interaction doesn't exist in source docs, what do you do? Reject the response or mark it as uncertain?"*
3. *"In that 200-test-case set, were the 'false' interactions realistic (things the LLM might plausibly invent)?"*

---

## 2.2 Precision/Recall Target

**QUESTION:**
> What was your precision/recall target for the drug interaction checker (or similar RAG system)? How did you decide on those numbers? Did you ever have to choose between precision and recall, and which did you optimize for?

**Expected Strong Answer:**
> "For drug interactions, we optimized for **high precision, acceptable recall**.
> 
> Target: Precision ≥ 95%, Recall ≥ 75%
> 
> Why precision? Missing a non-existent interaction (false positive) could kill a patient. Missing a real interaction (false negative) is bad but less immediately dangerous—the pharmacist can look it up manually.
> 
> In practice, we achieved Precision 94%, Recall 68% in v1. The 7% recall gap (missing ~14 of 200 known interactions) was acceptable because the fallback is manual FDA lookup, which is standard practice anyway."

**Padding Answer:**
> "We aimed for high accuracy in our RAG system."

**Scoring:**
- Specific precision/recall targets = +2
- Justification for trade-off choice = +2
- Actual achieved metrics = +3
- Explanation of acceptable gap = +2
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"How did you measure recall? Did you have a gold-standard dataset of all possible drug interactions?"*
2. *"That 68% recall—which interactions were you missing? Were they edge cases or common ones?"*
3. *"Did precision or recall ever cause a production incident?"*

---

## 2.3 Retrieval Failure

**QUESTION:**
> Describe a retrieval failure: a question where your system pulled the wrong document chunks and the LLM answered incorrectly as a result. What caused the retrieval to fail? How did you fix it? Did you switch embedding models, add reranking, or change your chunking strategy?

**Expected Strong Answer:**
> "Query: 'Can I take ibuprofen with enalapril (ACE inhibitor)?'
> 
> Retriever pulled: an old review article mentioning both drugs in unrelated sentences. LLM invented a plausible-sounding interaction.
> 
> Root cause: the embedding similarity was high (both drugs mentioned in article), but the *context* was wrong (different sections).
> 
> Fix: Tried three approaches:
> 1. Increased similarity threshold (0.75 → 0.85). Helped but missed some valid matches.
> 2. Added BM25 hybrid search. Dramatically improved—now only exact drug mention = retrieval.
> 3. Added paragraph-level chunking instead of 512-token chunks. Reduced context pollution.
> 
> Chose #2 + #3 together: BM25 for keyword precision, semantic for nuance, smaller chunks for specificity. Retrieval accuracy improved from 78% to 91%."

**Padding Answer:**
> "We optimized retrieval by tuning embedding thresholds and using semantic search."

**Scoring:**
- Specific failed query example = +2
- Root cause identified = +2
- Multiple fix attempts tried = +2
- A/B tested approaches = +2
- Quantified improvement = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"That BM25 + semantic hybrid: what's the query latency trade-off? Are you running two retrievals in parallel?"*
2. *"For the 'ibuprofen + enalapril' query, does BM25-only return the wrong docs too, or does it catch the right ones?"*
3. *"Did switching chunk size affect other queries negatively? Any trade-offs?"*

---

## 2.4 System Prompt Tradeoff

**QUESTION:**
> You mention 'structured system prompts' and 'few-shot examples.' Give me a concrete system prompt you wrote for a RAG system. What happens if you remove the few-shot examples, does latency improve? By how much? Was it worth the accuracy loss?

**Expected Strong Answer:**
> "System prompt for drug interaction checker:
> ```
> You are a clinical pharmacist expert. Given a list of patient medications, 
> identify serious drug interactions.
> 
> Rules:
> 1. Only cite interactions found in the provided documents.
> 2. Do not invent statistics or severity levels.
> 3. If unsure, respond "Unknown" rather than guessing.
> 4. Provide reasoning: why this interaction matters.
> 
> Examples:
> Q: Warfarin + Ibuprofen?
> A: MAJOR interaction (both increase bleeding risk). Source: FDA MedWatch database.
> 
> Q: Metformin + Aspirin?
> A: Minor interaction (may reduce aspirin effectiveness). Manageable with monitoring.
> ```
> 
> Removing few-shot examples: latency drops 8% (fewer tokens processed), but accuracy drops 15% (LLM makes up interactions more often). Not worth it. The examples are cheap insurance."

**Padding Answer:**
> "We used structured prompts and few-shot examples to improve LLM output quality."

**Scoring:**
- Actual prompt text provided = +3
- Latency trade-off quantified = +2
- Accuracy impact measured = +2
- Decision justified = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"How many examples do you need in the few-shot prompt? Did you test 1 vs. 3 vs. 5 examples?"*
2. *"That system prompt mentions 'Source: FDA MedWatch.' Does the LLM actually cite the source, or did you add that in post-processing?"*
3. *"Did you version the system prompt? If so, what changed between v1 and v2?"*

---

## 2.5 LangSmith Monitoring

**QUESTION:**
> You used LangSmith for observability. What specific metrics were you tracking? Hallucination rate? Retrieval accuracy? Token usage? Give me an example of when a LangSmith dashboard alert fired and what you did about it.

**Expected Strong Answer:**
> "LangSmith tracked:
> - **Retrieval precision** (% of top-3 chunks actually relevant to query)
> - **Hallucination rate** (% of responses containing unsourced claims, manually labeled)
> - **Token usage per query** (to catch prompt bloat)
> - **Latency p50/p95/p99** (to catch performance regressions)
> - **LLM error rate** (API failures, timeouts)
> 
> One alert: hallucination rate jumped from 5% to 12% on Tuesday. Investigated and found a model update (Claude 3.5 Sonnet released). Tested on the same queries: older model had 8%, new had 12%. Reverted to Opus, which had 6%. The new Sonnet model was more prone to inventing stats. Alert saved us from deploying to prod."

**Padding Answer:**
> "We used LangSmith to monitor RAG pipeline performance and LLM quality."

**Scoring:**
- Specific metrics listed = +2
- Alert example with root cause = +3
- Action taken based on alert = +2
- Shows proactive monitoring culture = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That hallucination rate of 5%—how many queries does that represent? 1 in 20? And how are you manually labeling hallucinations?"*
2. *"Did you ever have an alert that was a false positive (alert fired but no actual problem)?"*
3. *"Do you A/B test model updates in LangSmith before rolling to prod, or did you test after?"*

---

## 2.6 Cost of Evaluation

**QUESTION:**
> You mention 'manually reviewed 5% monthly.' That's expensive. How did you select which queries to review? Random? Only low-confidence responses? Did you ever find a correlation between a specific type of query (e.g., multi-hop reasoning) and hallucination rate?

**Expected Strong Answer:**
> "Sampling strategy:
> - 80%: random sample of all queries (to catch systemic issues)
> - 20%: stratified by confidence score (low-confidence responses reviewed more often; LLM outputs a confidence flag)
> 
> Over 6 months, found that:
> - Simple drug lookups: 1% hallucination rate (easy for LLM)
> - Multi-drug interactions: 8% (harder reasoning)
> - Edge cases (pregnant + meds): 15% (context-heavy)
> 
> This let us prioritize: focus eval on multi-drug and edge cases, reduce random sampling.
> 
> Cost: ~40 hours/month of pharmacist time at $100/hr = $4K/month. For a critical system, acceptable."

**Padding Answer:**
> "We manually reviewed queries to maintain quality."

**Scoring:**
- Sampling strategy explained = +2
- Stratification logic justified = +2
- Found correlation (query type → hallucination) = +2
- Cost quantified = +1
- Prioritization decision = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That 15% hallucination rate on edge cases (pregnant + meds)—did you retrain or change the prompt for those?"*
2. *"40 hours/month = 10 hours/week. How many queries is that reviewing? At 100 queries/hour, that's 1K queries reviewed/month?"*
3. *"Did you ever automate the hallucination detection (e.g., check if LLM's claim exists in source docs)?"*

---

## 2.7 Chunking Strategy: Size & Strategy

**QUESTION:**
> Walk me through your chunking strategy for one of your RAG systems. Document size? Overlap? Semantic chunking? Did you change strategy across different document types (PDFs vs. HTML vs. plain text)?

**Expected Strong Answer:**
> "Walgreens drug database:
> - **FDA drug database:** Fixed 256-token chunks, 50% overlap. Why? Drug info is dense; larger chunks dilute relevance.
> - **Clinical guidelines:** Semantic chunking (one chunk = one clinical recommendation/section). Variable size (200-400 tokens). Why? Sections are naturally discrete units.
> - **Conversation history:** Message-level chunks (no overlap). Why? Each message is a discrete turn; overlapping would duplicate context.
> 
> Initial mistake: tried 512-token fixed chunks for everything. Precision dropped. Clinical guideline retrieval would pull unrelated sections; LLM would confuse them.
> 
> Changed to semantic chunking for guidelines: precision improved 10%, latency increased 5% (more overhead). Worth it."

**Padding Answer:**
> "We used 512-token chunks with overlap for better retrieval quality."

**Scoring:**
- Different strategies per document type = +3
- Chunk size justified for each = +2
- Overlap strategy explained = +2
- Trial-and-error story = +1
- Quantified impact = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"That semantic chunking for guidelines—how do you detect where one section ends and another begins? Did you use HTML headers, or NLP?"*
2. *"Did you ever test smaller chunks (e.g., 128 tokens)? Why not?"*
3. *"For conversation history, why message-level and not turn-pair level (user + assistant response)?"*

---

## 2.8 Embedding Model Choice

**QUESTION:**
> You mention Pinecone, pgvector, Weaviate. Did you ever benchmark different embedding models (e.g., OpenAI text-embedding-3 vs. Anthropic's embeddings vs. open-source)? How did you measure 'better'—retrieval precision, embedding speed, cost, or something else?

**Expected Strong Answer:**
> "Benchmarked three models on 500 labeled drug interaction queries:
> 
> 1. **OpenAI text-embedding-3-small:**
>    - Precision@5: 87% | Cost: $0.02 per 1M tokens
>    - Speed: 50ms per embedding batch
> 
> 2. **OpenAI text-embedding-3-large:**
>    - Precision@5: 92% | Cost: $0.20 per 1M tokens
>    - Speed: 80ms per batch
> 
> 3. **Open-source (gte-large):**
>    - Precision@5: 85% | Cost: free (self-hosted)
>    - Speed: 200ms per batch (CPU inference)
> 
> Chose text-embedding-3-small: best cost/precision trade-off. Considered gte-large for cost savings but precision drop (2%) wasn't acceptable for drug safety."

**Padding Answer:**
> "We used OpenAI embeddings for semantic search in our RAG system."

**Scoring:**
- Multiple models benchmarked = +2
- Metric chosen (precision@5) = +2
- Cost comparison = +2
- Trade-off decision justified = +2
- Specific metric deltas = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"Did you re-embed your entire database when switching models, or did you keep old embeddings?"*
2. *"That 2% precision drop with gte-large—does that translate to missed interactions? How many false negatives?"*
3. *"Did you ever try domain-specific models (e.g., PubMedBERT for biomedical text)?"*

---

## 2.9 Hybrid Search

**QUESTION:**
> You mention 'hybrid dense-sparse retrieval.' Walk me through a case where BM25 (sparse) outperformed dense embeddings. Why? When do you choose dense-only vs. hybrid?

**Expected Strong Answer:**
> "Query: 'Warfarin + Ibuprofen interaction'
> 
> Dense embedding retrieval: pulled articles about anticoagulants (synonymous) and NSAIDs (synonymous), but missed the direct 'Warfarin-Ibuprofen' warning because the warning used slightly different phrasing in the source.
> 
> BM25 retrieval: found the exact phrase 'Warfarin' and 'Ibuprofen' in the same paragraph.
> 
> Why BM25 won: exact keyword matching. Dense embeddings excel at semantic similarity (synonyms, paraphrasing) but can miss exact-term matches buried in long documents.
> 
> Strategy: Hybrid (BM25 + dense) with a merge function:
> - BM25 score if keyword match exists
> - Fall back to dense if no keyword match
> - Combine both scores (weighted: 60% BM25, 40% dense)
> 
> When to choose:
> - Dense-only: exploratory search, summarization, questions with many synonyms
> - BM25-only: factual lookups, precise terms
> - Hybrid: critical info (drug interactions, legal clauses)"

**Padding Answer:**
> "Hybrid search combines dense and sparse retrieval for better results."

**Scoring:**
- Specific failed query example = +3
- Root cause of dense failure = +2
- Strategy to combine both = +2
- Weighting logic explained = +1
- Use case guidance = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"That 60/40 weighting (BM25/dense)—how did you arrive at those numbers? Did you A/B test?"*
2. *"For the merge function, are you using reciprocal rank fusion (RRF) or cosine similarity?"*
3. *"What latency hit did hybrid search add vs. dense-only?"*

---

## 2.10 AWS Niche: Bedrock + RAG

**QUESTION:**
> You integrated 'AWS Bedrock' for LLM calls. Did you build RAG on top of Bedrock embeddings, or did you use a third-party embedding model? What's the trade-off? How does Bedrock's knowledge cutoff affect hallucination risk in your domain (banking/pharmacy)?

**Expected Strong Answer:**
> "At Wells Fargo: used Bedrock for LLM inference (Claude 3 Sonnet) but **not** for embeddings. Why?
> 
> Bedrock embeddings (Titan) are limited (1024-d, general-purpose). Drug/finance domains need specialized embeddings. So architecture:
> - Embeddings: OpenAI text-embedding-3 (stored in pgvector)
> - LLM: Bedrock Claude 3 Sonnet (for compliance; data stays in AWS)
> 
> Knowledge cutoff risk: Bedrock Claude (April 2024 cutoff) doesn't know recent drug interactions or financial regulations. Mitigated by:
> - RAG retrieves fresh documents (updated weekly)
> - System prompt: 'Use provided docs, not your training knowledge'
> 
> Trade-off: Bedrock pricing is 3x OpenAI's Claude API for same model, but compliance wins (HIPAA on Bedrock FedRamp)."

**Padding Answer:**
> "We used Bedrock embeddings and LLM inference for RAG pipelines in AWS."

**Scoring:**
- Distinction between Bedrock embeddings vs LLM = +3
- Rationale for third-party embeddings = +2
- Knowledge cutoff risk identified = +2
- Mitigation strategy (RAG + system prompt) = +1
- Cost/compliance trade-off = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"Did you ever test Bedrock embeddings to see if they were 'good enough' for your domain?"*
2. *"That system prompt 'Use provided docs, not training knowledge'—how do you enforce that? Does Claude ignore its training if you ask?"*
3. *"Did HIPAA compliance actually require Bedrock, or was it a recommendation?"*

---

## 2.11 Context Window Limits

**QUESTION:**
> Walk me through how you handled context window limits. If your RAG retriever pulled 10 relevant documents but Claude's context only fits 3, how did you rank them? Did you ever change model (Claude 3.5 Sonnet vs. Haiku) based on available context?

**Expected Strong Answer:**
> "Claude 3 Sonnet: 200K context. Typical drug interaction query retrieves 5-10 documents (~50K tokens). Fit comfortably.
> 
> But for complex cases (e.g., pregnant patient on 8 medications + comorbidities), retrieved docs expanded to 100K+ tokens. Strategy:
> 1. Re-rank with cross-encoder: score all 10 docs, keep top 3.
> 2. Hierarchical summarization: summarize retrieved docs first (1K tokens), then pass summary + query to LLM.
> 3. Switch to Claude 3.5 Sonnet (higher context, but higher cost).
> 
> Never switched to Haiku; context too small.
> 
> In practice: re-ranking handled 85% of cases. Summarization + Sonnet used for edge cases (15%). Cost/latency acceptable."

**Padding Answer:**
> "We managed context window limits by filtering and prioritizing relevant documents."

**Scoring:**
- Multiple strategies tried = +3
- Specific token counts = +2
- Model trade-off decision = +2
- Quantified effectiveness (85%/15%) = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That hierarchical summarization—do you use Claude to summarize, or a smaller model?"*
2. *"Did reducing docs from 10 to 3 ever cause missed interactions (recall loss)?"*
3. *"What's the latency of re-ranking 10 docs with a cross-encoder?"*

---

## 2.12 Vector Store Selection

**QUESTION:**
> You list: Pinecone, pgvector, Weaviate, Chroma, Milvus. Did you deploy multiple vector stores for the same use case to benchmark? If so, which won and why? If not, how did you decide on your first choice?

**Expected Strong Answer:**
> "Walgreens:
> - **Pinecone (dev):** Managed service, easy setup, cost-prohibitive at scale (100M drug docs). $3K/month.
> - **pgvector (prod):** Postgres extension, self-hosted, cost $500/month (same data). Latency: 50ms vs. Pinecone's 20ms (acceptable).
> - **Weaviate (POC):** Tried for semantic search with filtering. Performance was slower than pgvector for drug lookup.
> 
> Benchmark: 500 queries on all three.
> - Pinecone: p95 latency 30ms, recall 98%
> - pgvector: p95 latency 70ms, recall 97%
> - Weaviate: p95 latency 120ms, recall 96%
> 
> Chose pgvector for prod: cost savings justified 40ms latency increase. Pinecone for dev (speed, no schema management)."

**Padding Answer:**
> "We used Pinecone for RAG and pgvector for semantic search in PostgreSQL."

**Scoring:**
- Benchmarked multiple stores = +3
- Quantified latency/recall/cost = +3
- Decision justified = +2
- Different tools for dev vs. prod = +1
- **Total: 9/10 = Real Expert**

**Follow-Ups:**
1. *"That 40ms latency increase (50ms → 70ms)—did it impact user experience? Did you add caching?"*
2. *"Did you ever consider DynamoDB as a vector store, or is it not suitable?"*
3. *"That $3K/month Pinecone cost—how many queries/month justified that price?"*

---

## 2.13 Embedding Dimensionality

**QUESTION:**
> What's the dimensionality of your embeddings? 1536 (OpenAI)? 768 (sentence-transformers)? Did you experiment with lower dimensions? Did PCA help? What's the trade-off you observed between dimensionality and recall?

**Expected Strong Answer:**
> "OpenAI text-embedding-3: 1536-d by default.
> 
> Query latency was 200ms in Pinecone (100M docs). Experimented:
> - Full 1536-d: recall 98%, latency 200ms
> - PCA to 768-d: recall 95%, latency 50ms (4x faster)
> - PCA to 512-d: recall 92%, latency 30ms
> 
> For drug interactions, 3% recall loss is unacceptable (could miss rare interactions). So kept 1536-d for prod.
> 
> But for exploratory search (patient education), used 768-d: faster, acceptable loss."

**Padding Answer:**
> "We reduced dimensionality with PCA to improve query latency."

**Scoring:**
- Specific dimensions tested = +2
- Measured recall at each level = +2
- Trade-off decision justified = +2
- Different strategies for different use cases = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"Why PCA instead of other dimensionality reduction (UMAP, t-SNE)?"*
2. *"Did you retrain embeddings after PCA, or did you project existing ones?"*
3. *"For exploratory search (768-d), did 3% recall loss cause user complaints?"*

---

## 2.14 Semantic vs. Keyword Search (BM25)

**QUESTION:**
> Walk me through a scenario where keyword search (BM25) outperformed semantic search. Give me a concrete query example. When do you choose BM25-only vs. hybrid vs. semantic-only?

**Expected Strong Answer:**
> "Query: 'FDA warning: warfarin + ibuprofen'
> 
> Semantic search: pulled articles about anticoagulants, NSAIDs, but not the specific FDA warning paragraph.
> 
> BM25: found 'FDA warning' + 'warfarin' + 'ibuprofen' in exact doc section.
> 
> Why BM25 won: exact keyword + regulatory phrase matching.
> 
> Decision tree:
> - **BM25-only:** Regulatory lookups, exact drug names, precise terms ('contraindication', 'MAJOR')
> - **Semantic-only:** Questions with synonyms ('What painkillers work with blood pressure meds?')
> - **Hybrid:** Mixed queries (e.g., drug name + symptom description)"

**Padding Answer:**
> "Hybrid search combines dense and sparse retrieval for better results."

**Scoring:** [Already scored in 2.9 above]

---

## 2.15 Metadata Filtering

**QUESTION:**
> Did you store metadata with embeddings (e.g., document date, source, category)? Walk me through a case where metadata filtering helped and one where it hurt. Did metadata filtering ever cause missed results?

**Expected Strong Answer:**
> "Metadata stored: source (FDA official vs. clinical commentary), date (created, updated), category (drug interaction, side effect, dosage).
> 
> **Helped:** Filtering to FDA-official sources only improved precision (fewer false claims).
> 
> **Hurt:** One case: patient's off-label drug wasn't in FDA database. Filtered search returned 0 results. Had to remove source filter and search clinical literature instead. Patient almost got bad advice.
> 
> Lesson: metadata filtering is powerful but can create false negatives. Added fallback: if filtered search returns 0, retry without filter."

**Padding Answer:**
> "We filtered documents by source and type for better retrieval."

**Scoring:**
- Specific metadata fields stored = +1
- Helped case + reason = +2
- Hurt case + impact = +2
- Fallback strategy = +2
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That fallback (retry without filter)—does the user know they're getting non-official sources? How do you surface that?"*
2. *"Did you ever have metadata that was missing or incorrect? How did you handle that?"*
3. *"For date filtering, how old is 'too old' for drug interactions? 1 year? 5 years?"*

---

## 2.16 Re-ranking Strategy

**QUESTION:**
> You mention using a reranker. Was it a cross-encoder (like mmarco-mMiniLMv2-L12-H384-v1)? A separate LLM call? Did you A/B test with/without reranking? What metric improved—recall, precision, or both?

**Expected Strong Answer:**
> "Used cross-encoder (mmarco-mMiniLMv2-L12-H384-v1) for dense retrieval re-ranking.
> 
> Pipeline: BM25 + semantic search → top-10 docs → cross-encoder scores → top-3 to LLM.
> 
> A/B test (500 queries):
> - Without reranker: precision 87%, recall 89%, latency 150ms
> - With reranker: precision 92%, recall 88%, latency 250ms
> 
> Trade-off: +5% precision, -1% recall, +100ms latency. Worth it for critical info."

**Padding Answer:**
> "We used a reranker to improve retrieval quality."

**Scoring:**
- Specific cross-encoder model = +2
- Pipeline diagram (implicit) = +1
- A/B test results = +3
- Trade-off decision = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"Why that specific cross-encoder? Did you benchmark others (ranker-lite, cross-encoder/mmarco-MiniLM)?"*
2. *"Cross-encoder inference—is it GPU-accelerated or CPU? Latency per doc?"*
3. *"Did you retrain the cross-encoder on drug interaction data, or use it off-the-shelf?"*

---

## 2.17 Cold-Start Problem

**QUESTION:**
> When you first deployed a RAG system, how did you seed your vector database? Manual indexing? Bulk import? How long did it take? Did you ever have to rebuild/reindex after discovering chunks were too large or embeddings were suboptimal?

**Expected Strong Answer:**
> "Walgreens drug interaction RAG:
> 
> **Initial seed:** 50K FDA drug documents.
> - Parsed PDFs → plain text chunks (256 tokens, 50% overlap) → generated embeddings (OpenAI text-embedding-3)
> - Bulk import to Pinecone via Python SDK: 2 hours (API rate limits: 20 requests/sec)
> - Deployment to prod: canary (10% queries) → full rollout
> 
> **Reindex event:** 2 months later, discovered chunking strategy was suboptimal. Reindexed with semantic chunking:
> - Regenerated embeddings: 1 hour
> - Rebuilt indexes: 3 hours downtime
> - Total: 4-hour maintenance window"

**Padding Answer:**
> "We indexed documents into the vector database for RAG pipelines."

**Scoring:**
- Specific doc count and seed strategy = +2
- Timing details = +1
- Reindex story (failure + recovery) = +2
- Downtime quantified = +1
- **Total: 6-7 = Credible**

**Follow-Ups:**
1. *"Did you test the reindexed system before deploying? How?"*
2. *"That 4-hour downtime—did you have a rollback plan?"*
3. *"Did users notice the reindex, or was it transparent?"*

---

## 2.18 Query Expansion

**QUESTION:**
> Did you implement query expansion (e.g., generating multiple search queries from a single user query to cast a wider net)? If so, walk me through an example. Did it help or hurt precision?

**Expected Strong Answer:**
> "User query: 'What painkillers can I take with blood pressure meds?'
> 
> Expanded to:
> 1. 'NSAIDs + antihypertensives'
> 2. 'acetaminophen + blood pressure medication'
> 3. 'analgesics + ACE inhibitors'
> 4. 'pain relief + hypertension drugs'
> 
> Ran all 4 queries in parallel (SQS), merged results with re-ranking.
> 
> Effect: +15% recall (caught synonymous phrasing). Precision dropped 3% (some unrelated docs). Cost: 3 LLM calls per query. Worth it for high-stakes domain (pharma)."

**Padding Answer:**
> "We used LLM-based query expansion to improve retrieval coverage."

**Scoring:**
- Specific expansion examples = +2
- Execution strategy (parallel, merge) = +2
- Quantified recall/precision/cost = +2
- Decision justified = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"Who generated the expanded queries? Claude? In real-time or pre-computed?"*
2. *"That 3% precision drop—was it noise docs or genuinely irrelevant?"*
3. *"Did you ever have an expansion query that actually worsened results?"*

---

## 2.19 Staleness & Updates

**QUESTION:**
> How often did you update embeddings? Real-time? Nightly batch? Weekly? Did you ever discover that embeddings became stale and caused wrong answers? How did you detect staleness?

**Expected Strong Answer:**
> "FDA drug database: weekly batch re-index (Sunday midnight, 1-hour window).
> 
> **Staleness incident:** New drug interaction warning added Tuesday. Wasn't indexed until Sunday. A patient was advised on Friday based on old data.
> 
> Detected: pharmacist flagged query as stale in manual review (LangSmith).
> 
> Solution: switched to streaming updates via CDC (Change Data Capture) from Postgres:
> - New docs streamed to embedding service
> - Embeddings generated in <2 min
> - Indexed in pgvector immediately
> - SLA: new info searchable within 5 minutes"

**Padding Answer:**
> "We maintained updated embeddings in the vector database."

**Scoring:**
- Update frequency specified = +1
- Staleness incident with real impact = +2
- Detection mechanism = +1
- Solution implemented (CDC) = +2
- New SLA defined = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That CDC streaming—does it handle deletes/corrections too, or just new docs?"*
2. *"Did the 5-minute SLA ever break? If so, what caused it?"*
3. *"For old embeddings (before Tuesday), do you have a way to detect them in retrieval results?"*

---

## 2.20 Embedding Model Selection (Deep)

**QUESTION:**
> Which embedding model did you use in production? OpenAI text-embedding-3-small? Large? Sentence-transformers? Domain-specific (e.g., BioBERT for pharma)? Did you benchmark multiple models? On what metric?

**Expected Strong Answer:**
> "Walgreens production: OpenAI text-embedding-3-small.
> 
> Benchmarked:
> - **text-embedding-3-small:** Recall 87%, cost $0.02/1M tokens, speed 50ms
> - **text-embedding-3-large:** Recall 92%, cost $0.20/1M tokens, speed 100ms
> - **gte-large (open-source):** Recall 85%, cost free (self-hosted), speed 200ms CPU
> - **BioBERT (domain):** Recall 88%, cost free, speed 150ms
> 
> Benchmark: 500 drug-interaction queries, manually labeled relevance.
> Metric: NDCG@5 (normalized discounted cumulative gain).
> 
> Chose text-embedding-3-small: best cost/quality. Considered gte-large but 2% drop unacceptable for drug safety."

**Padding Answer:**
> "We used OpenAI embeddings for semantic search in our RAG system."

**Scoring:**
- Multiple models benchmarked = +2
- Domain-specific model considered = +1
- Metric chosen (NDCG@5) = +1
- Quantified comparison = +2
- Trade-off justified = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That BioBERT option—did you test it more rigorously? Why not use domain-specific?"*
2. *"For text-embedding-3-large, the 5% recall improvement—do you quantify as number of missed interactions?"*
3. *"Did you ever switch embedding models in production (e.g., small → large)? How did you handle re-embedding?"*

---

## 2.21 Hallucination Mitigation: Retrieval vs. Prompt

**QUESTION:**
> When your system hallucinated, was it a retrieval problem (wrong documents pulled) or a generation problem (LLM made up info)? Give me examples of each and how you fixed them differently.

**Expected Strong Answer:**
> "**Retrieval hallucination:**
> Query: 'Can I take ibuprofen with enalapril?' Retriever pulled old review mentioning both drugs unrelated.
> LLM invented interaction.
> **Fix:** Add query-document similarity threshold (skip chunks <0.75 cosine similarity). Also added BM25 hybrid search.
> 
> **Generation hallucination:**
> Query: 'Warfarin + ibuprofen.' Retriever pulled correct docs.
> LLM said: 'Increases kidney damage risk by 30%' (made-up stat).
> **Fix:** System prompt: 'Do not cite statistics not explicitly in source documents. If unsure, respond "Unknown."'
> 
> Measured separately:
> - Retrieval hallucination: before BM25, 8 missed interactions per 100 queries. After, 2 per 100.
> - Generation hallucination: before prompt fix, 6 invented stats per 100. After, <1 per 100."

**Padding Answer:**
> "We reduced hallucinations with prompt engineering and LLM observability."

**Scoring:**
- Distinct examples for each type = +2
- Different fixes per type = +2
- Quantified before/after = +2
- Shows deep understanding = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"For retrieval hallucinations, did the similarity threshold ever reject valid docs?"*
2. *"That system prompt fix—did you version it? What changed between v1 and v2?"*
3. *"How do you measure 'invented stat'? Manual review or automated detection?"*

---

## 2.22 Multi-Modal RAG

**QUESTION:**
> Did you ever build RAG systems with images (e.g., drug labels, chemical structures)? If so, did you use image embeddings? How did you handle image + text search together?

**Expected Strong Answer:**
> "Wells Fargo: loan documents contained signatures, bank statements (images), and text.
> 
> Initial approach: OCR all images to text (expensive, error-prone). Switched to:
> - Text extraction via PyPDF for text-native sections
> - Image embeddings (OpenAI CLIP) for signature/document verification
> - Hybrid retrieval: text BM25 + CLIP image similarity
> 
> Use case: 'Find loan docs where customer signed' → CLIP similarity search for signature.
> 
> Limitation: CLIP isn't great for domain-specific images (bank logos, watermarks). Considered training custom image encoder but ROI wasn't there."

**Padding Answer:**
> "We handled multimodal documents in our RAG system."

**Scoring:**
- Specific approach (OCR → text extraction + CLIP) = +2
- Trade-off decision (OCR → hybrid) = +1
- Use case example = +1
- Limitation acknowledged = +1
- **Total: 5-6 = Padding (but recoverable)**

**Follow-Ups:**
1. *"Did CLIP work well for your signature matching, or did you need something better?"*
2. *"How did you embed images into your vector store? Did you create 'image' chunks like text chunks?"*
3. *"For that ROI calculation on training a custom encoder, what would've changed your mind?"*

---

## 2.23 Cost Breakdown

**QUESTION:**
> Break down the cost of your RAG system. Embedding API calls? Vector store queries? LLM inference? Storage? Which component was most expensive? Did you optimize the expensive one?

**Expected Strong Answer:**
> "Walgreens, 100K daily queries:
> 
> | Component | Daily Cost | % of Total |
> | --- | --- | --- |
> | Embedding API (OpenAI) | $50 | 16% |
> | Pinecone vector DB | $20 | 6% |
> | LLM inference (Claude) | $200 | 65% |
> | Storage (S3 + Pinecone) | $10 | 3% |
> | Monitoring (LangSmith, Datadog) | $15 | 5% |
> | **Total** | **$295** | **100%** |
> 
> Most expensive: LLM inference (65%).
> 
> Optimization: Implemented query caching (80/20 rule: 20% of queries = 80% of traffic). Cached results in Redis.
> - Before caching: $200/day LLM cost
> - After caching: $120/day (40% reduction)
> - Cache miss rate: 5% (acceptable)"

**Padding Answer:**
> "We optimized RAG costs using caching and efficient vector databases."

**Scoring:**
- Detailed cost breakdown = +3
- Identified most expensive component = +2
- Optimization strategy with impact = +2
- Real metrics (80/20 rule) = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That cache: TTL? How do you handle stale cached results?"*
2. *"Did you ever change embedding API (OpenAI → self-hosted) to reduce that $50/day?"*
3. *"What was the total monthly RAG spend? Feasible for Wells Fargo?"*

---

## 2.24 Evaluation Methodology

**QUESTION:**
> How did you evaluate your RAG system? Did you use RAGAS (Retrieval-Augmented Generation Assessment)? Custom metrics? Manual judgment? Walk me through your eval pipeline.

**Expected Strong Answer:**
> "Built evaluation pipeline:
> 
> 1. **Golden dataset:** 300 (query, relevant_docs, expected_answer) triplets, manually labeled by pharmacists.
> 
> 2. **Metrics:**
>    - Retrieval: recall@3, recall@5, recall@10, precision@3
>    - Generation: BLEU, ROUGE-L vs. gold answers
>    - Hallucination: % of responses with unsourced claims (manual spot-check, 5%)
>    - Latency: p50, p95, p99
> 
> 3. **Evaluation cadence:** Weekly runs, LangSmith + custom Python.
> 
> 4. **Regression testing:** Any prompt or embedding model change runs eval first. Don't deploy if metrics regress >2%."

**Padding Answer:**
> "We used LangSmith for RAG evaluation."

**Scoring:**
- Specific eval methodology = +3
- Multiple metric types = +2
- Cadence defined = +1
- Regression testing policy = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"Those 300 golden samples—how did you ensure diversity (simple queries, complex, edge cases)?"*
2. *"Did RAGAS ever come up? Why not use it?"*
3. *"That 2% regression threshold—how did you arrive at it? Was it empirically derived?"*

---

---

# SECTION 3: AGENT DESIGN DECISIONS

## 3.1 Primary: Rules → Agent

**QUESTION:**
> Walk me through a feature you initially built as a traditional API and then realized you needed to make into an agent. What was the trigger that made you decide 'this needs an agent, not a pipeline'? And what broke when you switched?

**Expected Strong Answer:**
> "At Walgreens, prescription form auto-completion was initially a rules engine: if antacid → hide H2-blockers, if ACE inhibitor → check kidney function. ~50+ rules.
> 
> Trigger: team kept adding rules. Business wanted: 'What if patient is also on X?' Exponential complexity.
> 
> Switched to agent: given patient meds + contraindications, let Claude reason through 'what can we recommend?'
> 
> What broke:
> 1. **Latency:** Agent took 2-3s per form. Users expected <500ms. Fixed with caching + pre-warming.
> 2. **Cost:** $0.001 → $0.02 per form (3-5 LLM calls per invocation).
> 3. **Observability:** Rules engine deterministic. Agent probabilistic. Added LangSmith tracing + spot-checks.
> 
> Retrospective: agents won only for truly ambiguous scenarios. 80% of forms, rules worked fine. Ended up hybrid: rules for clear cases, agent for edge cases."

**Padding Answer:**
> "We used LangChain to build agents with tool calling and function integration. This enabled autonomous reasoning and workflow orchestration for complex tasks."

**Scoring:**
- Specific trigger (exponential rules) = +2
- What broke (3 concrete issues) = +2
- Honest retrospective (hybrid approach) = +2
- Quantified impact (cost, latency) = +2
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That hybrid approach—how do you decide routing (rules vs. agent)? Rule certainty score?"*
2. *"The 80% rules, 20% agent split—did you measure that empirically?"*
3. *"If you were designing today, would you still use agents for the edge cases?"*

---

## 3.2 Tool Calling Implementation

**QUESTION:**
> Describe the tools your agent could call. How many? Give me an example of one tool and exactly what it did. Did the agent ever call tools in the wrong order, or with the wrong arguments, and how did you handle that?

**Expected Strong Answer:**
> "Walgreens drug interaction agent: 4 tools.
> 
> 1. **query_fda_database(drug_name: str) → list[str]:** Returns known interactions for a drug.
> 2. **check_formulary(drug_name: str) → bool:** Returns True if drug is covered by patient's insurance.
> 3. **get_patient_history(patient_id: str) → dict:** Returns past prescriptions (to catch repeat interactions).
> 4. **check_kidney_function(patient_id: str) → float:** Returns GFR (glomerular filtration rate) for dose adjustment.
> 
> Wrong order example: Agent called check_kidney_function before query_patient_history. Tried to adjust dose for unknown condition.
> 
> Handled via:
> - System prompt: explicit tool ordering ('First query FDA, then check formulary, then check patient history')
> - Tool output validation: if GFR but no prior condition, reject and retry.
> - Max tool calls: 10 (prevent infinite loops)"

**Padding Answer:**
> "We built agent tools for drug information and patient data retrieval."

**Scoring:**
- Specific tools with signatures = +2
- Tool output type specified = +1
- Wrong order story + fix = +2
- Error handling + safeguards = +2
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That system prompt ordering—did agents actually follow it, or did they need manual intervention?"*
2. *"The max 10 tool calls—did you ever hit that limit on legitimate queries?"*
3. *"For check_kidney_function, what happens if the API is down?"*

---

## 3.3 Reasoning Chain Length

**QUESTION:**
> How many steps did your agent typically take to solve a problem? 2-3 steps? 5-10? What was the longest chain you ever had to debug? Did token usage explode, and how did you optimize it?

**Expected Strong Answer:**
> "Typical drug interaction query: 4-5 steps.
> 1. Parse input (extract drugs, patient info)
> 2. Query FDA for each drug
> 3. Check formulary
> 4. Synthesize answer
> 
> Longest chain: 12 steps (pregnant patient on 8 meds + comorbidities). Token usage: 15K tokens (vs. typical 3K).
> 
> Problem: latency hit 8+ seconds. Optimized:
> - Parallelize tool calls (FDA query for all drugs at once, not one by one)
> - Summarize intermediate results ('Drug A: no interactions. Drug B: 1 minor. Drug C: contraindicated.')
> - Cache patient history (don't re-fetch every call)
> 
> Result: 12-step chain reduced to 7 effective steps, latency back to 2-3s."

**Padding Answer:**
> "Agents typically execute multiple steps to solve problems."

**Scoring:**
- Specific step count (typical + extreme) = +2
- Token usage measured = +1
- Performance problem articulated = +1
- Multiple optimization strategies = +2
- Quantified improvement = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That parallelization—how do you aggregate results from parallel tool calls? Sequential prompting?"*
2. *"Did summarizing intermediate results ever lose important info for downstream steps?"*
3. *"That 12-step chain—what was the longest you'd ever allow in production?"*

---

## 3.4 Failure Mode

**QUESTION:**
> Tell me about a time the agent got stuck in a loop or took too long to respond. What was the root cause? How did you detect it? How did you fix it?

**Expected Strong Answer:**
> "Agent got stuck in a loop: trying to reformulate a query that failed.
> 
> Scenario: Patient on 'Medication X' (fake name). Agent queries FDA, gets error 'Drug not found.' Retries with similar names, keeps failing.
> 
> Root cause: typo in patient's prescription list. Agent didn't recognize the error and kept retrying.
> 
> Detection: LangSmith trace showed repeated tool calls with no progress. Also: user clicked 'Cancel' after 10s timeout.
> 
> Fix: Added a 'step counter' + circuit breaker. If same tool called 3+ times without progress (output unchanged), agent says 'I can't find this drug. Please verify spelling.'
> 
> Prevention: Added input validation (check drug names against FDA database before running agent)."

**Padding Answer:**
> "We monitored agent performance and implemented safeguards against infinite loops."

**Scoring:**
- Specific loop example = +2
- Root cause identified (typo, no error handling) = +2
- Detection method (LangSmith + timeout) = +1
- Solution + prevention = +2
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That circuit breaker (3 calls + no progress)—did it ever trigger falsely on legitimate queries?"*
2. *"For input validation, how do you handle typos or drug name variations (brand vs. generic)?"*
3. *"What's the user experience when the agent says 'I can't find this drug'?"*

---

## 3.5 Deterministic vs. Probabilistic

**QUESTION:**
> You mention agents are 'probabilistic.' Did you ever need deterministic behavior (same input → same output every time)? How did you handle that? Did you cache agent responses, and if so, for how long?

**Expected Strong Answer:**
> "For drug safety, you *need* deterministic behavior. If agent says 'Warfarin + Ibuprofen is safe' on Tuesday, it must say the same on Wednesday.
> 
> Handled via:
> 1. **Response caching:** Hash of (patient_meds, patient_age, kidney_function) → cached agent response. TTL: 24 hours (medical info can change).
> 2. **System prompt determinism:** Exact system prompt version (v1, v2, v3) pinned to each feature release. Never change mid-production.
> 3. **Tool output determinism:** FDA database outputs are deterministic. LLM reasoning is not (but limited to 1 retry max).
> 
> Cache hit rate: 60% (same patient queries repeat). Reduced latency + cost."

**Padding Answer:**
> "We cached agent responses to improve performance."

**Scoring:**
- Determinism requirement articulated = +1
- Multiple determinism strategies = +2
- Caching with TTL = +1
- System prompt versioning = +1
- Cache hit rate = +1
- **Total: 6-7 = Credible**

**Follow-Ups:**
1. *"That 24-hour TTL—did you ever get complaints about stale data (e.g., patient's kidney function changed)?"*
2. *"How do you version the system prompt? Git tags? Database version column?"*
3. *"For that 1 retry max on LLM reasoning, what happens if it fails again?"*

---

## 3.6 Cost Comparison

**QUESTION:**
> Compare the cost of the rules engine vs. the agent. You said it went from $0.001 to $0.02 per request. Break that down: how many LLM calls per agent invocation? What was the token count? Did you ever optimize the prompt to reduce tokens?

**Expected Strong Answer:**
> "Walgreens prescription form:
> 
> **Rules engine:**
> - Deterministic logic, no LLM calls: $0.001 per form
> 
> **Agent:**
> - 1 initial agent call (Claude 3 Sonnet): ~500 tokens in, ~200 tokens out = $0.01
> - 2-3 tool calls (FDA API free + patient DB free): $0
> - 1 verification LLM call: ~300 tokens = $0.01
> - Total: $0.02 per form
> 
> Optimization: Reduced system prompt from 2K tokens to 800 (removed examples, added brief principles instead). Cost dropped to $0.015.
> 
> Trade-off: shorter prompt meant slightly lower reasoning quality (95% → 92% correctness). Not worth further reduction."

**Padding Answer:**
> "Agent implementation increased cost due to multiple LLM calls."

**Scoring:**
- Cost broken down by component = +2
- LLM calls quantified = +1
- Token count specified = +1
- Prompt optimization story = +2
- Trade-off articulated = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"Did you ever consider using Haiku instead of Sonnet to reduce cost?"*
2. *"That verification call—why a separate LLM call? Could it be part of the initial agent call?"*
3. *"At $0.015-0.02 per form, what volume is breakeven with rules engine complexity?"*

---

## 3.7 Latency Tradeoff

**QUESTION:**
> The agent took 2-3 seconds. What are the components of that latency? Tool calling overhead? Waiting for backend APIs? LLM inference time? How did you optimize each?

**Expected Strong Answer:**
> "2-3 second latency breakdown:
> 
> | Component | Time | Optimization |
> | --- | --- | --- |
> | Initial agent reasoning (Claude) | 800ms | Batched system prompt, cache |
> | FDA API calls (3 parallel) | 500ms | Added Redis cache for common drugs |
> | Patient DB lookup | 200ms | Preload on patient login |
> | Formulary check | 300ms | Cache formula table (changes weekly) |
> | Final response synthesis | 200ms | Reuse earlier reasoning |
> | **Total** | **2000ms** | **~40% reduction from 3s** |
> 
> Biggest win: Redis cache for FDA queries (50% cache hit rate)."

**Padding Answer:**
> "We optimized latency by caching and parallel processing."

**Scoring:**
- Latency breakdown detailed = +2
- Each component optimized = +2
- Cache strategies specified = +1
- Quantified improvement (3s → 2s) = +1
- **Total: 6-7 = Credible**

**Follow-Ups:**
1. *"That FDA API cache: how often does it miss? What happens on cache miss?"*
2. *"The 800ms initial agent reasoning—is that the LLM streaming time, or total request time?"*
3. *"For that 2s target, what's your p95 latency? Does it ever spike above 3s?"*

---

## 3.8 Agent Framework Choice

**QUESTION:**
> You mention LangChain. Did you ever consider LlamaIndex, Anthropic's client SDK, or custom orchestration? Why LangChain? Did you run into LangChain limitations and have to write custom logic?

**Expected Strong Answer:**
> "Evaluated three frameworks:
> 
> 1. **LangChain:** Mature, many tools, but heavy abstractions. Tool calling is a bit cumbersome.
> 2. **LlamaIndex:** Better for RAG (document indexing), weaker for agent orchestration.
> 3. **Anthropic SDK:** Lightweight, tool_use response handling is clean. No built-in multi-step orchestration.
> 
> Chose LangChain because:
> - Team knew it (learning curve risk)
> - Rich tool ecosystem (pre-built integrations)
> 
> Ran into limitations:
> - Tool output validation wasn't flexible; wrote custom ValidationAgent wrapper
> - State management between steps was clunky; built custom step tracking
> 
> If restarting, I'd use Anthropic SDK + custom orchestration. It's lighter and the tool_use interface is cleaner."

**Padding Answer:**
> "We used LangChain for agent orchestration."

**Scoring:**
- Multiple frameworks compared = +2
- Specific pros/cons = +2
- Limitations discovered = +2
- Alternative strategy articulated = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That ValidationAgent wrapper—how much custom code did it require?"*
2. *"If you'd known about those limitations, would you still choose LangChain?"*
3. *"For new projects, are you using Anthropic SDK instead?"*

---

## 3.9 AWS Niche: Lambda + Agent

**QUESTION:**
> You deployed agents on AWS Lambda. Did you hit the 15-minute timeout? How did you handle long-running agent workflows? Did you ever offload to EC2 or Batch?

**Expected Strong Answer:**
> "Walgreens typical agent: 2-3s, well under Lambda's 15-minute limit.
> 
> But complex edge cases (pregnant + 8 meds + comorbidities) could hit 10-12s.
> 
> Never hit 15-min limit, so Lambda was sufficient. But for future-proofing:
> - If agents ever exceeded 15s, strategy would be:
>   1. Async jobs: user submits query → SQS → Lambda worker → SNS notification when done
>   2. Polling endpoint: user polls /status?job_id=xyz
> 
> Didn't implement because ROI wasn't there. But architecture ready if needed."

**Padding Answer:**
> "We deployed agents on AWS Lambda for scalability."

**Scoring:**
- Lambda timeout understood = +1
- Latency baseline established (2-3s) = +1
- Edge case acknowledged (10-12s) = +1
- Async strategy articulated (not implemented) = +2
- **Total: 5-6 = Padding (but shows thinking)**

**Follow-Ups:**
1. *"For those 10-12s edge cases, did users notice the wait? Any complaints?"*
2. *"If you implemented async, would you rebuild the entire agent infrastructure?"*
3. *"Lambda cold starts: did they ever affect agent latency?"*

---

## 3.10 Model Selection for Agents

**QUESTION:**
> Which Claude model did you use for agents? Sonnet? Haiku? Opus? Why that choice? Did you ever A/B test models and measure differences in tool-calling accuracy or cost?

**Expected Strong Answer:**
> "Walgreens: Claude 3 Sonnet (mid-production) with occasional Opus (complex queries).
> 
> Why Sonnet:
> - Tool calling accuracy: 95%+ (Haiku: 88%, Opus: 97%)
> - Cost: 3x cheaper than Opus
> - Latency: fastest of the three
> - Token overhead: Sonnet efficient
> 
> A/B test results (500 queries):
> - Sonnet: accuracy 95%, cost $0.010/query, latency 800ms
> - Opus: accuracy 97%, cost $0.030/query, latency 1000ms
> - Haiku: accuracy 88%, cost $0.003/query, latency 600ms
> 
> Trade-off: 2% accuracy loss (Sonnet vs. Opus) ≈ 1 mistake per 50 queries. For edge cases, routed to Opus.
> 
> Didn't use Haiku: drug interactions too complex for small model."

**Padding Answer:**
> "We used Claude 3 Sonnet for agent reasoning and inference."

**Scoring:**
- Model choice justified = +1
- Specific models compared = +2
- Quantified trade-offs (accuracy/cost/latency) = +2
- A/B test results = +2
- Routing strategy (edge cases → Opus) = +1
- **Total: 8-9 = Real Expert**

**Follow-Ups:**
1. *"That 2% accuracy loss—how did you measure it? Manual review of all Sonnet queries?"*
2. *"For routing to Opus (edge cases), how do you detect 'edge case' before running Sonnet?"*
3. *"Did you ever test Claude 3.5 Sonnet? How did it compare?"*

---

---

# SECTION 4: RAG/SEMANTIC SEARCH DEEP GRILLING

*(Resume saturated with RAG/semantic search 2023+. Use these to expose depth vs. padding.)*

## 4.1 Vector Store Selection

[**See 2.12 above** - Same question, included in Section 4 for completeness]

---

## 4.2 Embedding Dimensionality

[**See 2.13 above**]

---

## 4.3 Semantic vs. Keyword Search (BM25)

[**See 2.14 above**]

---

## 4.4 Document Chunking: Size & Strategy

[**See 2.7 above**]

---

## 4.5 Metadata Filtering

[**See 2.15 above**]

---

## 4.6 Re-ranking Strategy

[**See 2.16 above**]

---

## 4.7 Cold-Start Problem

[**See 2.17 above**]

---

## 4.8 Query Expansion

[**See 2.18 above**]

---

## 4.9 Staleness & Updates

[**See 2.19 above**]

---

## 4.10 Embedding Model Selection (Deep)

[**See 2.20 above**]

---

## 4.11 Hallucination Mitigation: Retrieval vs. Prompt

[**See 2.21 above**]

---

## 4.12 Multi-Modal RAG

[**See 2.22 above**]

---

## 4.13 RAG + Agent Interaction

**QUESTION:**
> In your agentic workflow, did the agent call the RAG system? Walk me through: agent decides it needs context → triggers retrieval → uses docs to decide next action. Did the agent ever call retrieval multiple times in one chain? Did that cause latency issues?

**Expected Strong Answer:**
> "Wells Fargo loan underwriting agent:
> 
> Step 1: Agent receives loan application → decides it needs context
> Step 2: Triggers RAG (retrieve similar loans + regulations)
> Step 3: Uses docs to ask follow-up questions or request clarifications
> Step 4: If needed, retrieves more specific docs (e.g., recent regulatory changes)
> 
> Multiple retrievals per invocation: 2-4 times typical.
> 
> Latency issue: each retrieval = 200ms → total agent latency could hit 5-6s.
> 
> Optimized via:
> - Batch retrievals: if agent needs 3 docs, submit all 3 queries in parallel (SQS fan-out)
> - Prefetch: anticipate agent's likely next question, retrieve proactively in background
> - Caching: intermediate retrieval results cached for 30min
> 
> Result: 5-6s → 2-3s agent latency."

**Padding Answer:**
> "The agent uses RAG to retrieve context for decision-making."

**Scoring:**
- Specific agent + retrieval integration = +2
- Multiple retrieval calls acknowledged = +1
- Latency problem identified = +1
- Multiple optimization strategies = +2
- Quantified improvement = +1
- **Total: 7-8 = Credible**

**Follow-Ups:**
1. *"That prefetch strategy: how do you predict the agent's next question without running the agent?"*
2. *"For 30-minute cache, did stale docs ever cause wrong decisions?"*
3. *"Did parallel retrieval (SQS fan-out) add complexity to the agent logic?"*

---

## 4.14 Cost Breakdown

[**See 2.23 above**]

---

## 4.15 Evaluation Methodology

[**See 2.24 above**]

---

---

# LIVE INTERVIEW RESPONSE TRACKING

## Response Template

```
QUESTION ASKED: [e.g., 2.1 Primary Hallucination]

CANDIDATE RESPONSE:
[Paste or paraphrase their answer here]

SCORING:
- Real Expert (9-10) / Credible (7-8) / Padding (5-6) / Fabrication (0-4)
- Confidence: [HIGH/MEDIUM/LOW]

RED FLAGS:
- [List specific red flags from response]

GREEN FLAGS:
- [List specific green flags from response]

FOLLOW-UP QUESTIONS:
1. [Generated follow-up 1]
2. [Generated follow-up 2]
3. [Generated follow-up 3]

PANEL NOTES:
[Your observations: tone, hesitation, specific details, contradictions with resume]
```

---

---

# APPENDIX: SCORING RUBRIC

| Score | Signal | Definition |
|-------|--------|-----------|
| **9-10** | **Real Expert** | Specific examples + metrics + failure stories + quantified trade-offs. Shows deep production experience. Can explain why, not just what. |
| **7-8** | **Credible** | Mostly correct, some specifics, general understanding. Missing quantified impact or failure story, but not fundamentally wrong. |
| **5-6** | **Padding** | Correct terminology, no specifics, sounds rehearsed, no failure stories. Deflects to vague language ("it improved things"). |
| **0-4** | **Fabrication** | Wrong technical details, contradicts resume, can't explain basics, invents details when probed. |

---

---

# KEY GRILLING AREAS FOR THIS CANDIDATE

1. **Spec-First Adoption (Section 1):** Did they actually lead org-wide adoption, or just use it personally? Probe numbers.

2. **RAG Precision/Recall (Sections 2 & 4):** Resume is saturated with RAG 2023+. Ask for ONE concrete hallucination story. If vague, push for metrics.

3. **Agent vs. Rules (Section 3):** When did agents actually outperform? Be skeptical of "agent is always better" claims.

4. **Cost Optimization (Sections 2, 3):** All systems have cost trade-offs. If they claim "optimized for cost," ask for before/after numbers.

5. **Production Incidents (All):** Best way to detect padding: ask for a failure + recovery story. Real experts have one. Padders deflect.

---

**Ready for live responses. Paste candidate answers above as they come in.**
