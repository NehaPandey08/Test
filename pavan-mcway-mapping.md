# Incident Log Mapped Against The Mastercard Way

The Mastercard Way defines nine leadership behaviors: **Say What You Mean, Delivering Scalable Solutions, Think Big and Bold, Innovate with Intention, Bring In Different Perspectives, Help Each Other Be Great, Practice What Matters, Own the Outcome, Learn and Pivot.**

Mapped below: which behavior(s) each incident falls short of, and why.

| # | Incident (short) | Mastercard Way behavior(s) not shown | Why |
|---|---|---|---|
| 1 | Raised voice, interrupted Neha with freshers, called it a "repercussion" | **Say What You Mean** | Say What You Mean is explicit that direct communication must come from honesty and kindness together, not abruptness; framing his own conduct as a "repercussion" of someone else's action is the opposite of owning a communication choice plainly |
| 2 | "You are not my manager," "same level," felt "attacked" over a Jira question | **Say What You Mean** · **Help Each Other Be Great** | Positioning a routine clarification as a personal attack, rather than engaging directly on the substance, undermines the safe, direct team environment both behaviors call for |
| 3 | Claimed process changes caused delay, no specifics when asked | **Own the Outcome** | Textbook example of the behavior's own contrast: "focus on what's within your influence and own the outcome" vs. "blame or expect others to solve the problem" — an unspecified claim of external fault is the latter |
| 4 | Told scope for release plan should come from Neha as manager | **Own the Outcome** | Waiting for someone else to define the work before taking it on is the "victim waiting for others to deliver" pattern the behavior specifically warns against |
| 5 | 6 SP estimate for wireframes, effectively proposing prototyping for an in-flight project | **Practice What Matters** | The behavior is explicit about resisting "10 out of 10" perfectionism in favor of the minimum needed to deliver value — building working code to represent what a static wireframe should show is the inverse of that discipline |
| 6 | Told product Neha's processes were delaying a phase that was delayed before she joined | **Say What You Mean** · **Own the Outcome** | Not transparent or accurate communication, and displaces responsibility onto someone else rather than owning the actual (pre-existing) cause of the delay |
| 7 | "Not sure this is my scope" when asked to update a shared sheet in front of product | **Own the Outcome** | Declining ownership in front of the stakeholders it affects, rather than resolving it directly |
| 8 | Resisted taking a defect despite an offer to shift release timing by 2 days | **Own the Outcome** · **Practice What Matters** | Resistance to a clearly prioritized ask, even after the stated blocker was removed |
| 9 | Asked whether someone else could take the work; Shrikant completed it | **Own the Outcome** | Deflected ownership of a committed deliverable to another person rather than seeing it through |
| 10 | Reintroduced hard delete despite concerns already raised | **Learn and Pivot** | The behavior centers on incorporating feedback and adjusting course; reverting to a previously flagged approach without addressing the concern is the opposite of pivoting on new information |
| 11 | Approved a DDL PR without verifying code, missed bigint/bigserial issue | **Delivering Scalable Solutions** | The behavior is defined by "asking the tough questions" and anticipating what could go wrong before it ships — this is the direct failure mode it describes |
| 12 | Joined deployment-day call at 11:37 AM after being online since 10:30 | **Own the Outcome** | Reduced presence during a live, high-stakes deployment is a passive rather than an owning posture toward the outcome |
| 13 | Rollback plan not included in release plan despite being raised | **Delivering Scalable Solutions** | Anticipating failure modes (rollback) is core to the "what could possibly go wrong" mindset the behavior names explicitly |
| 14 | Agreed to notes table design, challenged it days later with no supporting data | **Think Big and Bold** · **Say What You Mean** | The behavior calls for opinions to be voiced "with some backing, with some thought behind it" — reopening a settled decision without evidence doesn't meet that bar, and contradicts what was previously and clearly agreed |
| 15 | Known defect missing from Ankit's requested list; asked Neha to add it | **Own the Outcome** | Passed a gap in his own tracked work back to his manager rather than resolving it |
| 16 | Didn't set up local Postgres until after a related deployment failure | **Delivering Scalable Solutions** · **Own the Outcome** | A direct instruction meant to catch exactly this class of failure went unactioned until the failure occurred |
| 17 | Postgres setup, once done, consumed capacity needed for the next release | **Practice What Matters** | Prioritization discipline — sequencing work so it doesn't compound cost during a critical window — is the specific behavior this misses |
| 18 | Blamed "AI" for an incorrect XSD when asked how a seeding error occurred | **Own the Outcome** | Attributing a technical error to a tool rather than the person directing and reviewing it is a direct instance of the "waiting for others (or something else) to respond" pattern the behavior contrasts with ownership |
| 19 | Gave PR review comments without reviewing the code | **Delivering Scalable Solutions** | Undermines the core diligence ("ask the tough questions," look "around corners") the behavior is built on |
| 20 | Committed an audit feature to product, didn't deliver, didn't disclose the commitment | **Say What You Mean** · **Own the Outcome** | Both a transparency failure (commitment not communicated) and an ownership failure (not delivered, and only surfaced when the other party raised it) |
| 21 | Held up the recovery PR requesting a call/walkthrough rather than raising written concerns | **Own the Outcome** | Introduced delay on work already covering a gap he'd created, rather than engaging directly and efficiently |
| 22 | Self-assigned a non-priority feature over the Sept release scope | **Practice What Matters** | Directly counter to the behavior's core discipline of aligning personal work to what the business has prioritized |
| 23 | Claimed unpaged=true "may have been a requirement," couldn't produce it; reframed the design flaw as expected defect analysis | **Say What You Mean** · **Own the Outcome** | An unverifiable claim followed by reframing a design mistake as procedural work is neither transparent nor an owned acknowledgment of the gap |
| 24 | Told twice to move a ticket to backlog, closed it as done both times; left a comment implying Neha directed the closure | **Say What You Mean** · **Own the Outcome** | Direct instruction not followed twice; a written comment that uses accurate words to imply an inaccurate conclusion is a transparency failure, not just a compliance one |
| 25 | On leave with no intimation during active release work | **Help Each Other Be Great** | Basic team coordination and consideration for those depending on his availability, missing here |
| 26 | High-priority backend item unaddressed for 3 working days; blamed the gap on Kunal, who was never assigned that piece | **Own the Outcome** · **Say What You Mean** | A prioritized, explicitly assigned item went undone, and the explanation given when questioned didn't match the actual task split |
| 27 | XSDs (source of truth for implementation) wrong on enum values; "AI created it" given twice instead of confirming whether he'd verified the output | **Delivering Scalable Solutions** · **Own the Outcome** | No evidence of the "what could go wrong" diligence the behavior calls for on a foundational artifact, and ownership of the output deflected rather than confirmed or acknowledged |

---

## Summary by behavior (frequency)

| Mastercard Way behavior | Times implicated |
|---|---|
| Own the Outcome | 18 |
| Say What You Mean | 9 |
| Delivering Scalable Solutions | 6 |
| Practice What Matters | 4 |
| Help Each Other Be Great | 2 |
| Think Big and Bold | 1 |
| Learn and Pivot | 1 |
| Innovate with Intention | 0 |
| Bring In Different Perspectives | 0 |

**Own the Outcome** is by a wide margin the most consistently absent behavior across this record — the training material's own framing (choosing to "take responsibility" vs. "wait for others to respond or deliver," and "blaming or expecting others to solve the problem") describes the pattern in this log almost exactly.
