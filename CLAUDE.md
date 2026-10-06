# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Source: `00-rook/company/notes/handoff-from-priya.docx` (outgoing PM's handoff, 21 Aug 2026), cross-checked against Rook's wiki (Team directory, Glossary, Q3 roadmap, Releases, product one-pagers).

### The products
Rook sells two products, both to handlers (and Supply also to quartermasters) — never directly to responders' legal identities, which Rook doesn't hold.
- **Rook Dispatch** (I own this): coordinates responders to incidents. Flow: incident enters the console → Dispatch ranks available responders (routing priority) → top-ranked responder gets a **ping** on their phone → taken, turned down, or missed → moves to next responder until taken. Headline metric: **acceptance rate** (share of pings taken). Also watched: **time-to-accept**, **coverage gap**. Web console for handlers, native phone app for responders. Ships monthly, 4.x release train. Current release: **4.2** (shipped 12 Aug 2026).
- **Rook Supply** (adjacent, not mine): equipment requisitions, approvals, maintenance, field failure reports. Touches Dispatch one way only — Supply *reads* the Responder Availability Record (which Dispatch writes) to schedule maintenance around callout load; it never writes back. Changes I make to availability logic can ripple into Supply's scheduling without any action on their side.

### The people
From the Team directory (updated 2 Sept 2026):
| Name | Role | Notes |
|---|---|---|
| **Helen Achebe** | Director of Product | My manager. Owns roadmap/commitments. "Good. Will give you room." Open conversation pending: which squeezed-out Q3 items are still committed. |
| **Marcus Oyelaran** | Engineering Manager, Dispatch | Runs the eng team. Straight talker, will tell you when something's a bad idea. Default first call when unsure. Can usually pull numbers on request. |
| **Wen Li** | Staff Engineer, Dispatch | Built the routing/ranking logic ("who gets pinged"). No written spec exists — understanding it means talking to her. (Was away 14–24 Aug 2026.) |
| **Nadia Hoffmann** | Support Lead | Hears handler complaints first. Worth a standing 15-min sync. |
| **Sofia Marino** | Product Designer | Owns console + phone app design. Ran the September customer interviews. |
| **Ravi Menon** | Data Analyst | Reports weekly on ping/acceptance numbers. |
| **Priya Raghunathan** | PM, Dispatch (predecessor) | Left 21 Aug 2026. Sole PM on Dispatch for 14 months. |

Company-wide: ~241 employees, HQ at Site Aleph plus Berlin/Singapore/Cornwall offices; mostly remote. Founded 2014.

### Vocabulary (Dispatch-specific)
- **Responder** — independent field worker who takes callouts; not a Rook employee; known to us only via capability tags + availability, never a legal identity. **Cover identity** is their public persona — do not design around mapping it to a real identity (contractual, not stylistic).
- **Handler** — manages a responder (or small group): availability, gear, readiness. Usually the one actually using the console.
- **Quartermaster** — Supply-side role; approves/owns equipment stock.
- **Callout** — a request for a responder to attend an incident; the unit of work.
- **Ping** — one callout offered to one responder's phone. Outcomes: **taken**, **turned down**, or **missed** (ping wait expired — tracked separately from turned-down, same downstream effect).
- **Ping wait** — how long a ping sits before counting as missed; set per-release, same for everyone (cut 90s → 60s in 4.2).
- **Routing priority** — the ranking score: proximity (travel-time estimate), availability, capability match, recent acceptance history. Turning down/missing a ping lowers your recent-acceptance component and your place in future rankings.
- **Capability tag** — competency label on a responder (flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation), matched against incident requirements.
- **Coverage gap** — no available responder had the required tag (distinct from a low acceptance rate — nobody *could* go, vs. nobody *would*).
- **Responder Availability Record** — shared record Dispatch writes and Supply reads.
- **Mutual aid** — responders covering for each other across areas; not built yet, Q4 exploration item.

### Where things stand
- **4.2 (12 Aug 2026)** shipped the long-requested change weighting proximity up relative to recent acceptance history (responders in wide/remote areas were being skipped for others with better acceptance records 40 min away) — plus the ping-wait cut (90s→60s) and cosmetic console-filter persistence.
- **Current fire:** acceptance rate is down and handler complaints are up since 4.2. Priya's read: likely mostly seasonal (August is soft every year) compounded by the ping-wait cut landing in the same release — not clear evidence the ranking change itself is the problem. Her explicit advice: look at seasonality first, and don't let this slide into a "revert 4.2" conversation — the change was a genuine, long-standing ask and reverting just trades one unhappy group for another.
- **Console filter persistence** (4.2) will generate tickets; it's cosmetic noise, not worth early-month attention.
- **Open/unfinished from Q3:**
  - *Availability Confidence* — committed to 4.2 on the roadmap but did not actually ship in it; status needs re-confirming with Helen.
  - *Requisition approval chains* — committed for 4.3.
  - *Handler phone app* and *Shared cover between responders* — both Q4, still "Exploring."
  - No written doc exists for how routing/ranking actually works (lives in Wen Li's head). Priya flagged this as something that needs writing.
- **First-month advantage, per Priya:** arriving with no attachment to past decisions is useful — "use it before it wears off."

### Session findings (investigation into the 4.2 dip)
- No prior-year data exists in the database, so Priya's "it's seasonal" claim can't be checked — and the acceptance-rate drop is a sharp cliff exactly at the 12 Aug release, not a gradual seasonal slide, which argues against it anyway.
- Missed pings (not turned-down) drove most of the drop — points at the ping-wait cut (90s→60s) more than the proximity/ranking change.
- Four responders (Farlight, The Undertow, Corporal Ashgrove, Halfmoon) have nearly stopped getting pings since mid-August and are still getting worse, not recovering, as of 7 Sep — hypothesis: a routing-priority feedback loop (miss → lower acceptance score → ranked lower → fewer pings → more misses).
- Support tickets since 4.2 split into three real themes: missed-ping complaints, the four-responder starvation pattern, and filter-persistence bugs (cosmetic noise, as Priya predicted).
- Not yet checked: whether those four responders share a handler/area/capability tag, whether the pattern reaches beyond them, and the actual routing code in `00-rook/code/dispatch-routing/` — the mechanism is still a hypothesis, not confirmed.
