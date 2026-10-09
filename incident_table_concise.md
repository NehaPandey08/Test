# Product Team Incidents — Summary Table
## Sept-Oct 2026 (One Quarter)

| # | Date | Incident | Result/Impact | Note |
|---|------|----------|---------------|------|
| 1 | Aug-Sept | Invalid defect rewritten instead of closed | Destroyed audit trail; Neha labeled "stubborn" for asking proper closure | Establishes override pattern |
| 2 | Early Sept | Wrong repo shared; Aman insisted on correctness until third party disproved on live call | Engineering spent time on wrong codebase; chat record never corrected | Pattern starts: product wrong, not acknowledged |
| 3 | Early Sept | 500 observations misclassified as defects (Apurva documented gap at sign-off) | 3 devs spent 8+ hours re-proving documented issue; product did no RCA | 24 dev-hours wasted; same pattern as #2 |
| 4 | Sept | Ticket 802: Features 919, 920 silently absorbed into 8-point story | No re-estimation, no new tickets, no notification | Scope creep absorbed silently |
| 5 | Sept | Enum CEP behavior changed silently, no documentation | Engineering discovered post-implementation | Undocumented requirement change |
| 6 | Sept | Legacy defects logged on pre-August behavior, only flagged post-August | Moving goalpost: what was accepted becomes defect | Retroactive accountability |
| 7 | Sept | Unprioritized cloning feature logged as defect when unbuilt | Engineering blamed for not building non-prioritized work | Defect misclassification hides scope gap |
| 8 | Sept | Email questions unanswered during implementation; defect raised later saying "should have asked at implementation time" | Catch-22: unanswered questions, blamed for not acting on them | Communication failure blamed on dev |
| 9 | Sept | Implicit expectation engineering infers missing features (team is 5/6 < 6 months tenure, 2 < 3 months) | Junior engineers held to impossible standard | Inference model requires institutional knowledge |
| 10 | Sept | No RCA after repo mismatch proven (Incident #2) | Same issues repeat; no learning loop | Product accountability gap |
| 11 | Oct 6-8 | 61 SP feature: conflicting timelines (Oct end → Oct 14 → Oct 13, same day, hours apart) | Delivery expectations unclear; Ankit had to request redraft | Dates shifted without explanation |
| 12 | Oct 6, after 6:15 PM call | Priority change without notification: P1s changed to P0s, team not told | Engineering team planned for wrong priority; Ankit questioned engineering's prioritization | Unilateral changes, no transparency |
| 13 | Oct 9 | Blocker escalation: Developer blocked on product clarification; Neha called out blocker professionally; Ankit publicly escalated to Shrikant Belan | Legitimate blocker call treated as insubordination | Private agreement → public escalation |
| 14 | Oct 9, 8:30 PM | Ankit scheduled call with Neha at 8:30; instead pulled Shrikant (dev) + Pavan (tech lead) + Aman into separate call; Neha not informed until call was happening | Call with developers held without manager present; business requirements discussed without manager context | Circumventing management structure; Aman & Pavan quickly liked then unliked announcement (knowing behavior) |

---

## Incident #14 Follow-Up Details

**Timeline:**
- 8:30 PM: Ankit's announced call with developers instead of scheduled call with Neha
- 8:52 PM: Neha asked Shrikant on group if he got answers and can proceed
- 9:01 PM: Shrikant responded: "No. Will be 11 AM Monday. We discussed the business part of requirement."
- Immediately after: Neha asked Shrikant personally for details on what clarity was received
- 10 minutes later: Shrikant went offline without responding; didn't even read the message

**Significance:**
- Call with developers held without manager presence or knowledge
- Call achieved no clarity ("answers on Monday")
- Developer was not instructed to report back to manager
- When manager asked direct question, developer avoided answering and went offline
- This establishes pattern: product talks directly to developers, bypassing manager

**Behavior markers:**
- Aman & Pavan liked announcement, then quickly unliked (they know this violates protocol)
- Shrikant refusing to report conversation to his manager
- Setting up Monday 11 AM call without involving Neha in planning

---

## Pattern Summary

**Pattern A:** Defect misclassification to hide scope gaps (Incidents #3, #7, #9)  
**Pattern B:** Documentation not read/provided, then used as evidence (Incidents #2, #3, #8)  
**Pattern C:** No RCA when product fails (Incidents #2, #3, #10)  
**Pattern D:** Private agreement → public escalation (Incidents #1, #13)  
**Pattern E:** Unilateral changes without communication (Incidents #5, #12)  
**Pattern F:** Timeline/priority contradictions in same day (Incident #11, #12)  

---

## Neha's Attempts to Help (Every Incident)

All 13 incidents involved Neha or her team:
- Offering guidance on why approach was incorrect
- Requesting documentation clarification
- Providing evidence for classification accuracy
- Calling out legitimate blockers

**Response in all cases:** Resistance, characterization (labeled "stubborn," "lacks support," "not collaborative"), not engagement on substance.

**One exception:** After Neha agreed to Aman's proposed solution (Incident #1), labels continued anyway. Shows labels are used for power dynamic, not response to behavior change.

---

## Cost to Engineering Team

- 24+ unplanned dev-hours re-proving documented information
- Constant context-switching on scope changes & unanswered questions
- Junior engineers (5/6 < 6 months tenure) held to impossible standard
- Retroactive accountability for undeclared work impacts appraisals & retention

