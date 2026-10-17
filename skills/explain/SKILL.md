---
name: explain
description: 'Use only when the user explicitly invokes this skill by name (for example /explain or "run explain"). Read-only. Explain how a piece of the system works by tracing the flow, and why it is that way from git history and docs/decisions.'
---

# Explain

Answer how and why. Touch no production file, no test and no doc except to read them. If you notice a bug, mention it; do not fix it in this skill.

## Scope

Restate the question in one sentence. If it covers a whole product, pick the subsystem the user named, or the one the open files point at, and say so.

Read `docs/terms.md` first and use those words. Read matching files under `docs/decisions/` and `docs/plans/` before you open a long source file.

## How: trace the flow

Start from an entry the user would recognise: a route, a command, a handler, a public function. Walk the runtime path, not the file tree for its own sake.

For each hop, record:

- the symbol and path
- what data it receives and what it returns
- where it branches
- who owns the state

Stop when a competent engineer could predict where a change should live. Do not paste functions in full. One or two short excerpts are enough when the shape is surprising.

If the area is large, split the trace into two or three angles (for example "write path" and "read path") and walk each. You may spawn read-only subagents to gather those angles; you still write the explanation.

## Why: history and decisions

How is not why. For the surprising turns in the trace, look for evidence:

- `docs/decisions/` entries that name this area
- commit messages and `git log -L` or `git blame` on the odd lines
- pull request bodies, if `gh` can see them
- comments that explain a constraint, not comments that narrate the code

Cite what you used: path, commit hash, decision file. Label each claim:

- **Recorded** — a decision file or commit says this
- **Inferred** — the code does this, and history is silent
- **Unknown** — you could not find a reason

Never invent a motive. "I cannot see why this is a queue rather than a table" is a valid answer.

## Reply

```markdown
## Question
<the scoped question>

## How it works
<the flow, in order, with paths>

## Where it lives
<the modules that own this, and what should not be touched for a typical change>

## Why it is this way
<only the turns that need a reason, each labelled recorded / inferred / unknown>

## Gotchas
<traps a changer would hit: hidden callers, dual writes, feature flags>
```

If `docs/decisions/` is silent and git is thin, say that the why section is thin. Do not pad it with guesses.

Offer a follow-up: `shape` if they now want to change it, `plan` if the change is large. Do not start that work here.
