---
name: review
description: 'Use when asked to review a diff, a branch or a change, or when a playbook calls for a single-pass review. Read once on two axes, correctness and design. Do not start a multi-model panel unless the user asked for one.'
---

# Review

One careful pass. Two axes. Findings, not a rewrite. If the user wanted independent models or an adversarial panel, stop and tell them to invoke `panel`.

## Scope

Take the paths they named, or the current branch against its base (`git diff <base>...HEAD`), or the files already in play. If the scope is empty, ask. Read the matching plan and decision files so you review the change they meant, not the change you would have preferred.

State the intent in two or three sentences before you list issues.

## Axis 1: correctness

Does the change do what the intent claims, under the conditions the code will actually see?

Look for:

- wrong results, dropped edges, off-by-one and empty cases
- error paths that hide or mis-report the failure
- races, double writes, and work that is not safe to retry
- trust of input at a boundary
- tests that cannot fail when the behaviour breaks, or that are missing for a new risk
- a "fix" that silences a symptom and leaves the cause

Run the project checks if they are cheap and relevant. A review that never runs the test the change claims to pass is incomplete. Quote failures. Do not claim they pass if you did not run them.

## Axis 2: design

Is this the shape a later editor can live with?

Look for:

- names that do not match `docs/terms.md` or that mean two things
- a module that now owns two jobs, or a new layer that only forwards a call
- duplication that will drift
- an API that makes an illegal state easy to write
- comments that narrate the code, or comments that hide a real constraint
- extra work that the intent did not ask for

Do not demand a redesign that is out of scope. "I would have split this package" belongs in Consider, not in a blocking list, unless the split is already on fire.

## How to write a finding

Each finding is one issue:

- **Title**
- **Axis:** correctness or design
- **Where:** path and symbol
- **Why it matters** for the stated intent
- **Severity:** block, should fix, or note

Skip nits that do not change behaviour or understanding. Skip style that the repo does not already enforce. Skip praise.

## Reply

```markdown
## Intent
<what this change is for>

## Correctness
- ...

## Design
- ...

## Checks run
<commands and results, or "none">
```

Do not apply edits unless the user asks after they have seen the list. If they want several models to argue, that is `panel`.
