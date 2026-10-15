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

### 1.
Read the four customer interviews in the Research section of Rook's wiki. These are four conversations with the people who run the console for our responders. Tell me what they're unhappy about, group it, tell me how many of the four raised each thing, and quote one line for each so I can hear how they actually said it.

### 2.
Check if those four responders share a handler, area, or capability tag

### 3.
i am a bit confused, explain to me what the research was trying to identify from the purpose of the research, what was the hypothesis and then what were the key findings. Make it readable and consistent and not too verbose

### 4.
Check the actual routing code in dispatch-routing

### 5.
so summarize this to me in plain english with the key finding from the research, what is the likely reason from what you could find and the impact you are foreseeing if we fix it

### 6.
send me the links to the interviews themslves

### 7.
get me the .txt files link you have in the github repo

### 8.
you can't find the actual interview transcripts?

### 9.
it might not say the word transcript or interview search by the interviewee name

### 10.
try again it might have been just added

### 11.
yes, save them as .txt files

### 12.
going back to the original exercise, from the summary you gave me, prepare a message to be sent to the team

### 13.
Based on the four interviews, the trouble with 4.2 is that offers now disappear before responders can answer, while some responders sit silent and others never stop.
[Request interrupted by user]
give me one sentence that says something like based on the four interviews the problem with 4.2 is... 

Ignore the last message i just gave you

### 14.
you should have access to tickets data. lets bring this into the analysis and do the same thing: Group them, count them and give me a one liner for each theme you have

### 15.
ok now with this knowledge and the previous exercise, what can you tell me about the found hypothesis and the recommended messages?

### 16.
simplify the findings and send it in a 1-2 sentences

### 17.
You've now read both piles. Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?

### 18.
summarize this in a table

### 19.
order them from agree to partially to disagree

### 20.
Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.
What it led to: An 8-step (later 12-step) plain-English walkthrough of the ping pipeline, mapped to availability.py, routing.py, offer.py and history.py.

### 21.
can you tell me why some don't recieve a ping at all
What it led to: Found two causes: never marked available, or ranked too low to be reached before the callout resolves.

### 22.
can you tell me if the four cases here were futher down the list and that is why they never get pinged

### 23.
ok so write me a one sentence conclusion of what you found here
What it led to: Queried ping order and home-area share; found the four rank 1st-2nd when pinged but are excluded from most callouts entirely (home-turf share fell as low as 16-19%), then a one-sentence root-cause summary.

### 24.
can you compare with what was implement with the spec that was provided by fro 4.2
What it led to: Compared the 4.2 wiki spec to config.py; found the engineering manager's own unanswered scope question from two days after ship.

### 25.
this is very long make it shorter and to the point
What it led to: Condensed the spec-vs-built comparison into four short lines.

### 26.
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

### 27.
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

### 28.
Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.
What it led to: Pinpointed record_declined (history.py) as the only penalty, and record_accepted as the only credit anywhere in the code.

### 29.
Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.
What it led to: A 7-step mechanical walkthrough showing recovery needs repeated lucky wins under the shortened timeout, with no shortcut anywhere in the code.

### 30.
but if htey are 0 and the other ones are higher, would that mean that we will keep getting lower ones since they are never going ot be up the list
What it led to: Confirmed a winner-take-all feedback loop across the whole pool, corroborated by The Gale's rising ping rate while Meteor Mite (same handler) collapsed.

### 31.
so how do we fix this, should they create a new account, would that reset the score
What it led to: Found a new account would reset the score (it's keyed only on name), but flagged it as exploiting the bug, not fixing it; pointed back to the drift-to-neutral fix instead.
