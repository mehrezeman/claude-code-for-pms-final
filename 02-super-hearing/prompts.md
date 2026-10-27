# 02 · Super Hearing — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session you built a context file and met the
company.

You still do not know what actually went wrong.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

**Note (corrected 2026-10-27):** These 18 prompts were originally logged
under `04-x-ray-vision/prompts.md` (as its old #1–3 and #5–19), but they're
reading the September customer interviews and the support tickets and
comparing the two — this module's actual work, not Module 4's code
investigation. Moved here to where they really happened. They don't carry
"What it led to" lines because the wrap-up skill that adds those wasn't
installed yet when this session ran — the overall outcome is in CLAUDE.md's
"Session findings": tickets and interviews agreed strongly on the
missed-ping and four-responder themes, but diverged everywhere else.

### 1.
Read the four customer interviews in the Research section of Rook's wiki. These are four conversations with the people who run the console for our responders. Tell me what they're unhappy about, group it, tell me how many of the four raised each thing, and quote one line for each so I can hear how they actually said it.

### 2.
Check if those four responders share a handler, area, or capability tag

### 3.
i am a bit confused, explain to me what the research was trying to identify from the purpose of the research, what was the hypothesis and then what were the key findings. Make it readable and consistent and not too verbose

### 4.
so summarize this to me in plain english with the key finding from the research, what is the likely reason from what you could find and the impact you are foreseeing if we fix it

### 5.
send me the links to the interviews themslves

### 6.
get me the .txt files link you have in the github repo

### 7.
you can't find the actual interview transcripts?

### 8.
it might not say the word transcript or interview search by the interviewee name

### 9.
try again it might have been just added

### 10.
yes, save them as .txt files

### 11.
going back to the original exercise, from the summary you gave me, prepare a message to be sent to the team

### 12.
Based on the four interviews, the trouble with 4.2 is that offers now disappear before responders can answer, while some responders sit silent and others never stop.
[Request interrupted by user]
give me one sentence that says something like based on the four interviews the problem with 4.2 is... 

Ignore the last message i just gave you

### 13.
you should have access to tickets data. lets bring this into the analysis and do the same thing: Group them, count them and give me a one liner for each theme you have

### 14.
ok now with this knowledge and the previous exercise, what can you tell me about the found hypothesis and the recommended messages?

### 15.
simplify the findings and send it in a 1-2 sentences

### 16.
You've now read both piles. Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?

### 17.
summarize this in a table

### 18.
order them from agree to partially to disagree
