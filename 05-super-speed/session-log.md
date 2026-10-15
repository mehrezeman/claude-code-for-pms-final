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
