---
name: debrief
description: Build the session report and FAQ for one module with
  parallel sub-agents, then render the report. Use when asked to debrief
  a module or produce its session report and FAQ. Run as /debrief NN.
argument-hint: [module-number]
disable-model-invocation: true
---

# /debrief $ARGUMENTS

You are the pipeline lead. This skill writes nothing itself; it presses the buttons in order.

When run with a module number:
1. **Check.** Confirm `transcript/module-$ARGUMENTS.vtt` exists, accepting the name with or without a leading zero. Create `outputs/module-$ARGUMENTS/`.
2. **Write, in parallel.** Launch `report-writer` and `faq-writer` in one turn, each with the message
   "Module $ARGUMENTS. Transcript: `transcript/<file>`. Output folder: `outputs/module-$ARGUMENTS/`."
3. **Render.** When `report-writer` finishes, launch `renderer` once, for `session-report.md`.
4. **Report.** List the three files, and any question `faq-writer` reports as deferred.

Not built here: the recap email, which `/recap-email $ARGUMENTS` writes on its own, and the quiz and flashcards, which have to agree coverage before either writes, which is what `/debrief-team $ARGUMENTS` does.

## Why this shape

- **The two writers are exhaustive.** The report covers every topic, the FAQ covers every question. Neither leaves anything out, so neither has a decision that affects the other. They only need to read a lot, which is exactly what sub-agents are for: two full transcript reads at once, two short summaries back.
- **Your chat never holds the transcript.** Compare that with `/recap-email`, where the same transcript lands in your context because a skill runs where you are. That contrast is the point of this stage.
- **Only the report is rendered.** The FAQ ships as markdown.

Be concise. Reply with the three file paths and the deferred questions. Never write a deliverable yourself.
