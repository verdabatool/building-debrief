# vault plugin

The knowledge layer, kept separate from the debrief pipeline so it can be added to a course later, or left off entirely.

```
skills/wiki-update/   /vault:wiki-update NN
agents/wiki-keeper.md the vault's only writer
```

The pipeline in `.claude/` builds what you ship for one session. This plugin builds what the course knows across all of them, in `vault/` at the repository root. Nothing in the pipeline depends on it: disable this plugin and `/debrief` and `/debrief-team` behave exactly as before.

## Using it

Run it last, after the deliverables exist, so concept pages can record which quiz question and which flashcard use a term.

```
/debrief NN
/debrief-team NN
/recap-email NN
/vault:wiki-update NN
```

## Turning it off

```bash
claude plugin disable vault@skills-dir
claude plugin enable  vault@skills-dir
```

## Notes

- Loads automatically for anyone who opens this repository, after they accept the workspace trust prompt. Start Claude Code from the repository root: project plugins load from the session's working directory and do not walk up.
- Edits to `SKILL.md` apply immediately. Edits to `agents/wiki-keeper.md` need `/reload-plugins`.
- The vault's own conventions and merge rules live in `vault/README.md`, not here. This folder is the machinery; `vault/` is the content.
