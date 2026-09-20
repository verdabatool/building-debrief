# Debrief

A small Claude Code project that turns a class transcript into the five documents a facilitator ships after each session: recap email, session report, FAQ, quiz, and flashcards.

It is also a teaching repo. Copy it, read every file, and you will have seen all three ways Claude Code shares work: **skills**, **sub-agents**, and **agent teams**. Each one solves a different problem, and the problem gets bigger at every step.

**New here? Start with [the guide in `doc/`](doc/README.md)**, which explains all three with diagrams and the worked example from this repo.

## Run it

```
/recap-email 02    # recap email
/debrief 02        # session report, FAQ
/debrief-team 02   # quiz, flashcards
```

Eight files land in `outputs/module-02/`. Add a module by dropping `module-N.vtt` into `transcript/`.

## The three ideas

| Idea | Problem shape | Covers here |
|---|---|---|
| **Skill** | Same work, same way, every time | Recap email format, HTML rendering, the two runbooks |
| **Sub-agent** | Independent jobs that each need a lot of context | Session report, FAQ, HTML renders |
| **Agent team** | One agent's choice constrains another's | Quiz and flashcards |

### Skills

The recap email needs the same subject line and the same closing lines every week. The cost of doing it by hand is inconsistency, not effort, so the format belongs in a file. Change the subject line in `.claude/skills/recap-email/SKILL.md` and every future email changes, with nothing else touched.

`beautiful-html` is the same idea for turning markdown into a styled page. `debrief` and `debrief-team` are buttons rather than formats: they press the others in order and write nothing themselves.

### Sub-agents

The session report and the FAQ are exhaustive. The report covers every topic, the FAQ covers every question. Neither leaves anything out, so neither has a decision that affects the other. They only need to read a lot.

Run them in your chat and you pay twice, in context and in waiting. Run them as sub-agents and both read the whole transcript at the same time in their own windows, and your chat gets two short summaries. You also get tool restriction, since writers hold Read and Write and nothing else, and a model chosen per job.

### Agent team

The quiz and flashcards are one study artifact. The quiz picks eight questions, the flashcards pick twelve terms, and a student uses both. If the quiz tests a term the cards never define, a student who memorised the cards fails the quiz.

That is not hypothetical. When the Module 02 quiz and flashcards were first written as independent sub-agents, quiz question 4 asked what the four layers are and the flashcards defined three of them. The kit in `outputs/` is the version written after the two were made to talk.

Neither writer can prevent it alone, because neither knows what the other picked. So they trade: twenty slots between them, under two rules. If the quiz tests a term, a card defines it. If a card defines a term, something uses it.

## Demo path

1. **Skill.** Run `/recap-email 02`. The email comes out in the right format because the format lives in a file. *Then look at the context meter: the transcript is now in your chat, and four more documents will not fit.*
2. **Sub-agents.** Run `/debrief 02`. Two writers read the transcript at once, each in its own window, and your chat stays clean. *Then open the outputs: with the recap from step 1, you have three of five documents, and the two that are missing are the two that cannot be written independently.*
3. **Team.** Run `/debrief-team 02`. Watch the two writers post their lists, find the gap, and trade slots. Then check: every term the quiz tests has a card behind it.

## Things to notice

- Writers can only Read and Write. The tool list is a boundary the harness enforces, not something the prompt asks for politely.
- The flashcard writer runs on Sonnet. Pick the model per job.
- The renderer is a sub-agent that uses a skill. `beautiful-html` is knowledge someone else wrote; the agent is the worker that applies it.
- `/debrief` and `/debrief-team` are marked `disable-model-invocation`, so only you can press them.
- **Not every subset needs a team.** The recap email also picks a handful of bullets from a long list of topics, so it looks like it belongs with the quiz and flashcards. It doesn't. Nobody's choices depend on which bullets it picks, so it stays a plain skill.

## Not here yet

A review stage. It belongs between writing and rendering, so corrections land before the HTML is built. An earlier version checked every citation against the transcript and found eight wrong claims in the Module 02 files, including the session report citing a topic 87 seconds before it starts. Worth rebuilding deliberately rather than leaving in half-formed.

## Credits

The `beautiful-html` skill comes from [hamzafarooq/claude-code-starter](https://github.com/hamzafarooq/claude-code-starter). Its templates are by Zara Zhang Rui, MIT licensed.