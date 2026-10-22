# 05 · Super Speed — session log

## 2026-10-15

### Goal
Build the deliverable Helen asked for (see [director-request.txt](director-request.txt)) — a one-pager plus something clickable showing what a fix to the quiet-responder problem would feel like to a handler and a responder, before engineering touches the routing code.

### Findings

| Finding | Evidence and source | Confidence | Status |
|---|---|---|---|
| Acceptance rate fell 77% → 64% after 4.2 | rook-database, pings table, split at 12 Aug 2026 | observed | confirmed |
| Missed (timed-out) pings jumped 2% → 18% after 4.2 | rook-database, pings table | observed | confirmed |
| Four responders' home-turf ping share collapsed 88% → 22% | rook-database, pings + callouts + responders | observed | confirmed |
| Ping answer window cut 90s → 60s in 4.2 | config.py / 4.2 release notes | observed | confirmed |
| `history.py` penalizes a miss (-0.12) harder than it rewards a take (+0.08), with no drift back to neutral — flagged in an unresolved 2019 code comment | 00-rook/code/dispatch-routing/history.py | observed | confirmed |
| The four responders in sustained collapse are Farlight, The Undertow, Vesper, and Meteor Mite | rook-database live trend past early Sept | observed | confirmed |
| Geography isn't the cause — the four's away-ping rate stayed flat; only their home-area share collapsed | rook-database, pings + callouts | observed | confirmed |
| Engineering manager asked on the 4.2 wiki page (2 days post-ship) whether the proximity reweight was meant to hit responders already turning down jobs — no answer is on record | rook-wiki, 4.2 release page | observed | open |

### Corrections
- **Believed:** the four starving responders were Farlight, The Undertow, Corporal Ashgrove, and Halfmoon. **Found:** Ashgrove and Halfmoon dipped after 4.2 but recovered by late August; the real sustained-collapse group is Farlight, The Undertow, Vesper, and Meteor Mite. **Why:** checking the live trend past early September, not just the initial post-release window.
- **Believed:** Priya's handoff note that the drop was "likely mostly seasonal." **Found:** no prior-year data exists to test that claim, and the drop is a sharp cliff exactly at the 12 Aug release — not a gradual slide — which argues against seasonality.

### Decisions
- Built the deliverable as one interactive HTML case file ([case-file-4.2.html](../00-rook/case-file-4.2.html)) rather than separate brief.md + prototype.html files, so the one-pager and the clickable walkthrough are the same artifact — matches Helen's ask for "something real" she can click through.
- Recommended fixing the scoring bug in `history.py` rather than reverting 4.2, per the student's own stated hunch and consistent with Priya's handoff advice not to let this slide into a "revert 4.2" conversation.
- Set fix priority order: critical = let the recent-acceptance score drift back toward neutral; high = put the proximity-reweight question on record, alert on a falling score; medium = re-open the 60s timeout decision with data, confirm/drop Availability Confidence; low = fix the filter-persistence regression.

### Deliverables
- [00-rook/case-file-4.2.html](../00-rook/case-file-4.2.html) — interactive case file for Helen (one-pager + clickable prototype in one). Private to this repo; not yet pushed to GitHub before this checkpoint.

### How to reproduce
- Acceptance / missed-ping / home-turf numbers: rook-database `pings`, `callouts`, `responders` tables, rows split at 2026-08-12.
- Penalty/reward constants (-0.12 miss, +0.08 take) and the unresolved 2019 comment: `00-rook/code/dispatch-routing/history.py`.
- Hero Health Lab replay (in the case file): starts each responder's score at neutral (0.5) at their first ping in the dataset (29 Jun 2026), replays their real taken/turned-down/missed sequence through `history.py`'s actual formula — a replay of what already happened, not a forecast.

### Open questions
- The four responders' actual live recent-acceptance scores (in-memory only, not in any database) — **ask Wen Li**.
- Whether capability-tag mismatches play any part (tags aren't tracked in the database or wiki) — **ask Wen Li / Marcus Oyelaran**.
- Whether the pattern reaches beyond these four — September interviews also named Vesper and Meteor Mite, now confirmed, but is anyone else drifting unnoticed — **ask Nadia Hoffmann / Ravi Menon**.
- Whether the proximity reweight's effect on these four was a deliberate call or an oversight — the engineering manager's question on the 4.2 wiki page was never answered on record — **ask Marcus Oyelaran / Wen Li**.
- Availability Confidence's real status — committed to 4.2 on the roadmap, absent from what shipped — **ask Helen Achebe**.

### Prompts and moves that worked
- Asking for one responder's week-by-week ping history in plain English, then the same for someone who *didn't* go quiet, made the pattern legible in a way the aggregate numbers didn't.
- "If I'd only asked for the number, not the rows, what would I have missed?" — a self-check that catches an average hiding an outlier group.
- Stating a hunch ("fix the bug, not revert") before asking for a recommendation kept the analysis from drifting toward reverting 4.2, in line with Priya's handoff advice.

### Start here next time
1. [00-rook/case-file-4.2.html](../00-rook/case-file-4.2.html) — Exhibit 6 (Orders for the developer) and Exhibit 7 (Loose ends).
2. [director-request.txt](director-request.txt) — Helen's original ask, to confirm sign-off.
3. `00-rook/code/dispatch-routing/history.py` — where the actual fix lands.

## 2026-10-22

### Goal
Build the actual deliverable Helen asked for — a one-pager and a clickable mockup showing the fix through Kip's and a responder's point of view — and pressure-test both before treating them as ready.

### Findings

| Finding | Evidence and source | Confidence | Status |
|---|---|---|---|
| No brief.md or prototype.html existed before this session, despite the 15 Oct log claiming the Case File already satisfied Helen's ask | `05-super-speed/` directory listing | observed | confirmed |
| CLAUDE.md ties Meteor Mite to Kip as "same handler" — the handler Helen named directly | CLAUDE.md, Working context | observed | confirmed |
| No real Rook console or responder-app design exists anywhere to build a realistic mockup from | rook-wiki, searched "console" and "responder app" | observed | confirmed |
| Round 1 review of brief v1 (3 independent models) converged on 2 critical gaps: the fix still read as "a number," and the fade speed was being approved before Wen Li's intent question was answered | agent reviews, brief round 1 | observed | confirmed |
| Round 2 review of brief v3 converged on one unanimous blocker: guardrails needed real numeric thresholds, not "must not rise" | agent reviews, brief round 2 | observed | confirmed |
| Prototype review round 1 (3 models): all three independently flagged the same invented decisions in the mockup — a "2 misses in a row" trigger, a firm "about 3 weeks" estimate, and an unclear reused confirm button — none of which the brief had actually settled | agent reviews, prototype round 1 | observed | confirmed |
| Prototype review round 2: one model caught a literal contradiction — a note saying "not shown to Kip" was itself inside the panel Kip's own screen displays | agent review (Haiku), verified directly against prototype.html source | observed | confirmed, fixed |

### Corrections
- **Believed** (per the 15 Oct log's own "Decisions" entry): the Case File 4.2 artifact already satisfied Helen's one-pager-plus-clickable ask. **Found:** it doesn't — it's a developer-facing investigation artifact explaining the *problem*; Helen's memo explicitly asks for the *solution* shown through a person's eyes, with no mechanism explained. **Why:** re-reading her original note against what the Case File actually contains.

### Decisions
- Scope: one prototype pair (Kip + Meteor Mite) built, but all four affected responders named in the brief — the split the student chose after the three models disagreed (2 of 3 wanted all four prototyped; the student went with the lighter-weight option and transparency in the brief instead).
- Kept the comic-book identity for the prototype's page chrome (student's explicit choice) but switched in-screen content type to plain app-style font after review feedback that the comic font read as game-like for Meteor Mite's screens specifically.
- No raw score shown to Meteor Mite anywhere, in any state — plain-language status only, to avoid gaming and anxiety.
- The "Confirm still available" button never affects ranking — only the Responder Availability Record — stated explicitly in its own copy, to avoid creating a second gaming lever right next to the one this fix is supposed to close.
- The brief's recommended fix is framed as two parts (close the miss/take point gap *and* add drift-back-to-neutral), not drift-back alone — closing the gap was added after review feedback that decay alone "just slows the bleeding."

### Deliverables
- [brief.md](brief.md) — v4 (final), the one-pager for Helen.
- [prototype.html](prototype.html) — the clickable Kip/Meteor Mite mockup, 4 revisions. Published as a private Claude artifact: https://claude.ai/artifact/8UQhrS4ftejYCzs3jZHzSD

### How to reproduce
- Brief's numbers reuse the queries already recorded in [04-x-ray-vision/session-log.md](../04-x-ray-vision/session-log.md) (acceptance rate, missed-ping rate, home-turf share).
- The prototype has no backend — every state is hard-coded click-through content matching brief.md's "Ideal state" table exactly; nothing in it is pulled live from the database.

### Open questions
- See [brief.md](brief.md)'s own "Open questions with owners" table — Wen Li's live-score check and her answer on original intent are still the two items gating anything from actually shipping.
- Whether the stuck browser sign-in popup needs clearing before the student does their own live click-through — **the student's own app UI, not something fixable from here.**

### Prompts and moves that worked
- Asking for multiple independent model reviews, explicitly withheld until "aligned," turned one opinion into a real confidence signal — convergence across 3 separate models mattered more than any single review.
- "Put it in a table on what they agree on and don't, with discrepancy and importance" turned three long paragraphs into one scannable artifact and surfaced the one real disagreement precisely.
- Re-running the same review after each fix round (not just trusting a fix worked) caught a real regression — text labeled "hidden" that was still visible on screen.
- "Do not ask me which one" (picking the responder) forced a grounded read of CLAUDE.md instead of a clarifying question — worth doing whenever the answer is actually inferable from what's already written down.

### Start here next time
1. [brief.md](brief.md) — the final one-pager, ready to send to Helen.
2. The prototype: https://claude.ai/artifact/8UQhrS4ftejYCzs3jZHzSD — click through it yourself before sending.
3. [brief.md](brief.md)'s "Open questions with owners" table — Wen Li is the one person standing between this and an actual ship decision.
