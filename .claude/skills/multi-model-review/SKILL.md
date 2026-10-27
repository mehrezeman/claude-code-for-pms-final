---
name: multi-model-review
description: Runs a document, brief, or prototype past a specific stakeholder persona using several different models in parallel, then reconciles their feedback into one agreement/discrepancy table instead of a single opinion. Use this whenever the student says "run this by [a named person]," "pretend you're X and review this," "get feedback using multiple models," "check this with the team," or wants higher confidence in a review than one pass would give — especially for a brief, prototype, or decision that needs someone specific's buy-in before it ships. Also use it for repeat rounds: "run it by [persona] again," "check if they're aligned now," or "see if this is resolved" after a document has been revised — that's where this skill earns its keep.
---

# Multi-model review

One model reviewing a document tends to approve its own thinking, or close
to it. Running the *same* review prompt through several genuinely
different models — not the same model asked twice — gives an actual
confidence signal instead of one opinion dressed up as consensus. Where
they agree, that's real. Where they split, that's a judgment call for a
human, surfaced instead of averaged away.

## What you need before starting

1. **The persona.** Who is reviewing this, and why they'd care — a name,
   a role, and 2–4 sentences of grounding (what they own, what they've
   said before, what they're skeptical of). If a file already captures
   this — a request doc, a handoff note, an email — read it and quote it
   directly in the prompt rather than paraphrasing. The reviewers should
   see the persona's own words, not your summary of them.
2. **The document or artifact to review.** For a text file, read it and
   pass the full content. For something interactive that a subagent can't
   actually click through (a prototype, an app), write a complete,
   literal description — every screen, every state, every real piece of
   copy, every interaction's actual behavior. A thin description gets a
   thin review back.
3. **Prior-round context, if this isn't the first pass.** Summarize
   exactly what was flagged last time, specifically enough that this
   round's reviewers can say whether each point is resolved — don't make
   them rediscover the same issues cold.

## Step 1: Pick the models

Default to three models spanning different capability tiers, for genuine
diversity — not the same model run three times. Unless told otherwise,
use Opus, Sonnet, and Haiku.

## Step 2: Build one shared prompt

Fill in the template at `references/reviewer-prompt-template.md` with the
persona, their original ask (if any), the document or description,
prior-round context (if any), and the specific questions this round needs
answered. Use the exact same prompt text across every model in this
round — any difference in the results should come from the model, not
from you phrasing it differently each time.

Always ask for: a verdict in character; explicit resolved / partial /
still-open status for anything flagged in a prior round; anything new
that concerns them; and a short numbered list of conditions if the
verdict isn't a clean approval. Cap the response length (300–400 words)
so synthesis stays readable.

## Step 3: Launch all agents in parallel, in one message

Spawn one Agent call per model, all in the same response so they run
concurrently — never one at a time. Name each task clearly (e.g. "Persona
review — Opus, round 2") so results are easy to trace back to a model and
a round later.

## Step 4: As each one lands, give a one-line status update

Don't go quiet while waiting. When a result comes in, send one short
sentence on what it said and how it's trending — but don't treat any
single one as the answer, and don't synthesize until all of them are in.

## Step 5: Once all are in, build the comparison table

Use two tables when there's prior-round context: one for "does this
resolve what was flagged last time," one for "what's new this round."
Columns: `# | Point | Model A | Model B | Model C | Agreement |
Importance`. Keep each model's cell to a short status, not their full
sentence. Call out:

- **What's unanimous (3/3)** — this is the real signal; treat it as the
  priority.
- **What's genuinely disputed** — models actually disagreeing on
  direction, not just emphasis. Flag it explicitly as a human call;
  don't quietly pick a side for them.
- **What only one model caught.** Don't discard these by vote count. A
  single sharp, specific catch — especially one you can verify yourself
  against the source — is often worth more than something all three
  mention in passing. Check it before dismissing it.
- **The overall verdict** each gave (approve / approve with conditions /
  not yet), and whether it's converging across rounds or still drifting.

## Step 6: Offer to close the loop

If there's a clear set of agreed fixes, offer to apply them — don't apply
automatically unless asked — and once applied, offer to run the same
persona past the revised version again. This skill earns its value as a
loop (draft → review → fix → review again), not a one-off.

## Keep in mind

- This is for decisions that genuinely need outside judgment — a
  stakeholder's buy-in, a design review, a go/no-go call. Don't reach for
  it on routine edits.
- The persona's simulated feedback carries no real authority. It's a way
  to pressure-test a document before the real person sees it, never a
  substitute for their actual sign-off.
- If every model gives nearly identical feedback round after round,
  that's a sign the diversity isn't adding much for this particular
  document — a plain re-read might be just as fast, and just as honest
  about it.
