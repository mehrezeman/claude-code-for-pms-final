# 04 · X-Ray Vision — session log

## 2026-10-15

### Goal
Confirm, in the actual routing/scoring code, exactly how the 4.2 release caused some responders to stop getting pinged — and package the findings into a shareable artifact.

### Findings

| Finding | Evidence and source | Confidence | Status |
|---|---|---|---|
| Ping answer window cut 90s → 60s in 4.2 | config.py (`OFFER_TIMEOUT_SECONDS`), CHANGELOG.md | Observed | Confirmed |
| Acceptance rate fell 77% → 64% after 4.2 | rook-database `pings` (831/1085 pre, 391/611 post, split 12 Aug) | Observed | Confirmed |
| Unanswered (missed) pings jumped 2% → 18% after 4.2 | rook-database `pings` | Observed | Confirmed |
| The four responders' home-turf ping share fell from as high as 100% to 16–28% | rook-database `pings` + `callouts` + `responders` joined on area | Observed | Confirmed |
| When they *are* pinged, the four still rank 1st–2nd on average — they're excluded from the sequence entirely on most callouts, not buried at the bottom of a long one | rook-database, ping position via `ROW_NUMBER() OVER (PARTITION BY callout_number ORDER BY sent_at)` | Observed | Confirmed |
| Recent-acceptance score: −0.12 per miss/decline, +0.08 per take, floor 0.0, ceiling 1.0, no drift back to neutral | history.py, config.py | Observed | Confirmed |
| `record_declined` (history.py) is the *only* place the score drops; `record_accepted` is the *only* place it rises, anywhere in the repo | grep across every .py file in the repo | Observed | Confirmed |
| The 4.2 proximity/acceptance reweight was shipped as one global constant with no carve-out for responders already turning jobs down — flagged as an open, unanswered question by the engineering manager on the 4.2 wiki page two days after ship | rook-wiki, 4.2 release page comments | Observed | Confirmed |
| A responder's score is keyed only on `responder.name` in an in-memory dict — a brand-new account/identity would reset it to neutral (0.5) | history.py `_scores` dict | Observed | Confirmed (mechanically — not a recommended fix) |
| Pings are funnelling toward already-high-scoring responders as others go quiet: The Gale's per-day ping rate rose after 4.2 while Meteor Mite (same handler, Kip) collapsed | rook-database `pings`, per-responder daily rate pre/post 12 Aug | Observed | Confirmed |

### Corrections

- What we believed: the four starved responders were simply ranked lower *within* each callout's offer sequence.
- What we found: when they are in a callout's sequence at all, they still rank 1st–2nd on average. The real mechanism is that they're left out of the sequence entirely on 70–85% of their own area's callouts.
- Why: ping position only reflects callouts they were already a candidate for — it can't see the callouts where someone else outranked them before any offer went out at all.

### Decisions

- Did not recommend a new responder account as a fix for a quiet responder, even though it would mechanically reset the score — it discards legitimate history and is a gaming loophole, not a fix. Recommended the drift-to-neutral code fix (Order #1 in the case file) instead.
- Built the requested deliverable as a standalone comic-styled interactive artifact rather than a plain report, per the explicit design spec given.
- For the wrap-up v2 install: left `course-setup/` and `.claude/settings.json` untouched rather than renaming them to `-old`, since they were confirmed byte-identical to the classmate's versions and renaming would only add clutter — flagged as a judgment call for the student to overrule if they'd rather have a backup copy anyway.

### Deliverables

- **Case File 4.2** — private Claude artifact: https://claude.ai/artifact/XqdcwjZooyBfrcUwjTi6Ne
- Source file: `00-rook/case-file-4.2.html`
- `wrap-up` skill upgraded to v2 at `.claude/skills/wrap-up/SKILL.md` (previous version kept at `.claude/skills/wrap-up-old/SKILL.md`)

### How to reproduce

- Acceptance / missed rates: `SUM(outcome='taken'|'missed')` over the `pings` table, split at `sent_at < '2026-08-12'` vs. `>=`.
- Home-turf ping share: join `pings` → `callouts` (on `callout_number`) → `responders` (matching `callouts.area = responders.area`), count distinct callouts pinged vs. total callouts in that area, split pre/post 12 Aug 2026.
- Ping position: `ROW_NUMBER() OVER (PARTITION BY callout_number ORDER BY sent_at)` on `pings`.
- Code mechanism: `history.py` (`record_accepted`, `record_declined`, `_scores`), `config.py` (`ACCEPTANCE_CREDIT`, `DECLINE_PENALTY`, `OFFER_TIMEOUT_SECONDS`, weights), `offer.py` (the dispatch loop), `routing.py` (the score formula).

### Open questions

- Do the four responders' actual live recent-acceptance scores sit near the floor, confirming the mechanism end-to-end? The scores are in-memory only, not queryable. — **Ask Wen Li.**
- Was the proximity reweight's effect on already-low-scoring responders a deliberate decision or an oversight? The engineering manager's question on the 4.2 wiki page was never answered on record. — **Ask Marcus Oyelaran / Wen Li.**
- Have responders starved of pings this long started disengaging or quitting outright? Not visible in any data pulled this session. — **Ask Nadia Hoffmann / Support.**

### Prompts and moves that worked

- Asking "find me every single thing in this code that puts points back on," not just the one obvious spot — grepping the whole repo turned up that there is genuinely only one credit path, making the imbalance concrete instead of assumed.
- Challenging "wouldn't the low scorers never get back up the list" pushed the analysis from a single-responder problem into a pool-wide feedback-loop finding, corroborated by The Gale's rising ping rate.
- Asking for a short, one-sentence conclusion after a long investigation kept the final takeaway legible.

### Start here next time

1. `00-rook/code/dispatch-routing/history.py` — the scoring mechanism (`record_accepted` / `record_declined` / `_scores`).
2. The Case File 4.2 artifact (https://claude.ai/artifact/XqdcwjZooyBfrcUwjTi6Ne), Exhibit 5 — the Hero Health Lab, for the live scoring simulation.
3. The open questions above — Wen Li's live scores are the one piece that would move this from strong hypothesis to confirmed.
