# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

**Note (corrected 2026-10-27):** The prompts originally logged here as
#1–3 and #5–19 were about reading the September customer interviews and
the support tickets, and comparing the two — that was Module 2's work,
not code investigation. They've been moved to
`02-super-hearing/prompts.md`, where they actually happened. What
follows is just the real code investigation, renumbered.

### 1.
Check the actual routing code in dispatch-routing

### 2.
Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.
What it led to: An 8-step (later 12-step) plain-English walkthrough of the ping pipeline, mapped to availability.py, routing.py, offer.py and history.py.

### 3.
can you tell me why some don't recieve a ping at all
What it led to: Found two causes: never marked available, or ranked too low to be reached before the callout resolves.

### 4.
can you tell me if the four cases here were futher down the list and that is why they never get pinged

### 5.
ok so write me a one sentence conclusion of what you found here
What it led to: Queried ping order and home-area share; found the four rank 1st-2nd when pinged but are excluded from most callouts entirely (home-turf share fell as low as 16-19%), then a one-sentence root-cause summary.

### 6.
can you compare with what was implement with the spec that was provided by fro 4.2
What it led to: Compared the 4.2 wiki spec to config.py; found the engineering manager's own unanswered scope question from two days after ship.

### 7.
this is very long make it shorter and to the point
What it led to: Condensed the spec-vs-built comparison into four short lines.

### 8.
Build me a shareable, interactive artifact: a "case file" explaining what release
[4.2] did to responders in Rook Dispatch, built from the analysis we have already
done in this session. Use only data from the rook-wiki, the rook-database, and the
files in this folder. Do not invent numbers.

THEME: bold comic-book super-hero style. Page background [Laguna Seca blue], white
content panels with thick navy outlines and hard shadows, halftone dots, a
comic display font for headings, yellow and orange as the accent colours. Keep all
body text high-contrast on white panels. Include a light version and a dark
version, and make it work at phone width with no sideways scrolling.

SECTIONS, in this order:
1. Four headline numbers in comic "burst" tiles (before to after).
2. "The mission": the code walkthrough as 12 clickable steps from "something goes
   wrong" to "the phone buzzes". Each step has a plain-English line and the file
   it lives in. Add a switch, "Show where the heroes got stuck", that marks the
   steps forming the penalty loop.
3. "Asked versus built": a scoreboard of each change that was requested, with
   BUILT / NOT BUILT and WORKED / DID NOT stamps and one line of evidence.
4. "The villains": the bugs and design flaws as cards with severity, the file
   it lives in, and evidence. Add filter buttons by file.
5. "The hero health lab": an interactive replay. Embed the real ping-by-ping
   history for 6 to 8 heroes (the ones who went quiet, one busy hero, one
   unaffected). Sliders for what a missed offer costs, what a refusal costs,
   misses in a row before any cost, daily drift toward neutral, and the lowest
   allowed score, plus a checkbox for "no cost for offers outside their home
   area". Draw today's rules against my rules as two lines, with a marker where
   the release shipped and a strip showing each offer's outcome. Read the real
   scoring rules from the code folder.
6. "Orders for the developer": a prioritised checklist, each with the file and a
   "done when" line, a progress bar, and a "copy as text" button.
7. "Loose ends": what could not be verified and who to ask.

RULES:
- Plain English, no code words. A non-technical reader should follow it.
- Label what is observed versus estimated. Say clearly that the replay shows
  what happened to the score, not a forecast.
- Never try to identify the real people behind cover identities.
- Test it before publishing: open it locally, check there are no console errors,
  and check that every slider and button works.
- Save the source file inside this folder, then publish it as a private artifact
  and give me the link.
What it led to: Published "Case File 4.2," an interactive comic-styled artifact covering the mechanism, spec-vs-built, defects, a live scoring simulator and a developer checklist (00-rook/case-file-4.2.html).

### 9.
Install the "wrap-up v2" skill from a classmate's repo into my course folder.

SOURCE (a public repo, pinned to the exact commit that was reviewed):
  https://github.com/fnajam325/claude-code-for-pms-final   commit 700ac37
FILES TO INSTALL, all inside that repo's .claude/ folder:
  skills/wrap-up/        the v2 wrap-up skill
  skills/course-setup/   the save routine that wrap-up depends on
  settings.json          pre-approves the rook-wiki and rook-database servers

HOW:
- Download the .claude folder at that commit into a temporary folder first. Do not
  clone into my course folder. Put GIT_TERMINAL_PROMPT=0 in front of every git
  command that talks to GitHub, and never ask me for a password or token.
- Before installing anything, read every file you downloaded. Tell me in plain
  English what the scripts do and flag anything that looks risky. Wait for my
  "yes" before copying.

RULES:
- Never delete or overwrite anything. If a file or folder is already there,
  rename mine with "-old" on the end and tell me. If I already have a
  .claude/settings.json, show me both and merge by adding, not replacing.
- Do not change CLAUDE.md, and never touch anything above its "---" line.
- Keep the .sh scripts executable.
- When finished, list exactly what was installed, tell me to start a new session
  (skills load at the start of a session), and suggest I say "checkpoint" to try it.
- Do not commit or push anything until I say so.
What it led to: Installed wrap-up v2 from a classmate's repo at a pinned commit, after reading every file; kept the old skill as wrap-up-old; left identical course-setup/settings.json untouched.

### 10.
Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.
What it led to: Pinpointed record_declined (history.py) as the only penalty, and record_accepted as the only credit anywhere in the code.

### 11.
Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.
What it led to: A 7-step mechanical walkthrough showing recovery needs repeated lucky wins under the shortened timeout, with no shortcut anywhere in the code.

### 12.
but if htey are 0 and the other ones are higher, would that mean that we will keep getting lower ones since they are never going ot be up the list
What it led to: Confirmed a winner-take-all feedback loop across the whole pool, corroborated by The Gale's rising ping rate while Meteor Mite (same handler) collapsed.

### 13.
so how do we fix this, should they create a new account, would that reset the score
What it led to: Found a new account would reset the score (it's keyed only on name), but flagged it as exploiting the bug, not fixing it; pointed back to the drift-to-neutral fix instead.
