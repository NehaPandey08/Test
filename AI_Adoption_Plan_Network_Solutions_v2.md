# AI Adoption Plan: Network Solutions

**Prepared by:** Neha, Manager of Software Engineering
**Sponsor:** Shrikant Belan
**Stakeholders:** Sanjay, Sanjeev
**Date:** July 2026

## 1. Purpose

Network Solutions currently has no consistent approach to AI-assisted development. Adoption is ad hoc, usage is untracked, and there is no shared infrastructure that individual teams can build on. This plan proposes a phased rollout that starts with the tooling the team has built and proven on Clear on Behalf (COB), one of our products, and extends it across Network Solutions repos, with governance built in from the start rather than retrofitted later.

This plan also aligns with the direction set at the AI champions review on 28 July 2026, where the AIDLC framework was presented and endorsed, and with the AI-native SDLC governance model outlined in Colin Wynd's draft paper (Treasury Services). The intent is that Network Solutions adoption becomes an early implementation of that org-level framework, not a parallel effort.

## 2. Current State

- The team has built a feature-delivery skill in Claude Code and an AI usage tracking system that gates GitHub PR merges, currently deployed and tested on COB.
- A dashboard is being built to track pre- and post-AIDLC metrics, giving a before/after view of delivery and quality impact.
- A Claude Code PR review skill has been built and packaged for distribution via a marketplace structure, ready for use across other Network Solutions repos.
- The AIDLC framework, which we are building and piloting on COB, was reviewed by the AI champions on 28 July 2026 with a positive outcome: pilot results (2 of 3 defects delivered first-pass, controlled with/without comparison demonstrating the framework's value) and an explicit mandate to converge with the similar Treasury Services POC (Himanshu), which is already in production use.
- The champions endorsed distribution through the shared skills repo created and owned by Maveric Xhang as the org-wide channel, with iteration toward v2/v3 as models and learnings improve.
- No other team in Network Solutions has equivalent tooling. Adoption elsewhere is individual and unmeasured.
- There is no org-level visibility into how much AI-assisted work is happening, what quality impact it has, or where the risk points are.

## 3. Objectives

1. Give every Network Solutions team access to the same baseline AI tooling already proven out on COB.
2. Make AI usage visible and measurable at the org level, not just on the COB pilot.
3. Keep quality and review standards intact as usage scales, using gating rather than trust-based adoption.
4. Do this without creating a parallel bureaucracy: reuse the infrastructure we have already built instead of building new systems per team.
5. Align Network Solutions adoption with the emerging org-level AI governance framework (evidence-based review, skill registry with ownership and versioning, exception-based human review), so this work scales beyond Network Solutions rather than getting rebuilt.

## 4. Phased Rollout

### Phase 1: Codify, Converge, Package (In Progress)
- Feature-delivery skill and PR-gating usage tracker built by the team and running on COB as the first product.
- PR review skill packaged into marketplace format for org-wide distribution.
- Convergence with the Treasury Services POC: single agreed approach combining their production learnings with the AIDLC framework architecture (routing by work type, confidence-gated open questions, context-isolated review agents), presented back to the AI champions.
- Skills merged into Maveric Xhang's shared skills repo with ownership, versioning, and change review defined, rather than maintained as a single-team fork.
- Output: reusable, documented, centrally owned components other teams can install without rebuilding.

### Phase 2: Pilot Expansion (Next 4-6 weeks)
- Select 1-2 additional Network Solutions teams with active repos and willing tech leads.
- Install the PR review skill and usage tracker via the marketplace structure.
- Track adoption friction: setup time, false positives in gating, skill misfires.
- Weekly sync with pilot teams to capture what needs to change before wider rollout.

### Phase 3: Org-Wide Rollout (Following Phase 2 completion)
- Roll the marketplace skill and tracker out to all Network Solutions repos.
- Publish a lightweight onboarding guide (install steps, what gets tracked, what gates a merge).
- Nominate a point of contact per team for first-line support instead of routing everything through the team.
- Present the approach to the payments group, per the champions' direction, as the step toward adoption beyond Network Solutions.

### Phase 4: Governance and Reporting (Ongoing from Phase 3)
- Standing usage report at the Network Solutions level: adoption rate, PRs gated vs. passed, time saved where measurable.
- Pre/post AIDLC metrics dashboard as the primary evidence view: baseline delivery and quality metrics before AIDLC against the same metrics after adoption, per repo and aggregated.
- Feed this into the September funding review, since the tracking data is direct evidence of the initiative's value.
- Quarterly review of what the tracker gates on, adjusted based on false-positive/negative data from pilot and rollout phases.
- Evolve reporting toward the evidence-pack model from the governance paper: each AI-assisted change carries traceable evidence (analysis artifacts, independent AI review report, test evidence, human sign-off at gates), and review attention concentrates on exceptions and low scores rather than uniform inspection.
- Maintain skill metadata in the shared skills repo consistent with the policy mapping model: owner, version, approval status, allowed use cases, defect history.

## 5. Governance: AI Usage Tracking

The gating system is the core of this plan's credibility. Without it, "AI adoption" is just anecdote. With it:

- Every PR is tagged with whether AI assistance was used and what type (code generation, review, both).
- Merge gates enforce that AI-assisted PRs get the same or stricter review scrutiny, not less.
- Aggregated data becomes the evidence base for scaling decisions and leadership reporting, rather than relying on self-reported adoption.
- The tracker also becomes the calibration mechanism the governance framework needs: correlating gate outcomes with production defect escapes over time, so scores and thresholds are validated against reality rather than asserted.

MCP usage is governed alongside skills: a maintained inventory of MCP servers in use (currently Atlassian for Jira and Confluence), including deployment source, auth model, and write scopes, reviewed before any new server is adopted.

## 6. Risks

| Risk | Mitigation |
|---|---|
| Teams resist a system built by another team | Position COB as the pilot product, not the owner; ownership of the marketplace skills moves to the shared skills repo (owned by Maveric Xhang) with named maintainers once merged |
| Duplication with the Treasury Services POC undermines both efforts | Convergence is mandated by the AI champions; single approach agreed in Phase 1 before wider distribution |
| Gating creates PR friction and gets bypassed | Start with soft gates (warnings) in Phase 2, move to hard gates only after false-positive rate is low |
| No time allocated by other teams to onboard | Get explicit sponsor backing (Shrikant, with Sanjay/Sanjeev) before Phase 2 starts, not after |
| Usage data reveals uneven or low adoption | Report it honestly; uneven adoption is itself useful data for where support is needed |
| Scores and gates drift into compliance theater | Tie thresholds to production outcome data via the tracker; keep scores explainable in the PR |

## 7. Success Metrics

- Number of Network Solutions repos with the tracker and PR review skill installed.
- % of PRs across those repos tagged with AI usage data.
- Reduction in review cycle time on AI-assisted PRs vs. baseline.
- Qualitative feedback from pilot team leads by end of Phase 2.
- Converged approach agreed with Treasury Services and merged into the shared skills repo by end of Phase 1.

## 8. Timeline Summary

| Phase | Duration | Owner |
|---|---|---|
| 1: Codify, Converge, Package | In progress; convergence within 2-3 weeks | Neha, with Treasury Services counterpart |
| 2: Pilot Expansion | 4-6 weeks | Neha + pilot team leads |
| 3: Org-Wide Rollout | 4-8 weeks after Phase 2 | Neha + team POCs |
| 4: Governance and Reporting | Ongoing | Neha, reporting to Shrikant/Sanjay/Sanjeev |

## 9. Ask

- Sponsor backing from Shrikant to approach 1-2 pilot teams for Phase 2.
- Support for the convergence sync with Treasury Services and the follow-up presentation to the AI champions.
- Sanjay/Sanjeev visibility into this plan ahead of the September funding review, so the usage tracking data can be positioned as supporting evidence for continued investment.
