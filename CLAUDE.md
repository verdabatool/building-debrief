# Debrief

Turns a class transcript into the five documents a course facilitator ships after each session.

## Layout

```
transcript/module-N.vtt      input, one Zoom transcript per session
outputs/module-NN/           five .md deliverables plus three .html renders
.claude/skills/              reusable instructions: formats and the /debrief runbook
.claude/agents/              workers with their own context and tools
.claude/agent-memory/        what each worker carries across modules
vault/                       what the course knows across all modules
```

Add a transcript, run the command, get a folder. Nothing else changes between sessions or between courses.

## Commands

- `/recap-email NN` writes the recap email in this chat.
- `/debrief NN` builds the session report and FAQ with sub-agents, then renders the report.
- `/debrief-team NN` builds the quiz and flashcards as two teammates who agree coverage first. Run it after `/debrief`. Needs the env var in `.claude/settings.json`.
- `/vault:wiki-update NN` ingests the finished module into `vault/`. Run it last. Comes from the `vault` plugin in `.claude/skills/vault/`, which can be disabled.

`NN` is the module number. It selects `transcript/module-NN.vtt`, accepted with or without a leading zero, and names the output folder `outputs/module-NN/`.

## Reading a transcript

These rules hold for any course and any module. The specifics live in the next section.

- Transcripts are Zoom `.vtt` files. Read the whole file before writing anything.
- Cite the **start** time of a cue, trimmed to seconds: `[00:01:35]`. A cue end time is not a valid citation, because it lands in the gap between speakers.
- Consecutive cues from the same speaker are one turn. Read them together.
- The speaker name is the text before the first colon in a cue. Anyone named as an instructor or facilitator in the course section below is staff. Everyone else is a student, referred to by first name.
- A **topic** is something an instructor explains for over a minute. A **demo** is a screen share. An **announcement** is anything about assignments, schedule, setup, or where to find resources. A **question** is deferred if the instructor says they will cover it later.
- Questions are sometimes read aloud from a chat or a Q&A tool. Attribute them to whoever the instructor names. If no name is given, say so rather than guessing.
- This is machine transcription, so expect errors: misheard product names and people's names, dropped words, and phrases that make no sense as written. Use your judgment. When the intended meaning is clear from context, write what was meant rather than what was heard. When it is genuinely ambiguous, keep the transcript's wording rather than invent something.

## This course

Replace this section when you reuse the repo. Nothing above it needs to change.

- **Course:** "Claude Code in Practice + Agentic AI for Product Managers", a Maven cohort for product managers.
- **Instructors:** Hamza Farooq, Aishwarya Ashok. **Co-facilitator:** Verda Batool.
- **Questions** come from the call and from Slido. **Resources** live on Maven.

## Rules

- Never edit files in `transcript/`.
- `outputs/` is per session and regenerated. `vault/` is cumulative and merged; only `vault:wiki-keeper` writes it, and its rules live in `vault/README.md`.
- Outputs are regenerated from the transcript. Do not hand-edit them; fix the skill or agent instead.
- Keep every file short. This is a teaching repo and people read it top to bottom.