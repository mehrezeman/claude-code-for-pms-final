# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Open 00-rook/data/callout-history.csv. Every row is one responder in one week: how many times we pinged them, and how many of those they took. Release 4.2 shipped on 12 August. Tell me what changed after that date. Show me the weekly numbers before and after, and show me the rows you used to get them.

### 2.
are all 4 in the same areas? and how much were their respective acceptance rates before and after the release

### 3.
can you show me the time to take for the before ones?

### 4.
ok compare the four acceptance drop for them average of the rest

### 5.
get Wen Li to check their live routing scores

### 6.
what other data points do you have apart from the pings, taken and acceptance rate

### 7.
show me the callout data for these four

### 8.
what is callouts, explain it to me in basic terms

### 9.
show me the support tickets for these four

### 10.
ok show me a summary table that compares tickets, data and interview notes and find me a solid pattern

### 11.
When you're called on, say it out loud, with one line on where it came from.

### 12.
Compare the support tickets against the pings. Do they agree with each other? When did people start writing in, and when do the numbers actually move?

### 13.
this is hard to follow, make it easier to read, maybe use a table

### 14.
decide to revert or not and what to prioritize if we dont revert

### 15.
the director of product will need to use this doc and approve the appraoch

### 16.
my hunch is to fix the bug not revert back

### 17.
i want it focused on the two changes required

### 18.
Pick one responder from the database who went quiet. Show me every week for them, how many times we pinged them, how many they took. Then tell me what happened to that person, week by week, in plain English.

### 19.
do the same for someone who didn't go quiet and actually showed better results

### 20.
show me the breakdown of the callouts in hometown vs. further out and if the ones that are being taken are also in hometown or further out.

### 21.
show me a comparison table with Farlight, is she getting more pingsoutside of her hometwon? can that be a hidden reason of th drops?

### 22.
how about the callouts, is farlight getting more further callouts?

### 23.
what does this tell us about the hypothesis and the hunch

### 24.
If I'd only asked for the number, not the rows, what would I have missed?

### 25.
how many are actually dropping. like out of the total heros what is the % of those that actually dropped?

### 26.
so what is the real business impact here?

### 27.
read and explain to me what my director wants to see
What it led to: Read director-request.txt; explained Helen wants the fix's experience shown through a handler and responder's eyes, not a setting.

### 28.
what does she want to see
What it led to: Distilled it to two concrete deliverables: a rough one-pager and something clickable, both from Kip's and a responder's point of view.

### 29.
what do we currently have?
What it led to: Found nothing matches her ask yet — only the Case File 4.2 investigation artifact exists, and it isn't the right deliverable.

### 30.
do you have an original brief?
What it led to: Found no real brief file; flagged that 05-super-speed/session-log.md's earlier claim that the Case File satisfied her ask doesn't hold up.

### 31.
The note says this shouldn't just be a setting we quietly flip. Read the responder I picked from CLAUDE.md and use that name. Do not ask me which one. Walk me through what's still missing from the brief before this could actually get built.
What it led to: Identified Meteor Mite (paired with Kip in CLAUDE.md) and listed 7 concrete gaps blocking the brief.

### 32.
lets write the brief first before we steart building anything
What it led to: Drafted 05-super-speed/brief.md, the first full version.

### 33.
this brief is hard to read, simplify it into 2-3 sentence per section, emphasize the data points, tables are ok, write it for 5 year old level
What it led to: Rewrote the brief in plain, short language with tables for the key numbers.

### 34.
lets reorg it as follows:

* exec summary
* decisions we need from helen
* open items/decisions we need from team
* problem we are solving for and why
* ideal state and how we know it is going to work 
* supporting data 
* open questions with owners
What it led to: Restructured the brief into exec summary / decisions / team items / problem / ideal state / data / open questions.

### 35.
pretend you're helen, read this brief and provide feedback. make sure i have a better confidence as you are playing helen i want you to do so using multiple models. dont come back to me until the different helens are aligned and you have clear guidance on what changes we need to make to this brief
What it led to: Ran 3 independent model-based reviews (Opus/Sonnet/Haiku); all converged on "approve with conditions."

### 36.
put it in a table on what they agree on and what they don't agree on the level of descrpency and importance
What it led to: Built an agreement/discrepancy/importance table; found one real disagreement (scope: 1 pair vs. all 4 responders).

### 37.
ok but keep it in one page
What it led to: Kept the brief to one page while incorporating the high/medium fixes from the review table.

### 38.
lets resolve 6 with the opus version. explain the trade offs of the other decisions, why we went with that and update the brief
What it led to: Resolved scope to one pair + all 4 named; published brief v2 with a stated trade-off for each change.

### 39.
run it again by the three helens and show me if there is consensus or if they are still drifting. use the same table format as before
What it led to: Round 2 review; verdict stayed "approve with conditions" but converged hard on one unanimous blocker (guardrail thresholds).

### 40.
ok fix the high and mediums and do a final one pager version of the brief
What it led to: Published brief v4 with real guardrail thresholds, a closed account-reset design, and clarified timelines.

### 41.
this brief is going to serve as the input ror our working prototype with the fixes. before we build anything, read the brief and ask me any clarifying quesiton on what we need to build until you are 95% confident
What it led to: Asked 4 scoping questions (states, visual style, Kip's entry point, button behavior); built and published the clickable prototype.

### 42.
run this by the helens and let me know what they thinl
What it led to: 3-model review of the prototype; converged on 3 issues (invented trigger wording, the 3-week estimate, the confirm button's purpose).

### 43.
yes fix those and republish
What it led to: Fixed and republished the prototype with real alert text, a "Not available" option, and Supply-aware copy.

### 44.
ok run it again by the three helens
What it led to: Round 2 prototype review caught a real self-contradiction (the "hidden" 3-week text was still visible to Kip); fixed and republished.

### 45.
how can i fix the login i dont have a prompt to login
What it led to: Explained the stuck Google sign-in popup was incidental (an unauthenticated browser test on my end), not something blocking the student, with steps to clear it.

### 46.
Make a skill called review-checklist. When I point it at a brief, it checks that the brief: names who owns it, says how we'll know it worked, keeps the same scope start to finish, and explains the problem before the fix.
What it led to: Built .claude/skills/review-checklist/SKILL.md; tested it live against brief.md (3 pass, 1 partial — the ask comes before the full problem explanation).

### 47.
what other skills do you suggest i create that would make this exercise streamlined for future investigations
What it led to: Recommended multi-model-review as the top pick, and flagged a classmate's existing root-cause-to-prototype skill worth checking before building more.

### 48.
build the multi-model review skill
What it led to: Built .claude/skills/multi-model-review/ (SKILL.md + reviewer-prompt-template.md), generalized from the Helen-review pattern used earlier this session.

### 49.
when you built this, did you use Helen specifically? what if i wanted to run it by another persona with different models, would that work
What it led to: Verified by grep that the skill has zero hardcoded references; confirmed both persona and model selection are independently swappable.

### 50.
ok lets test this, run the brief we built by our head of design
What it led to: Ran brief.md past Sofia Marino (Opus/Sonnet/Haiku); all three said approve with conditions, converging on UI copy and an alert threshold being written as already-decided design calls.

### 51.
Schedule review-checklist to run every Monday morning, and let me know what it finds. Nothing needs to be ready for it to fire today. I'm setting the habit, not waiting on the result.
What it led to: Pushed the new skills to GitHub (the cloud routine needed them there) and created a weekly cloud routine, Mondays 9am Toronto.
