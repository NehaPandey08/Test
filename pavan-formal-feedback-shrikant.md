# Performance Feedback: Pavan Verma (Tech Lead, ModelHub)
**Submitted by:** Neha Pandey, Delivery Manager, ModelHub
**For:** Shrikant Belan (Pavan's reporting manager)
**Period covered:** 20th July – present (approx. 12 weeks)

---

## Context

I work with Pavan day to day as delivery manager for ModelHub. This feedback is based on direct, first-hand observation, documented contemporaneously as incidents occurred, and consolidates what was shared with Shrikant in an interim escalation on 2nd September, what has occurred since, and where things stand now. The intent of this submission is forward-looking: to set out clearly what needs to change, what support is needed to get there, and what I'm seeing since that escalation that is encouraging but not yet established.

## Overall assessment

Pavan is technically capable, but his depth and judgment currently lag behind what is expected at his level, and this shows up consistently as a preference for the easier fix over the correct one. The clearest recent examples:

- In a database design discussion, he proposed a reference table pointing to the data table it is meant to describe, the inverse of how that relationship should be modeled, and one that would make the reference table's own meaning dependent on scanning the larger table rather than the other way around.
- He proposed implementing an authorization/role mechanism at the UI layer rather than the backend, which would leave every API accessible regardless of role, since enforcement at the UI alone does nothing to protect the actual endpoints.
- As part of a performance fix, he proposed changing the API response object to return less data to the client, rather than reducing what is read from the database in the first place, treating the symptom at the transport layer instead of the actual source of the performance cost.

Each of these was caught before being implemented, but the pattern of defaulting to the shallower fix, rather than reasoning to the correct one, is the core development area for Pavan at this point, more fundamental than any single incident.

## Development area 1: Ownership of outcomes

Scope and decisions are repeatedly deferred back to me rather than owned, even when Pavan has what he needs to act. When work doesn't go to plan, responsibility tends to be placed elsewhere: on AI tooling ("AI created it" when asked who built an incorrect XSD), on a teammate (a high-priority item he hadn't progressed was described as being "with" a colleague who had only been asked about an unrelated part of the same ticket), or on circumstance (telling the product team that my processes were the cause of a delay that predated my joining the team). Commitments made directly to product were, on at least one occasion, not delivered and not disclosed to me, surfacing only when product raised it directly.

**What I need to see going forward:** when a ticket or decision is assigned to Pavan, ownership through to resolution, including surfacing blockers proactively rather than attributing outcomes elsewhere after the fact.

## Development area 2: Technical depth and judgment

Covered above. In addition to the three recent examples, this has shown up in approving a database migration script without catching a data-type issue that went on to contribute to a deployment failure, and in a map-version table design review where, when pressed by Shrikant on what a newly proposed table would actually contain, Pavan's answer was "I haven't given a thought to it."

**What I need to see going forward:** design proposals that have been reasoned through before they're brought to review, including how the proposed change would actually be read and used, not just how it looks at the point it's proposed.

## Development area 3: Representing and advocating for the team

As tech lead, Pavan has historically been the one brokering discussion and agreement with the product team, though these conversations were rarely documented. When product's stated requirements shifted or were misremembered, the expectation was that Pavan, having carried those original conversations, would correct the record on the team's behalf. On more than one occasion, he did not, and the team was left to absorb blame for a requirement that had, in fact, never been agreed as described. This only surfaced when I asked him directly whether the requirement had actually been agreed; he confirmed it had not.

**What I need to see going forward:** when Pavan is the one who holds the context of what was actually agreed with product, he needs to speak up for the team proactively, not only when directly asked, and he needs to back his team when they are not confident enough to push back themselves.

## Development area 4: Professional disagreement and engagement

Pavan needs to develop a more professional way of handling disagreement or disappointment with a decision. On occasions where a call has gone against what he proposed, most notably during the September release, this has translated into visible disengagement from the work itself rather than a professional disagreement followed by continued full commitment. Disagreeing with a decision is reasonable and expected; disengaging from delivery because of that disagreement is not, particularly during an active release.

**What I need to see going forward:** disagreement raised directly and professionally at the time a decision is being made, and once a decision is taken, full engagement with delivering it regardless of whether it was his preferred outcome.

## Recent trajectory

Since the 2nd September escalation to Shrikant, I have seen genuine signs of improvement in Pavan's behavior. This is encouraging, and I want that noted clearly rather than only focusing on what preceded it. That said, the change is recent, roughly a few weeks against a pattern that held for twelve, and I'm not yet treating it as established. I plan to keep observing consistency over the coming weeks rather than close this out on the strength of early signs alone.

## What I'm asking for

I'd like to align with you on what "good" looks like for Pavan from here, and on a reasonable timeframe over which we'd expect the improvement to hold before considering this resolved. I'm not looking to escalate further at this point, given what I'm seeing recently, but I did want you to have the full, consolidated picture, including the most recent technical examples, so we're assessing progress from the same baseline.

---

*P.S. For reference, the interim escalation sent on 2nd September documented over two dozen individual incidents across the preceding six weeks, roughly one per working day. The examples raised in this feedback from September onward are already reflected in that record and are not being counted twice.*
