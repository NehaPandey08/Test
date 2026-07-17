Analyze this repository end to end and report back in the structure below. Read actual files, don't infer from names alone, if a skill or agent definition exists, open it and base your assessment on its actual instructions/prompt/tools, not just its filename or description.

## 1. Repo overview
- What does this repo implement, in plain terms, and what problem is it solving.
- Directory structure: map it out and explain what lives where.
- Entry points: how does someone actually start using this (a slash command, a skill trigger phrase, a CLI invocation, a hook)?

## 2. AIDLC phase breakdown
For each phase implemented (analysis, story creation, planning, implementation, testing, or however this repo has divided it):
- What triggers this phase.
- What input it expects and what output it produces (file, chat response, commit, PR comment).
- Is there a human approval gate before the next phase runs, or does it chain automatically? Quote the actual instruction that enforces (or fails to enforce) the gate.
- What happens on failure or ambiguity, does it stop and ask, or does it guess and proceed?

## 3. Architecture inventory: skills, agents, hooks, MCP
List every skill, subagent, hook, and MCP server connection used in this repo. For each one:
- Name and location in the repo.
- What it does, in one or two sentences, based on reading its actual definition.
- Why this primitive was chosen (skill vs subagent vs hook vs MCP tool). What's the actual mechanical reason it needs to be a subagent (isolated context, parallel execution) versus a skill (repeatable instructions, shared context)?
- **Critique it:** does the choice of primitive actually fit the job? Specifically:
  - Any subagent that could be a skill instead, flag it and explain the downside of the current choice (unnecessary context isolation, lost visibility, added latency/cost).
  - Any skill that's doing something better suited to a subagent (e.g. long-running isolated work bleeding into the main context window).
  - Any skill or set of skills that would be better packaged as a plugin (bundling multiple skills, hooks, and MCP config together for one-command team install) rather than loose files.
  - Any hook that's doing too much or too little, could logic embedded in a hook be a skill instruction instead, or vice versa.

## 4. Everyday dev workflow integration
- Walk through, step by step, what a developer's actual day looks like using this: where do they type what, what do they see, where do they wait for a gate.
- Where does this sit relative to normal git workflow, does it expect a fresh branch per feature, does it commit automatically, does it open PRs itself.
- What's the friction point for a developer picking up a small, trivial change, does this repo have a lightweight path, or does everything go through the full phase sequence regardless of size?

## 5. Testing integration
- How does the test phase (if present) decide what to test, does it read acceptance criteria from an earlier phase's output, or infer from the diff.
- Does it run existing test suites, generate new tests, or both.
- What happens on test failure, does it self-correct, and if so under what bounds (retry limits, escalation)?
- Is there any code quality/static analysis step (SonarQube, linting, security scanning) wired in, and if so is it a hard gate or advisory?

## 6. Missing opportunities
Be specific and prioritized, not generic. For each gap, state what's missing and what adding it would concretely improve:
- Any phase from the standard AIDLC shape (analyze, story, plan, implement, review, test) that's absent or thin here.
- Governance gaps: is there an audit trail? Can a human see why the AI made a given plan/implementation decision after the fact? Is there any check on the AI touching sensitive files (auth, access control, config) without extra scrutiny?
- Missing quality/review integration: is GitHub Copilot's PR review being read and triaged anywhere? Is there a SonarQube or equivalent static analysis gate?
- Cost/context management: any sign of context window bloat (e.g. huge skill files, no subagent isolation for large read tasks) that would degrade quality on a big codebase?
- Reusability: is this repo-specific and hardcoded, or genuinely portable to another repo/team without rewriting?
- Anything that looks like "agent-washing," a linear scripted workflow being marketed as agentic without real multi-step reasoning or tool use.

## 7. Verdict
- One paragraph: is this a solid foundation to build on, or does it need architectural rework before wider adoption?
- If you were extending this repo tomorrow, what's the single highest-leverage next addition?
