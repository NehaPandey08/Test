# Brief: Build and self-verify the PR review plugin

## Location
Work inside this exact path (create parent folders if missing):
```
~/CodeRepos/Marketplace-Repo/
```

## Target structure to create
```
Marketplace-Repo/
├── .claude-plugin/
│   └── marketplace.json
└── network-solutions-review-plugin/
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        └── pr-review/
            └── SKILL.md
```

## File specs

### 1. `.claude-plugin/marketplace.json`
```json
{
  "name": "modelhub-review-marketplace",
  "owner": {
    "name": "Neha"
  },
  "plugins": [
    {
      "name": "network-solutions-review-plugin",
      "source": "./network-solutions-review-plugin",
      "description": "Standard PR review workflow: Jira mapping, correctness, dead code, field usage, patterns."
    }
  ]
}
```

### 2. `network-solutions-review-plugin/.claude-plugin/plugin.json`
```json
{
  "name": "network-solutions-review-plugin",
  "version": "1.0.0",
  "description": "Standardized PR review workflow for ModelHub, Clear on Demand/RTC, and related repos: Jira mapping, correctness, dead code, field usage, design patterns.",
  "author": {
    "name": "Neha"
  },
  "keywords": ["code-review", "pr-review", "modelhub"]
}
```

### 3. `network-solutions-review-plugin/skills/pr-review/SKILL.md`

Frontmatter:
```yaml
---
name: pr-review
description: Full PR review mapping changes to Jira requirements, checking correctness, dead code, correct field/API usage, good practices, and design pattern opportunities. Use when reviewing a pull request end-to-end.
context: fork
agent: Explore
---
```

Body, in this exact order:

1. **Repo confirmation line** (first line after frontmatter):
   > Before doing anything else, run `gh repo view --json nameWithOwner` and print the result as "Reviewing PR $ARGUMENTS in <owner>/<repo>" so it's clear which repository this review is running against. PR numbers are scoped per repository, so this confirms you're not accidentally reviewing the wrong repo's PR #$ARGUMENTS.

2. **Opening instruction:**
   > Review PR $ARGUMENTS end-to-end. Do not just skim the diff, reason about it against the actual codebase and the linked Jira ticket.

3. **Step 0 — Repo-specific criteria:** check for `.claude/pr-review-criteria.md` in the current repo; if present, apply as additional rules and report separately in the summary; if absent, proceed silently.

4. **Step 1 — Jira requirement mapping:** find the Jira ticket from PR title/description/branch name; fetch acceptance criteria via Jira MCP if available, otherwise ask the user to paste them; verdict per criterion (fully/partially/not addressed); flag unexplained scope creep.

5. **Step 2 — Correctness & logic:** walk each changed file; identify logic errors, null/edge-case handling gaps; for ISO 8583/20022/NCM-Nexo mapping logic, verify field tags, data types, optional/mandatory handling.

6. **Step 3 — Unused/dead code:** flag unused new methods/variables/imports/config/DTO fields, unused new dependencies, leftover commented-out code or untracked TODOs.

7. **Step 4 — Correct field/API usage:** check naming/typing/validation conventions match the codebase; check for reimplementation of existing utilities; flag hardcoded values that should be config/enum-driven.

8. **Step 5 — Good practices:** error handling, test coverage, CLAUDE.md naming/readability conventions, security (input validation, PANs/tokens not logged in plaintext), backward compatibility.

9. **Step 6 — Reusability & design patterns:** identify reusable logic and which other use cases benefit; scan for existing patterns in the codebase (Strategy, Factory, Builder, Adapter, Chain of Responsibility, Template Method) and whether this PR follows/breaks them; name specific patterns for complex/duplicated/tightly-coupled code with an explicit cost/benefit call; tag each as "fix now" or "flag for later."

10. **Step 7 — Summary:** pass/fail verdict per Jira criterion; prioritized issues (blocker/should-fix/nit); repo-specific criteria findings listed separately; pattern recommendations split "worth doing now" vs "worth a backlog ticket"; anything uncertain to verify manually.

11. **Final line:** run `gh pr view $ARGUMENTS --comments` first to check for an existing review; cite specific file paths and line numbers for every issue raised.

## After creating the files

Run a self-test verification pass. Read back all three files and confirm against this checklist, reporting each item as present or missing (do not just say "looks good", check explicitly):

- [ ] `marketplace.json`: name, owner, plugins array with correct `source` path `./network-solutions-review-plugin`
- [ ] `plugin.json`: name matches folder name exactly (`network-solutions-review-plugin`), version, description, author, keywords
- [ ] `SKILL.md` frontmatter has all four fields: `name`, `description`, `context: fork`, `agent: Explore`
- [ ] Repo confirmation line is the first line of the body, immediately after frontmatter
- [ ] All 8 sections present in order: Step 0 through Step 7
- [ ] Step 0 references `.claude/pr-review-criteria.md` specifically
- [ ] Step 2 references ISO 8583/20022/NCM-Nexo specifically
- [ ] Final line references both `gh pr view $ARGUMENTS --comments` and the file path/line number citation instruction

Show me the full contents of all three files plus the checklist results.

## Constraints
- Do NOT run `git init`, commit, or push.
- Do NOT rename any folders beyond what's specified above.
- Do NOT install the plugin or add any marketplace, this is local file creation and verification only.
