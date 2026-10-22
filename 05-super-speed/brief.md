# Brief: Fix for Quiet Responders

For Helen. One page. v4 — after three rounds of her feedback.

## Exec summary

A responder named **Meteor Mite** went quiet after missing a few calls in
August — three others are stuck the same way. Rook's rules never let a
quiet stretch fade, so it kept calling them less and less, forever. The
fix and the screens below ship **together, as one change** — not a number
first and a nicer screen later.

## Decisions we need from Helen

| # | Decision | Recommendation |
|---|---|---|
| 1 | Approve the fix's **shape**: close the gap between what a miss costs (−0.12) and a take earns (+0.08), let standing drift back to normal after a quiet stretch, and alert Kip when a score starts falling — not only once recovery begins. This is correct regardless of what team item #1 finds — that item only sets the fade *speed*. | Approve the shape now; speed comes later |
| 2 | Leave the 60-second answer window unchanged for now. Revisit it specifically if missed-pings are still above 10% six weeks after this ships (today: 18%; before 4.2: 2%). | Approve |
| 3 | Prototype stays one pair — Kip + Meteor Mite. All four affected responders are named below; not building four full screens for a rough mockup. | Our recommendation — final call is yours |

## Open items/decisions we need from team

| # | Item | Who | Blocks |
|---|---|---|---|
| 1 | Was hurting low-scoring responders on purpose, or a mistake? Still unanswered on the wiki. | Marcus, Wen Li | Item 2 |
| 2 | Pick the real fade-back speed (placeholder used below: 3 weeks). | Wen Li, Marcus | Waiting on item 1 |
| 3 | Close the account-reset loophole: today's score is keyed to a responder's **display name**, so a new name resets it. Re-key it to their permanent account ID instead — a cover identity can change, the account can't. | Wen Li | Ships with this fix, not after |
| 4 | Before ship: does a change in ping patterns affect Supply's maintenance scheduling? Not yet assessed. | Marcus, to route to Supply | Ships with this fix |
| 5 | Two bugs staying separate from this fix: the filter-persistence console bug, and no way to see a live score without asking Wen Li directly. | Marcus | — |

## Problem we are solving for and why

Meteor Mite missed some calls. Rook's system never forgives a miss, so
they got called less and less, forever. Kip sees nothing — no reason, no
warning, no fix in sight. This is not one bad apple: four responders are
stuck the same way, and their pings are quietly piling up on others.

| Responder | Local jobs, before | Local jobs, after 4.2 |
|---|---|---|
| Farlight | 100% | 19% |
| The Undertow | 100% | 28% |
| Vesper | 100% | 26% |
| **Meteor Mite** | 52% | **16%** |

## Ideal state and how we know it is going to work

**Kip's screen:**

| | Today | The moment it starts (new) | After the fix |
|---|---|---|---|
| What Kip sees | Nothing. Just silence. | A flag after 2 declines/misses in a row with no take between them — before they go fully quiet. | "Meteor Mite is recovering — back to normal rotation in about 3 weeks (placeholder)." |
| What Kip can do | Guess, or shrug. | Same button, used early: **"Confirm still available."** | Same button again, to close the loop once they're back. |

*The button never touches ranking — it only updates the Responder Availability
Record (the one Supply also reads; see team item 4).*

**Meteor Mite's screen:**

| | Today | Day 1 of the fix | A few weeks later |
|---|---|---|---|
| What they see | Nothing. No calls, no reason. | Standing starts climbing immediately from wherever it is today — no reset, the climb just begins. | "You're back in the running for local jobs." Plain language, no raw number. |
| How it feels | Stuck, with no way out. | Moving, for the first time in weeks. | Getting better on their own, without needing a lucky call first. |

*Why no visible score:* a number invites gaming and anxiety, and a number
is itself setting-like — the opposite of what Helen asked for.

**Who could end up worse off, and the guardrails:**

| Group | Risk | Metric | Threshold (proposed) | Owner | Review |
|---|---|---|---|---|---|
| Handlers already filing missed-ping complaints | Re-pinged quiet responders may be slower on time-critical callouts | Time-to-accept | No more than 10% above the 4-week average right before launch | Marcus | Weekly, first 6 weeks |
| High-scorers like The Gale | Lose some of the extra pings they'd been absorbing | **Their own weekly ping volume** (not coverage gap — wrong metric for this risk) | No more than 15% below their pre-launch weekly average | Marcus | Weekly, first 6 weeks |
| Everyone | Fix doesn't work as intended | Overall acceptance rate | Must not fall below 60% (today: 64%; before 4.2: 77%) | Ravi Menon | Weekly, first 6 weeks |

*Breach any one: pause the rollout and bring it back to Helen.*

**How we'll know it worked:** three separate clocks — first visible
movement across the group within 2 weeks; Meteor Mite specifically back to
normal rotation around 3 weeks; the full group back near **their own**
pre-4.2 baseline (not the same number for everyone — Meteor Mite's was 52%,
the other three's was 100%) within 6–8 weeks.

## Supporting data

| What we measured | Before 4.2 | After 4.2 |
|---|---|---|
| Overall acceptance rate | 77% | 64% |
| Pings that got no answer at all | 2% | 18% |
| 4 quiet responders' share of local jobs | up to 100% | 16–28% |
| Meteor Mite's share of local jobs | 52% | 16% |

| Rule today | Value |
|---|---|
| Points lost for 1 miss | −0.12 |
| Points gained for 1 take | +0.08 |
| Points gained for just waiting | 0 |
| Time to answer a call | 60 seconds (was 90) |

## Open questions with owners

| Question | Owner |
|---|---|
| What is Meteor Mite's real, live score right now? | Wen Li |
| Does their skill set (capability tags) play any part? | Wen Li / Marcus |
| Is anyone else quietly stuck like this, not yet noticed? | Nadia Hoffmann / Ravi Menon |
| Is "Availability Confidence" still coming, or dropped? (her own roadmap item — separate from this fix, doesn't block it) | Helen |

**Noted, not required to ship:** nothing here guards the wide-area
proximity gain 4.2 was originally trying to deliver — worth a metric later,
not blocking this. Same for the 6-week gap before revisiting the 60s
window — if it matters, flag it and we'll shorten the review window.
