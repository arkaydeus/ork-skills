---
name: handoff
description: 'Use only when the user explicitly invokes this skill by name (for example /handoff or "run handoff"). Write a docs/handoffs note covering what is done, what is open, the decisions, and the exact next step.'
---

# Handoff

Leave a note that a fresh agent, or a later you, can pick up without rereading the thread. The note is the artefact. Do not keep working after you write it unless the user says to continue.

## When to write

Invoke this at a pause, a change of owner, or the end of a long session that did not ship. If the user told you what the next session is for, aim the note at that.

## Gather, do not invent

Read the conversation, `git status`, the diff, and any files already in `docs/plans/`, `docs/decisions/` and `docs/terms.md`. Quote those paths. Do not paste their contents. Do not claim a test passed unless you still have the command output.

Redact secrets, tokens and personal data. Write `<REDACTED>` rather than a guessed stand-in.

## File

Create `docs/handoffs/` if needed. Write `docs/handoffs/YYYY-MM-DD-<slug>.md`. The slug is the work, not the word "handoff" twice.

```markdown
# Handoff: <short title>

- Date: YYYY-MM-DD
- Branch: <name or "none">
- Plan: <docs/plans/... or "none">
- Decisions: <paths, or "none">

## Done

- <what is actually in the tree, with paths>

## Open

- <unfinished work, known bugs, skipped playbook steps with their reasons>

## Decisions the next agent must not relitigate

- <one line each, with a path to docs/decisions/ or "stated by the user in this session">

## Exact next step

<one action the next agent should take first, including the command or file to open>

## Suggested skills

- <ork playbook or skill names that fit the next step>

## Do not

- <traps: the spike folder that must not merge, the test that is still red on purpose, the env that is missing>
```

## Quality bar

- "Exact next step" is one action, not a paragraph of options.
- "Done" is only what a `git status` and a file read would confirm.
- If nothing was decided, say so. An empty decision list is honest; a reconstructed one is not.
- Keep it short enough to read in a minute. Detail lives in the linked files.

## Reply

Give the path and the exact next step. Then stop.
