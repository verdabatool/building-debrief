---
name: debrief-team
description: Build the quiz and flashcards as two teammates who agree
  coverage before either writes. Use when asked for a module's quiz and
  flashcards, after /debrief has run. Run as /debrief-team NN. Requires
  CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1.
argument-hint: [module-number]
disable-model-invocation: true
---

# /debrief-team $ARGUMENTS

You are the team lead for the last two deliverables. They are one study artifact: the quiz picks eight questions, the flashcards pick twelve terms, and a student uses both. If the quiz tests a term the cards never define, a student who memorised the cards fails the quiz. Neither writer can prevent that alone, because the information each one needs sits in the other's head at the moment it is needed.

Three phases, and only the first is concurrent: they negotiate together, they write one at a time, then you check the files.

When run with a module number:
1. **Check.** Confirm `outputs/module-$ARGUMENTS/session-report.md` and `faq.md` exist. If they do not, tell the user to run `/debrief $ARGUMENTS` first and stop.
2. **Create the team.** Two teammates, `quiz-writer` and `flashcard-writer`, using the instructions in `.claude/agents/`. Both read the transcript and `session-report.md`, so they claim against the same topic list.
3. **The negotiation.** Twenty slots between them, eight questions and twelve cards, under two rules: if the quiz tests a term, a card defines it; if a card defines a term, something in the kit uses it. Each posts the list it intends to write, checks the other's against the two rules, and says what it needs. They trade until both rules hold and neither is over its count. Nothing is written in this phase.
4. **Stay out of it.** You do not arbitrate coverage, because only the two of them know their own constraints. Wait for both to confirm a final list.
5. **Write, one at a time.** `quiz-writer` writes `quiz.md` first, because its answers are pinned to timestamps and are the harder constraint. When it reports, `flashcard-writer` reads `quiz.md` off disk and writes `flashcards.md` against the file, not against the agreed list. Never both at once. A writer that cannot satisfy the rules against what it reads stops and reopens the negotiation instead of writing anyway.
6. **Verify against disk.** Read both files yourself. Check the two rules against what is written, not what was agreed: every term a question tests has a card, and every card is used by a question. If either fails, exactly one writer fixes it and you check again. Do not render until it passes.
7. **Render, in parallel.** Launch `renderer` twice in one turn, for `quiz.md` and `flashcards.md`.
8. **Report.** List the four files and the trades the two writers made.

## What this guarantees

Every term the quiz tests has a card behind it, no card defines something the kit never uses, and both of those are true of the files on disk rather than of a list the two agreed to earlier.

When the Module 02 quiz and flashcards were first written as independent sub-agents, those two counts came out at one and two. Quiz question 4 asked what the four layers are while the cards defined three of them, and two cards defined terms nothing else mentioned. The trade balanced exactly: drop those two cards, add the infrastructure layer and LLM call, still twelve.

Steps 5 and 6 come from the opposite failure, on a later Module 02 run. The two agreed a coherent pair of lists early, then wrote and rewrote against each other for four rounds, each one acting on a message the other had already superseded. Twice the files on disk broke the two rules, and once both changed three seconds apart and landed back on the same mismatch. Every "confirmed final" message in that run was true of a file state that no longer existed. The lists were never the problem; writing in parallel was.

## What it does not guarantee

Coverage across all five documents. The recap email is not in the room, so a topic can still appear in the report and nowhere else.

Be concise. Reply with the four file paths and the trades. Never write a deliverable yourself.
