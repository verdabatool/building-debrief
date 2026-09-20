# Vault

What the course knows, across every module. `outputs/` holds what you ship for one session and is regenerated from the transcript each run; pages here accumulate and are never regenerated.

One agent writes here: `wiki-keeper`, through `/wiki-update NN`. Everything else reads.

## Pages

```
concepts/<term>.md     one per idea an instructor defined or explained
people/<name>.md       instructors, and students who recur
sessions/module-NN.md  what one session covered, linking out
index.md               every page, by category
log.md                 one entry per ingest
```

## Conventions

- Filenames are kebab-case: `human-in-the-loop.md`. A page is named for the idea, not the module.
- Link with `[[kebab-name]]`. A link to a page that does not exist yet is fine: it marks something worth writing, not an error.
- Cite as `[[module-NN]] [HH:MM:SS]`, using the cue start times `CLAUDE.md` defines.
- One page per idea. Two pages describing the same thing get merged, and the loser becomes a line in `index.md` pointing at the winner.
- Record what an instructor said, not what you concluded from it.

## Updating

- **Add and correct, never delete.** An ingest may add a line, and may correct a line it recognises as the same moment described worse. It may not remove a line, including a line its own module put there on an earlier run. Never rewrite a page from scratch.
- A re-run of the same module will find more or fewer topics than last time, because what counts as a topic is a judgment. One pass missing something is not evidence it did not happen, so the thinner pass adds nothing and takes nothing away.
- Deleting is a separate, deliberate operation. `/wiki-lint` proposes removals and a human approves them. Nothing is removed as a side effect of running a command.
- When a session contradicts an earlier one, keep both and name the module that said each. Contradictions are findings, not errors to resolve.

## Source

Pattern adapted from [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) by Andrej Karpathy.
