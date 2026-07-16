# Brief: Set up org-wide PR review skill via Claude Code plugin

## Context
I need a Claude Code plugin that packages a standardized PR review workflow as a skill, distributed via a private marketplace so every engineer on my team can install it once and use it across all repos (ModelHub, Clear on Demand/RTC, and any future repos), not just one project.

## What to build

Create the following directory structure at `<TARGET_PATH>` (ask me for the path if not already told):

```
marketplace-repo/
├── .claude-plugin/
│   └── marketplace.json
└── modelhub-review-plugin/
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        └── pr-review/
            └── SKILL.md
```

### 1. `.claude-plugin/marketplace.json` (marketplace root)
A marketplace manifest listing one plugin, `modelhub-review`, sourced from the relative path `./modelhub-review-plugin`.

### 2. `modelhub-review-plugin/.claude-plugin/plugin.json`
A plugin manifest with:
- `name`: `modelhub-review`
- `version`: `1.0.0`
- `description`: standardized PR review workflow for ModelHub and related repos (Jira mapping, correctness, dead code, field usage, design patterns)
- `author`: my name
- `keywords`: `code-review`, `pr-review`, `modelhub`

### 3. `modelhub-review-plugin/skills/pr-review/SKILL.md`
A skill invoked as `/pr-review <PR-number>`. Frontmatter: `name: pr-review`, a description telling Claude to use this whenever reviewing a pull request end-to-end, `context: fork`, `agent: Explore` (so it runs in an isolated read-only subagent and doesn't pollute the main session).

The skill body must direct Claude, when run, to do the following, **in this order**, using `$ARGUMENTS` as the PR number:

**Step 0 — Repo-specific criteria (check first, always)**
Look for a file at `.claude/pr-review-criteria.md` in the current repo. If present, read it and apply its contents as additional rules on top of everything below, reporting those findings separately in the final summary as "repo-specific criteria." If absent, proceed silently with the standard review.

**Step 1 — Jira requirement mapping**
Find the Jira ticket ID from the PR title/description/branch name. Fetch acceptance criteria via Jira MCP if available, otherwise ask me to paste them. For each acceptance criterion: state fully satisfied / partially satisfied / not addressed. Flag any PR changes not explained by the ticket (scope creep, unrelated refactors, leftover debug code).

**Step 2 — Correctness & logic**
Walk through each changed file: what changed and why. Identify logic errors, off-by-one issues, null/empty handling gaps, broken edge cases. For mapping/transformation logic (ISO 8583, ISO 20022, NCM/Nexo field mappings) verify field-level correctness: correct field tags, data types, optional/mandatory handling per spec.

**Step 3 — Unused / dead code**
Flag newly added methods, variables, imports, config entries, or DTO fields never referenced elsewhere. Flag unused new dependencies. Flag leftover commented-out code or TODOs without a ticket reference.

**Step 4 — Correct field / API usage**
Check new/modified fields match existing naming, typing, and validation conventions in the codebase. Check for reimplementation of logic that already exists as a utility/helper. Flag hardcoded values that should be config-driven or enum-based.

**Step 5 — Good practices**
Error handling (swallowed/logged/propagated appropriately). Test coverage for new logic and edge cases. Naming/readability against the repo's CLAUDE.md if present. Security (input validation, injection risks, PANs/tokens not logged in plaintext). Backward compatibility for existing API/mapping consumers.

**Step 6 — Reusability & design patterns**
Identify logic generic enough to extract into a reusable utility/service, and which other use cases in the codebase could reuse it. Scan the codebase for design patterns already in use (Strategy, Factory, Builder, Adapter, Chain of Responsibility, Template Method, etc.) and note whether this PR follows or breaks those conventions. For complex conditionals, duplication, or tight coupling in the PR, name a specific pattern that would help and explain how. Give an explicit cost/benefit call per suggestion, current code fine vs. genuinely worth the pattern, don't just list options. Tag each recommendation as "fix now" (cheap, high value) or "flag for later" (valid, out of scope for this PR).

**Step 7 — Summary**
Pass/fail verdict per Jira acceptance criterion. Prioritized issue list (blocker / should-fix / nit). Repo-specific criteria findings, listed separately. Pattern/reusability recommendations split into "worth doing now" vs. "worth a backlog ticket." Anything uncertain that I should verify manually.

**Final instruction in the skill body:** run `gh pr view $ARGUMENTS --comments` first to check whether a review was already posted. Cite specific file paths and line numbers for every issue raised.

## After creating the files
1. Show me the final file tree and contents for confirmation before I push anything.
2. Do NOT run `git init`, commit, or push, I'll handle repo creation and push myself.
3. Ask me for the target path if I haven't specified one, don't assume `/mnt` or any prior session's paths.

## Not in scope for this task
- Do not create the `.claude/pr-review-criteria.md` repo-specific criteria file itself, that's a separate follow-up per repo.
- Do not attempt to install the plugin or add it as a marketplace, this is local scaffolding only.
