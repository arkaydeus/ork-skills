---
name: plan
description: 'Use only when the user explicitly invokes this skill by name (for example /plan or "run plan"). Write docs/plans/<slug>.md covering goals, steps, risks and verification, then get sign-off before any production code.'
---

# Plan

Write the plan the work will follow. The plan is the artefact. Code comes after sign-off, not before.

## Gather

Read the request, `docs/terms.md`, `docs/decisions/` and any existing file under `docs/plans/` that overlaps. Inspect the code you will touch well enough to name real files and module boundaries. If a product choice is still open, stop and tell the user to invoke `shape` first. Looking up facts is your job; do not stall the plan on a question a file can answer.

If the user already accepted decisions in this conversation, treat those as inputs. Quote their paths in the plan rather than restating them at length.

## Choose a slug

Pick a short kebab-case slug from the goal, for example `billing-retry-queue`. The file is `docs/plans/<slug>.md`. Create `docs/plans/` if it does not exist. If a file with that slug already exists, extend it in place or choose a more specific slug. Do not silently overwrite a plan that belongs to other work.

## Write the plan

Use this shape. Drop a section only when it would be empty, and say why in the reply.

```markdown
# <Plan title>

- Status: proposed
- Date: YYYY-MM-DD
- Decisions: <paths under docs/decisions/, or "none">
- Terms: <canonical names, or "none">

## Goal

<one paragraph: who this is for and what will be true when we are done>

## Out of scope

<what we are deliberately not doing, so the implementation cannot grow>

## Steps

1. <action, the files or surfaces it touches, and the check that the step is done>
2. ...

## Risks

- <what could go wrong, how we would notice, and what we would do>

## Verification

- Tests: <commands or cases>
- Types and lint: <commands the repo already uses>
- UI or manual: <the path a person or browser should exercise, or "not user-visible">

## Open questions

- <only questions that still need the user; omit this heading if there are none>
```

## Quality bar

- Steps are ordered and small enough that each one can fail on its own.
- File paths and command names are real, not "add tests" with no target.
- Risks are specific to this change. "It might break something" is not a risk.
- Verification names how we will know, not that we will "be careful".
- Use the words from `docs/terms.md`. If you must introduce a term, update the glossary in the same sitting.

## Sign-off

Show the path and a short summary. Ask the user to accept the plan, or to say what to change. Until they accept it, or they explicitly waive a written plan, do not edit production code.

When they accept, set `Status: accepted` in the file. Then stop. Implementation is a different skill (`ork` with the feature playbook, or ordinary coding if they prefer). If they reject it, revise the file and ask again.

## Revisions

If work has already started and the plan is wrong, update the file and record what changed at the top as a dated note. Do not leave a stale plan in `docs/plans/` that no longer matches the work.
