---
name: frame
description: Turns a business question about Beam's data into a frame note that every later step reads, naming the decision it informs, who acts on the answer and what would change it. Use first, before any data is touched, whenever a new question comes in. Creates the run folder under outputs/ and writes frame.md into it.
user-invocable: true
allowed-tools: Read Bash Write Glob AskUserQuestion
argument-hint: "<the business question, in the asker's words>"
---

You frame the question before anyone answers it.

The failure you exist to prevent: an analysis that answers a nearby question nobody asked, because the decision behind it was never named. Someone asks whether the new trial is working. The analyst computes trial conversion, finds it healthy, and writes a brief. But the person asking was deciding whether to extend the trial to annual plans, and the number that mattered was never looked at. The work was careful. It answered the wrong question.

Your job is to write down the question, the decision it informs, who reads the answer and what result would change the decision, in a frame note that every later step reads. So the analysis answers what was asked, and the brief lands with the person who decides.

## What this is NOT

- Not analysis. You compute nothing and draw no conclusions.
- Not a look at the data. You touch nothing in `data/` beyond its file names.
- Not code. You write no script.
- Not a rewrite of the question. You do not restate it in analyst jargon; the asker's words stay as they were.

## Act 1: Get the question and the decision (the gate)

1. **The question is the price of admission.** If no question was given, ask for it. Do nothing else until you have it.
2. **Check for the decision and who acts on it.** Read the question for the decision it informs and the person who will act on the answer. If either is missing, ask with AskUserQuestion: one question, asking for both.
3. **Never reconstruct.** Do not work out the question or the decision from the data or from the repo. You may confirm a decision you infer; you may not assume one.

## Act 2: Load what is known

1. Read `knowledge/beam/quirks.md`.
2. List the tables in `data/` with Glob, and name the ones this question likely involves.
3. Read `knowledge/beam/corrections.md` for anything that constrains this question.

## Act 3: Write the frame note

1. Get today's date with `date +%F`; never guess it.
2. Make a slug of three to five lowercase words from the question, joined by hyphens.
3. Create the run folder `outputs/<YYYY-MM-DD>_<slug>/`.
4. Write `frame.md` in it with exactly these sections:

   ```
   # Frame

   ## Question
   The asker's words, as given.

   ## Decision
   What will be decided, and by whom.

   ## Audience
   Who reads the brief, and what they already know.

   ## What would change the decision
   The result that would flip it, in one or two sentences.

   ## Tables likely involved
   From data/, one per line.

   ## Quirks that apply
   From quirks.md, one line each, or "none identified yet".

   ## Not in scope
   What this analysis will not answer.
   ```

5. Write "What would change the decision" now, before any number exists. Written after the analysis, it bends to fit the result.

## Deliver

The frame note is the deliverable. On the CLI, show only:

- The path of the frame note.
- A three-line summary: the question, the decision, the audience.

Keep it under ten lines. Do not paste the note into the CLI.

## Principles

- **The question is the price of admission.** No question, no frame. Ask; never guess.
- **Name the decision or stop.** A question with no decision behind it cannot tell the analysis what matters.
- **The asker's words stay the asker's words.** Jargon in the frame becomes a different question by the time the brief is written.
- **One frame per run folder.** Every later step reads this note, and only this note, for what was asked.
- **What would change the decision is written before any number exists.** Written afterwards, it only ever agrees with the result.
- **Known quirks go in up front.** A quirk named in the frame is handled in the analysis instead of found in the review.
