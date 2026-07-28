# Brief: Generate AI Adoption Plan Deck from Markdown

## Task

Create a PowerPoint presentation from `AI_Adoption_Plan_Network_Solutions_v2.md` using the attached Mastercard template. The deck is for presenting the Network Solutions AI adoption plan to leadership (Shrikant Belan as sponsor, Sanjay and Sanjeev as stakeholders) ahead of the September funding review.

## Inputs

- Content source: `AI_Adoption_Plan_Network_Solutions_v2.md` (single source of truth, do not add facts not in this file)
- Template: the attached Mastercard .pptx/.potx template

## Template rules

1. Open the template first. Extract and reuse its slide layouts, master, theme colors, and fonts. Do not introduce any color, font, or style not defined in the template.
2. Build every slide from an existing template layout (title slide, section header, title and content, two content, table layout if present). Do not create blank slides with manually positioned text boxes if a matching layout exists.
3. Keep all Mastercard branding elements from the master (logo, footer, slide numbers) untouched.
4. If the template has a divider/section layout, use it to separate the three parts of the deck listed below.

## Slide structure

Target 11 slides. One idea per slide, no slide with more than 6 bullets, no bullet longer than 2 lines.

**Part 1: Why**
1. Title slide: "AI Adoption Plan: Network Solutions" with presenter name Neha, date July 2026, sponsor and stakeholders as subtitle text.
2. Purpose: the problem (ad hoc adoption, untracked usage, no shared infrastructure) and the one-line proposal. Include the alignment note: endorsed at the AI champions review on 28 July 2026 and aligned to the AI-native SDLC governance direction.
3. Current state: what the team has built (feature-delivery skill in Claude Code, PR-gating usage tracker, pre/post AIDLC metrics dashboard in progress), piloted on COB. Include the champions review outcome bullets: pilot results, convergence mandate with Treasury Services, distribution via the shared skills repo owned by Maveric Xhang.

**Part 2: What and How**
4. Objectives: the 5 objectives, compressed to one line each.
5. Phased rollout overview: a single visual of the 4 phases with names and durations (use a horizontal process/timeline graphic from the template if available, otherwise a 4-column layout).
6. Phase 1 and 2 detail: key deliverables only. Phase 1 must show convergence with Treasury Services and merge into the shared skills repo. Phase 2 shows pilot teams, friction tracking, weekly syncs.
7. Phase 3 and 4 detail: org-wide rollout, payments group presentation, standing reporting, evidence-pack direction, skill metadata governance.
8. Governance: AI usage tracking. The 4 tracking bullets plus the MCP inventory line. Frame the tracker as the credibility and calibration mechanism.

**Part 3: Proof and Ask**
9. Risks: render the risks table using the template's table style. Keep all 6 rows, compress wording to fit without shrinking font below template default.
10. Success metrics and timeline: metrics as bullets on the left, the timeline summary table on the right (use a two-content layout). The pre/post AIDLC dashboard is the headline metric view for September.
11. Ask: the 3 asks, verbatim intent from the MD, one line each. Close with the September funding review connection.

## Content rules

- Pull all content from the MD. Compress, do not paraphrase into new claims.
- No em dashes anywhere in the deck.
- Direct language, no filler, no hedging.
- Dates and names must match the MD exactly: 28 July 2026, Maveric Xhang, COB (Clear on Behalf), Treasury Services POC.
- Add slide notes for slides 3, 8, and 11: 2-3 sentences of speaker context drawn only from the MD.

## Verification before finishing

1. Render every slide to an image and visually check: no text overflow, no overlapping elements, table fits, template branding intact.
2. Confirm slide count is 11 and every slide uses a template layout.
3. Confirm zero em dashes with a text scan of the final file.
4. Output file: `AI_Adoption_Plan_Network_Solutions.pptx`
