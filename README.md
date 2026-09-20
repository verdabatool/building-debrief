# Debrief

## Why we built this

Every week a course facilitator finishes a live class and has to ship the same five documents: a recap email, a session report, an FAQ, a quiz, and a set of flashcards. The raw material is a two-hour Zoom transcript, tens of thousands of tokens of speech-to-text with mishearings in it. The work is real, repetitive, and easy to do inconsistently.

Debrief turns that transcript into those five documents, plus HTML renders of three of them.

## The four ideas

| Idea | The problem it solves | Where it is used here |
|---|---|---|
| **Skill** | The same work, done the same way, every time | Recap email, HTML rendering, the three runbooks |
| **Sub-agent** | Several jobs that each need a lot of reading and do not affect each other | Session report, FAQ, the renders |
| **Agent team** | One agent's choice constrains another's, and neither can know it in advance | Quiz and flashcards |
| **Memory and a vault** | Every run starts from zero, and nothing accumulates | Per-agent notes, and `vault/` |

## Repo structure

```
CLAUDE.md                          read first: layout, house rules, your course
transcript/module-N.vtt            input, one Zoom transcript per session
outputs/module-NN/                 generated: five .md deliverables, three .html renders

.claude/
  settings.json                    turns agent teams on
  skills/
    recap-email/SKILL.md           a format: the weekly email
    beautiful-html/
      SKILL.md                     a format: markdown into a styled page
      cartesian.html               the page template it fills
    debrief/SKILL.md               a runbook: report, FAQ, render
    debrief-team/SKILL.md          a runbook: quiz and flashcards, negotiated
    vault/                         the vault plugin, optional and removable
      .claude-plugin/plugin.json   the manifest that makes it a plugin
      README.md                    what it is, and how to switch it off
      agents/wiki-keeper.md        the only agent allowed to write the vault
      skills/wiki-update/SKILL.md  becomes /vault:wiki-update
  agents/
    report-writer.md               opus   · memory · Read, Write
    faq-writer.md                  opus   · memory · Read, Write
    quiz-writer.md                 opus   · memory · Read, Write
    flashcard-writer.md            sonnet · memory · Read, Write
    renderer.md                    sonnet ·        · Read, Write + beautiful-html
  agent-memory/                    written by the agents themselves, not committed

vault/                             what the course knows, across every module
  README.md                        the conventions and merge rules
doc/README.md                      the long guide, with diagrams
```

`NN` is the module number, accepted with or without the leading zero. Add a session by dropping `module-N.vtt` into `transcript/`. Eight files land in `outputs/module-02/`, and that folder is regenerated on every run and is not committed.

## How each one is used here

### Skills

A skill turns a repeatable task into a command. The recap email needs the same subject line and the same closing lines every week, and the cost of doing that by hand is inconsistency rather than effort, so the format lives in `.claude/skills/recap-email/SKILL.md`. Change the subject line there and every future email changes, with nothing else touched.

`beautiful-html` is the same idea for turning a finished markdown file into a styled page. `debrief`, `debrief-team` and `wiki-update` are a different kind of skill: they hold a procedure rather than a format, write nothing themselves, and exist to start the agents in the right order.

### Sub-agents

An agent gets its own context window and its own tool list. The session report and the FAQ each need the whole transcript, and both are exhaustive, the report covering every topic and the FAQ every question. Neither makes a choice that affects the other, so they run at the same moment in separate windows and hand back a short summary each. Your own chat never holds the transcript.

The `renderer` is the third, and a different kind of example: an agent that uses a skill. `beautiful-html` is the knowledge, the renderer is what applies it, and the page template never enters your context.

### Agent teams

The quiz and the flashcards are one study artifact. The quiz picks eight questions, the flashcards pick twelve terms, and a student uses both. If the quiz tests a term the cards never define, someone who memorised every card still fails that question.

Neither writer can prevent that alone, because the thing each one needs to know sits in the other's head at the moment it is needed. So they trade: twenty slots between them, under two rules. If the quiz tests a term, a card defines it. If a card defines a term, something uses it.

They negotiate together and then write in turn, the quiz first because its answers are pinned to timestamps and have the least room to move. The lead checks both finished files against the two rules on disk, not against the lists that were agreed, because the two can differ.

### Memory and the vault

Four agents carry `memory: project` and keep a notebook under `.claude/agent-memory/`: terms already carded, topics already taught, mishearings already agreed. It stays terse on purpose, because it is loaded into that agent's context on every run.

The `vault` plugin is the other half. `/vault:wiki-update NN` hands the finished module to `wiki-keeper`, the only agent allowed to write in `vault/`, which keeps one page per idea and adds a line each time a later module returns to it. Nothing in the pipeline depends on it: disable the plugin and the first three commands behave exactly as before.

## How to run each one

Run them in this order. Each one leans on what the last produced.

### Skills

```
/recap-email 02
```

Writes `outputs/module-02/recap-email.md`, under 350 words, in the template's exact shape. Then look at your context meter: the whole transcript is now sitting in your chat, because a skill runs where you are. That is fine once, and it is the reason for the next command.

### Sub-agents

```
/debrief 02
```

Starts `report-writer` and `faq-writer` in the same turn, so both read the transcript at once in separate windows, then starts `renderer` on the finished report. Writes `session-report.md`, `faq.md` and `session-report.html`. Watch what comes back into your chat: a list of topic titles and a count of deferred questions, not the transcript.

### Agent teams

```
/debrief-team 02
```

Needs `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`, which `.claude/settings.json` sets, and it is read when the session starts, so restart Claude Code after adding it. Run it after `/debrief`, because both writers read the session report. They post their lists, trade slots, then write in turn and the lead checks both files. Writes `quiz.md`, `flashcards.md` and the two renders.

### Memory and the vault

```
/vault:wiki-update 02
```

Optional, and run last, so the concept pages can record which quiz question and which flashcard use each term. Hands the module to `wiki-keeper`, which writes one page per idea into `vault/`. Memory needs no command at all: the four agents carrying `memory: project` have been filling `.claude/agent-memory/` since your first run. The payoff for both arrives on the second module, when a concept page gains a line rather than a duplicate appearing.

## Reuse it for your own course

Edit the "This course" section of `CLAUDE.md`: the course name, the instructors, and where questions and resources come from. Nothing above that section changes, and no file in `.claude/` changes at all. Then drop the next transcript into `transcript/` and run the commands in order.

## Credits

The `beautiful-html` skill comes from [hamzafarooq/claude-code-starter](https://github.com/hamzafarooq/claude-code-starter). Its templates are by Zara Zhang Rui, MIT licensed. The vault pattern is adapted from [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) by Andrej Karpathy.
