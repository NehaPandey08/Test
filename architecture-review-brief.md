# Architecture Review Brief — ModelHub Backend & Frontend

## Context
A stakeholder has produced an architecture diagram/document on Confluence and scheduled a walkthrough session. Before that session, I need an independent, evidence-based comparison between what the document claims and what the actual codebase does. The goal is to walk into the review already knowing where the document holds up, where it doesn't, and what to ask.

## Task
1. Read the architecture document at [CONFLUENCE_URL — fill in].
2. Read the backend repo at [BACKEND_REPO_PATH — fill in] and the frontend repo at [FRONTEND_REPO_PATH — fill in].
3. Systematically compare the document's claims against what the code actually does. Do not take the document at face value — verify each claim against the repo.
4. Produce a structured findings report (format below).

## What to specifically check

### Accuracy of the document itself
- For every component, flow, or data path shown in the diagram, confirm it exists in the code as described. Flag anything in the diagram that doesn't match the actual implementation (wrong direction of a call, a service that doesn't exist, a flow that's actually handled differently).
- Flag anything the diagram shows as simple/clean that is actually more complex, hacky, or coupled in the real code (e.g. a "service boundary" that's actually tightly coupled, a "cache layer" that doesn't exist).

### Completeness — what's missing from the document
- Any service, module, job, or integration that exists in the code but isn't represented at all in the document.
- Data flows that exist in code (especially background jobs, seeders, runners, scheduled tasks) but aren't shown.
- Error handling, retry logic, and failure modes — does the document show what happens when something fails, or only the happy path?
- Any undocumented external dependencies (other teams' APIs, DBA-owned scripts, shared infrastructure) that the architecture actually relies on.

### Architectural red flags — what an architect/tech lead should already know
- **Scalability**: any place where the design will break or degrade under realistic load (N+1 query patterns, unpaged API calls, missing indexes, synchronous calls that should be async, no pagination on list endpoints).
- **Data integrity**: any schema design that doesn't support safe rollback, lacks audit/versioning where it should have it, or has unclear ownership of source-of-truth data.
- **Coupling**: tight coupling between layers that should be independent (e.g. delete logic coupled to reseed logic, UI directly dependent on raw DB structure).
- **Consistency**: inconsistent patterns across the backend (e.g. some endpoints paginated, others not; some using DTOs/projections, others returning raw entities).
- **Security/access control**: anything in the document or code that doesn't account for who can modify what (e.g. no DB-level or code-level guard on fields that shouldn't be manually editable).
- **Testability**: whether the described architecture is actually something that can be tested locally (e.g. does it depend on environment-specific behavior that can't be replicated in dev?).

### Questions a reviewer should be asking
For each significant design decision in the document, generate the specific question I should ask in the walkthrough: why was this approach chosen over the alternative, what tradeoff was made, what happens at failure, how does this scale, who owns this data, is this tested.

### Reasoning quality
Where the document states a design rationale, check whether that rationale is actually consistent with what the code does. Flag any place where the stated "why" doesn't match the actual implementation, or where no rationale is given for a non-obvious choice.

## Output format
Produce a report with these sections:

1. **Summary** — 3-5 sentence overview: does the document broadly reflect reality, or are there significant gaps?
2. **Inaccuracies** — table: claim in document | what the code actually does | file/location reference
3. **Missing from document** — table: what's missing | why it matters | file/location reference
4. **Architectural concerns** — table: issue | why it's a concern | severity (high/medium/low) | file/location reference
5. **Questions to raise in the walkthrough** — a prioritized list, most important first, phrased as direct questions
6. **What a tech lead/architect building this system should already know** — a short, pointed list of anything in the above that reflects a gap in understanding of the system's own design, not just a documentation gap

Be specific and cite file paths / line references wherever possible so every finding can be independently verified in the review. Do not soften findings — if something is a real architectural problem, say so plainly and explain the impact.
