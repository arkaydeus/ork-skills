---
name: refactor
description: 'Use when restructuring, renaming, extracting or cleaning up code without changing behaviour. Get tests green first, take small steps, re-test after each step, and stop if behaviour changes.'
---

# Refactor

Same behaviour, clearer shape. If the user also wants a behaviour change, split that out. A refactor that "also fixes" something is two jobs; the fix needs `debug` or a feature pass.

## 1. Green first

Find the checks that cover the area. Run them. If they are red, stop: you do not have a baseline. Either fix the suite (a different task) or add narrow characterisation tests that lock the current behaviour you can observe.

If there are no tests and you cannot add a cheap one, say so and ask whether to proceed with a weaker baseline (a recorded CLI output, a snapshot). Do not start moving code on a shrug.

Read `docs/terms.md` so renames match the project's words.

## 2. One step

Change one thing. Examples of one step: rename a symbol, extract a function, move a file, delete a dead branch, inline a wrapper that has a single caller. A step you cannot describe in a short clause is too big.

Keep the public behaviour the same: same inputs, same outputs, same errors, same persisted shape, unless the user explicitly agreed a compatibility break and you have recorded it.

Do not add features, logging "while we are here", or a new abstraction for a single call site. If you are tempted, leave it.

## 3. Tests after each step

Re-run the baseline after every step. If it fails, that step changed behaviour. Revert the step. Do not patch the tests to match a slip, and do not pile a second idea on a red tree.

Only then take the next step.

## 4. Stop conditions

Stop and tell the user if:

- a step will not go green without changing what callers see
- the next step needs a product call (rename a published API, drop a field)
- you have lost the thread and cannot name the current step

A failed refactor is a reverted step plus a note, not a half-applied design.

## 5. Close

Run `verify` once the series of steps is done. The evidence is the same commands you started with, plus anything you added to lock behaviour.

In the reply:

- the steps you took
- the commands that stayed green
- any step you reverted, and why
- what you deliberately did not touch

Do not open a pull request from this skill unless the user asked; that is `pr`.
