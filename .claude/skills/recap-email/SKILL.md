---
name: recap-email
description: Write the post-session recap email for a module. Use when
  asked for a module's recap email or the note that goes out to students
  after a session. Run as /recap-email NN.
argument-hint: [module-number]
---

# /recap-email $ARGUMENTS

You are the course facilitator writing to the students. Unlike `/debrief`, this skill runs where you are, so the transcript lands in your own context.

When run with a module number:
1. Read `transcript/module-$ARGUMENTS.vtt` in full, accepting the name with or without a leading zero.
2. Take the topics from what the instructors taught, using the definition in `CLAUDE.md`: something an instructor explained for over a minute.
3. Collect every announcement the instructors actually made. Never invent a deadline.
4. Write `outputs/module-$ARGUMENTS/recap-email.md`, in this exact format:

```markdown
**Subject:** Module $ARGUMENTS recap and next steps

Hi everyone,

<One warm sentence about the session.>

## What we covered

- <A topic the instructors taught, one line each, in the order taught. Five to eight bullets.>

## Announcements and instructions

- <Every assignment, deadline, setup step, or schedule note the instructors gave.>

## Next steps

- <What students should do before the next session.>

If anything is unclear, reach out to the course staff. We read every message.

All slides, recordings, and resources for this module are on <resource platform>.

See you next session,
The course team
```

## Rules

- Subject line is exactly `Module NN recap and next steps`.
- "What we covered" holds topics only. Leave out breakouts and student exercises, student questions and the answers to them, reviews of student work, and anything about logistics. Those belong in the other sections or nowhere.
- The last two lines before the sign-off are mandatory: one telling students to contact course staff, one saying where the resources are. Take the platform name from the course block in `CLAUDE.md`.
- Plain, friendly, second person. No jargon the students did not hear in class.
- Under 350 words.

Be concise. Reply with the file path and the word count. Never write anything except that one file.
