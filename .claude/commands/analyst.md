---
description: "Answer a business question about Beam's data end to end: frame, profile, analyse, validate, deliver."
argument-hint: "<the business question>"
---

Answer this question about Beam's data end to end: $ARGUMENTS

If the question above is empty, ask for the question and stop. Never invent one.

1. Run the `frame` skill with the question above: read `.claude/skills/frame/SKILL.md` and follow it. Note the run folder it created.
2. For each table named under "Tables likely involved" in the frame note that has no `knowledge/beam/<table>.md`, run the `profile` skill on it (`.claude/skills/profile/SKILL.md`).
3. Run the `analyse` skill on the run folder (`.claude/skills/analyse/SKILL.md`).
4. Run the `validate` skill on the run folder (`.claude/skills/validate/SKILL.md`). If its verdict is **Fix first**, fix `analysis.py` according to the findings, rerun it, and run `validate` again. At most two rounds. If it is still **Fix first**, stop and report the findings; do not write a brief.
5. Run the `deliver` skill on the run folder (`.claude/skills/deliver/SKILL.md`).
6. Finish with the path of the run folder, the verdict from `review.md`, and the Key Takeaways from `brief.md` copied verbatim. Nothing else on the CLI.
