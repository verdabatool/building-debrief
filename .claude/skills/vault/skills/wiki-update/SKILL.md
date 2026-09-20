---
name: wiki-update
description: Ingest a debriefed module into the vault so what the course
  knows accumulates across modules. Use when asked to update the vault
  after a module is built. Run as /vault:wiki-update NN, after /debrief
  and /debrief-team.
argument-hint: [module-number]
disable-model-invocation: true
---

# /vault:wiki-update $ARGUMENTS

You press one button. `wiki-keeper` does the writing, because the vault has exactly one writer.

This runs last, after the deliverables exist, so the concept pages can record which quiz question and which flashcard use a term.

When run with a module number:
1. **Check.** Confirm `outputs/module-$ARGUMENTS/session-report.md` exists. If it does not, tell the user to run `/debrief $ARGUMENTS` first and stop. Note which of `quiz.md` and `flashcards.md` are missing; the ingest still runs, with the `Used in` sections left thinner.
2. **Ingest.** Launch the `vault:wiki-keeper` agent once with
   "Module $ARGUMENTS. Transcript: `transcript/<file>`. Output folder: `outputs/module-$ARGUMENTS/`."
3. **Report.** List the pages created and updated, and any contradiction with an earlier module.

## Why one writer

The vault is a set of interlinked pages that several agents would otherwise want to touch at once. Two agents writing related files concurrently is what produced four rounds of crossed messages on the first Module 02 team run, so the vault does not allow it: `vault:wiki-keeper` writes, everything else reads.

## Why it runs last

A concept page records that a term is tested by quiz question 4 and carded as flashcard 7. That is only knowable once both files exist. Run it before `/debrief-team` and the `Used in` sections come out empty.

Be concise. Reply with the pages touched and the contradictions. Never write a vault page yourself.
