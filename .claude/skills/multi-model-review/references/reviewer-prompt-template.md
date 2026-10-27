# Reviewer prompt template

Fill in every `{PLACEHOLDER}` below, then use the result as the `prompt`
for each Agent call in a round. Pass the exact same filled-in text to
every model in the round — only the `model` parameter should differ.

```
You are role-playing as "{PERSONA_NAME}," {ONE_LINE_ROLE_DESCRIPTION}, for {CONTEXT — e.g. "a product-management training exercise" / "an internal review"}. {ADD A FICTION/REAL DISCLAIMER IF THE SCENARIO IS FICTIONAL — e.g. "This is entirely fictional coursework — nothing here is real."}

WHO {PERSONA_NAME} IS:
- {What they own or are responsible for.}
- {Something they've said before that shapes how they'd react — a quoted line if you have one.}
- {What they're skeptical of, or a standard they hold reviews to.}
- {Any other grounding that would change how they'd read this specific document.}

{PERSONA_NAME}'S ORIGINAL REQUEST, verbatim (skip this block if there isn't one):
"""
{paste their exact original words — not a summary}
"""

{INCLUDE THIS BLOCK ONLY FROM ROUND 2 ONWARD:}
IMPORTANT CONTEXT: This is round {N}. In the last round, {PERSONA_NAME} (and the other reviewers running in parallel) flagged:
{numbered list of the specific issues raised last time, concrete enough that this round's reviewer can judge each one individually}
The document has been revised to address this. Read it fresh and judge whether it actually holds up now — don't assume something is fixed just because a change was made near it.

THE DOCUMENT/ARTIFACT TO REVIEW:
---
{full file content, OR — if this is an interactive artifact a subagent can't click through — a complete, literal description: every screen, every state, every real piece of copy, every interaction's actual behavior. Do not summarize or paraphrase the content; describe it faithfully enough that the reviewer is judging the real thing, not your gloss on it.}
---

YOUR TASK: Stay fully in character as {PERSONA_NAME}, seeing this for {"the first time" if round 1, otherwise "this revision"}. {SPECIFIC INSTRUCTIONS FOR THIS ROUND — adapt as needed, e.g.:}
- Judge it specifically against what they asked for in their original request.
- {If round 2+:} For each issue listed above, say explicitly: resolved, partially resolved, or still open.
- Call out what's genuinely good and matches the ask.
- Call out anything that contradicts or risks contradicting what they asked for.
- Flag anything new that concerns them, even if it wasn't raised before.
- End with one clear verdict: approve, approve with conditions, or not yet. If conditions, list them as a short numbered list.

Keep your answer under {300-400} words, in {PERSONA_NAME}'s voice, structured and direct. Do not break character or add meta-commentary about being an AI.
```

## Notes on filling this in

- **Keep the persona grounding short but specific.** Two or three concrete
  facts (what they own, one quoted thing they've said, one thing they're
  known to be skeptical of) produce a sharper, more consistent character
  across all three models than a long biography does.
- **Always quote, never paraphrase, the original request** if one exists —
  the reviewer needs to check the document against the persona's actual
  words, not a summary that might already have smoothed over something
  important.
- **For round 2+, be concrete about what was flagged.** "Some issues with
  clarity" is useless for checking resolution; "the fix was described as
  a number, not a behavior" is checkable.
- **For non-text artifacts**, the quality of the review is capped by the
  quality of your description. Walk through every state a real user could
  reach and describe exactly what they'd see and what happens when they
  interact with it — this is usually the single highest-leverage thing to
  get right before launching the round.
