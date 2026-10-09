# Complete Incident Log: Product Team Issues
## Sept 27 - Oct 8, 2026

---

## DEFECT MISCLASSIFICATION INCIDENTS

### 1. Invalid Defect Rewriting (Early Sept)
- **What happened:** Invalid defect logged. Neha provided screenshot + logged requirement proving it invalid.
- **Response:** Instead of closing as invalid and creating new ticket, product team rewrote the entire defect with different description.
- **When challenged:** Labeled as "stubborn" for holding line on document hygiene.
- **Impact:** Broke audit trail, removed accountability record, violated defect hygiene standard.

### 2. Unspecified Requirements Logged as Defects (Post-Aug 15 Release)
- **Pattern:** Multiple requirements that were unclear or unspecified logged as defects.
- **Product position:** Expected dev team to infer what was needed.
- **Neha's push back:** "Requirements gaps are not defects."
- **Result:** Labeled "not collaborative" for distinction.

### 3. Pre-August Behavior Logged Post-August (Sept Release)
- **What:** Behavior existing since August flagged as defect only after August release.
- **Implication:** Retroactive defects on existing functionality.
- **Impact:** Penalizes dev team for not proactively fixing things that were never reported as problems.

### 4. Scope Explicitly Moved Out, Then Logged as Defect
- **What:** Feature was discussed, documented, and explicitly moved out of scope.
- **Later:** Same feature logged as defect when not built.
- **When challenged with documentation:** Product team tried to override the invalid defect marking.
- **Neha's response:** Held line on document hygiene.
- **Result:** Labeled "not collaborative" and "stubborn."

### 5. Email Questions Unanswered, Then Defect Raised
- **Sequence:** 
  1. Dev team asked clarifying question via email
  2. Product team did not respond
  3. Dev team proceeded with best judgment
  4. Product team raised defect saying "you should have asked at implementation time"
- **Issue:** Responsibility shifted to dev team for product's non-response.

---

## REPOSITORY & DOCUMENTATION ISSUES

### 6. Incorrect Repo Shared Without Self-Analysis (Major)
- **What happened:** Product team shared incorrect repository for work.
- **Product owner position:** Insisted repo was correct.
- **Discovery:** Only when third-party co-maintainer confirmed on live call that repo was wrong.
- **Product response:** Still no self-analysis on why they provided wrong repo or how it happened.
- **Impact:** Resulted in 500+ "defects" being logged based on incomplete repo.

### 7. Apurva's Sign-Off Documentation Ignored (500 Observations Issue)
- **What Apurva did:** Attached documentation at sign-off explicitly stating "These items are missing in the repo, so they will show up as observations."
- **Product team action:** Did NOT read the documentation or comments.
- **Result:** Flooded system with 500 defects based on documented gaps.
- **When challenged:** Apurva said "No, that's the documented gap, not defects. The repo is incomplete."
- **Product response:** Kept insisting "repo is right, repo is right" until third party confirmed otherwise.
- **Cost:** 3 dev resources spent 8+ hours analyzing all 500 to categorize which were actual defects vs. expected gaps.
- **Outcome:** Product team still treating as dev failure with zero self-analysis on bypassing Apurva's documentation.

---

## SCOPE CREEP & STORY SIZING ISSUES

### 8. Scope Creep in Ticket 802
- **What:** 8-point estimated story attempted to absorb additional unprioritzed features (919, 920).
- **Issue:** Scope added mid-sprint without ticket update.
- **When pushed back:** Labeled as "not collaborative" for not absorbing scope silently.

### 9. Silent Scope Changes Without Documentation
- **What:** Enum CEP behavior changed without announcement or documentation.
- **Discovery:** Found during QA, not communicated proactively.
- **Impact:** Requires rework to understand what actually changed and why.

### 10. Unprioritized Feature Logged as Defect (Cloning Feature)
- **What:** Cloning feature was NOT prioritized by product team.
- **When unbuilt:** Logged as defect (dev team's fault for not building unprioritized work).
- **Impact:** Redirects dev team resources from roadmap to product's missed prioritization.

### 11. Eight Story Point Story Exception Without Discipline (Pre-Oct 14)
- **Agreement with Shrikant:** Stories ≤ 5 SP standard.
- **Ankit's request:** Allow 8 SP and 16 SP stories for this sprint without breakdown.
- **Neha's agreement:** "Okay, with understanding that overflow spills to next iteration."
- **Later claim:** Ankit asking "why aren't you delivering the 8 and 16 SP stories by Oct 14?"
- **Issue:** Acting like agreement didn't exist; expecting delivery that violates the trade-off he agreed to.

### 12. Sixteen Story Point Story Reseeding (NCM/UNCM)
- **What:** 13-15 SP already spent on NCM/UNCM seeding work.
- **Change:** Requirements changed, requiring all previous work to be undone.
- **New effort:** Need additional 16 SP to reseed with new changes.
- **Blocking:** Three P0s depend on this 16 SP story completing first.
- **Oct 14 feasibility:** Cannot complete by Oct 14 deadline with current capacity.
- **Impact:** 13-15 SP wasted; team now doing rework instead of new features.

### 13. Sixty-One Story Point Feature (Yesterday's Discussion)
- **Feature discussed:** High-level = ~61 SP, will definitely bloat once requirements clarified.
- **Feasibility:** Cannot be completed before Oct 30.
- **Product response:** Still pushing as if it should fit in Oct 14 sprint.
- **Issue:** Capacity math not being respected.

---

## STORY BREAKDOWN & PRIORITIZATION

### 14. Breakdown Provided, Dependencies Ignored
- **Neha provided:** High-level breakdown of 61 SP feature with clear sequential dependencies:
  - Mapping & cloning = 28 SP (first)
  - DD messages/export sync = 9 SP (only AFTER mapping & cloning)
  - History = 16 SP (only AFTER mapping & cloning)
  - Audit = 13 SP total (can start independently)
  
- **Aman's response:** Marked mapping & cloning (28 SP) and DD messages sync (9 SP) as P0, but did NOT reflect:
  - Sequential dependencies
  - Feasibility within timelines
  - Blocked nature of other P0s
  
- **Current status:** Three P0s cannot be completed because prerequisites don't exist.

---

## DOCUMENTATION & DEFECT CLASSIFICATION

### 15. Defect vs. Requirement Gap Distinction Rejected
- **Neha's position:** "Requirement gaps are not defects. Whatever are actual technical defects, we'll take those soon."
- **Product position:** Everything missing = defect, regardless of whether it was requested.
- **When challenged with distinction:** Labeled "not collaborative" and "lacking support."

### 16. Reverse Defect Labeling When Challenged
- **Pattern:** Neha provides RCA showing something is NOT a defect.
- **Product response:** Labeled "lack of collaboration and support" for providing the RCA.
- **Effect:** Evidence-gathering itself triggers the "not collaborative" label.
- **System impact:** Creates environment where providing data is punished.

---

## TEAM IMPACT INCIDENTS

### 17. Apurva's Documentation Effort Wasted
- **What Apurva did:** Detailed sign-off documentation with explicit warnings about missing repo items.
- **Result:** Ignored; had to re-prove the same information 8 hours later.
- **Impact on Apurva:** Her analytical work and documentation discipline not valued; creates disincentive for detailed work.

### 18. Three Dev Resources on Analysis Not Development
- **Time lost:** 8+ hours of dev team analyzing 500 defects that were already documented.
- **What they should have been doing:** Strategic architectural work on ModelHub or AIDLC.
- **Cost:** Roadmap impact, team morale, sense of wasted effort.

---

## PROCESS & COMMUNICATION VIOLATIONS

### 19. Product Team Not Reading Sign-Offs
- **Pattern:** Dev team provides detailed sign-off documentation.
- **Product team action:** Does not read it; proceeds with assumptions.
- **Result:** Rework required; misalignment; blame lands on dev team.

### 20. Burden of Proof Shifted Unilaterally
- **Setup:** "Prove to us it's not a defect" (burden on dev).
- **When dev provides proof:** "Lack of collaboration" (proof itself is punished).
- **Effect:** No path where evidence wins; evidence-gathering is weaponized against.

### 21. Label "Lack of Collaboration" Used to Silence Data
- **When Neha holds line on classification:** "Not collaborative"
- **When Neha provides RCA:** "Lack of support"
- **When Neha challenges without evidence:** "Rigid" / "Stubborn"
- **Pattern:** Every path to protecting data quality triggers a label.

---

## FAILED PRIORITIZATION

### 22. Unprioritized Items Later Called Critical
- **What:** Items not prioritized in earlier cycles.
- **Later status:** Raised as P0/P1 defects that "must be fixed now."
- **Issue:** Product team making prioritization decisions retroactively through defect labels.

### 23. Live Product Prioritized Over Internal Project (Impact)
- **Context:** ModelHub is internal platform; live products get priority in appraisal cycles.
- **Result:** Team working on internal project takes accountability for all gaps, gets lower recognition.
- **Morale impact:** Why stay on internal project when all blame lands on you and live product gets credit?

---

## PROCESS AGREEMENT VIOLATIONS

### 24. Agreement on Story Size Ignored Mid-Sprint
- **Agreement:** No breakdown for this sprint; 8 SP and 16 SP allowed as exception.
- **Trade-off understood:** Overflow to next iteration.
- **Later:** Acting like agreement doesn't exist; expecting full completion by Oct 14.
- **Implication:** Agreements can be unilaterally reversed mid-sprint.

---

## SUMMARY BY PATTERN

**Total incidents: 24**

**By category:**
- Defect misclassification: 7 incidents
- Documentation/repo: 3 incidents  
- Scope creep/sizing: 7 incidents
- Team impact: 2 incidents
- Process violations: 3 incidents
- Failed prioritization: 2 incidents

**By severity:**
- **High impact:** 500 observations incident, repo mismatch, NCM/UNCM rework, 61 SP feature capacity violation
- **Medium impact:** Scope creep (802), story exceptions, unprioritized features as defects
- **Pattern/systemic:** Defect labeling, document ignoring, burden of proof reversal, label weaponization

**Root cause across all:** Product making unilateral decisions, not reading dev documentation, labeling any pushback as "not collaborative," using "collaboration" to mean "accept our narrative without evidence."
