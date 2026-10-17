---
name: debug
description: 'Use when something is broken, crashing, returning the wrong result, or regressed. Build a reliable repro or failing test first, then narrow the cause, fix it, and keep the regression test.'
---

# Debug

Do not hunt in the source until you can turn the bug on. A theory without a red repro is a story.

## Redact

Commands, logs and traces in your reply must not carry secrets. Replace tokens and personal data with `<REDACTED>`. Prefer environment variables in any harness you add, so the credential never lands in the file you quote.

## 1. Pin the symptom

Write the user's symptom in their words: the action, the expected result, the actual result. If those three are not clear, ask. Confirm you are chasing *that* failure, not a nearby one you happened to notice.

Read `docs/terms.md` if it exists so you name the same things they do.

## 2. Get a red signal

Spend most of your effort here. You need one command you have already run that shows the user's symptom. Prefer the first option that works:

1. A failing test at the layer that reaches the bug.
2. A scripted HTTP or CLI call against a local server.
3. A headless browser step that asserts on the visible symptom.
4. A replay of a captured payload through the offending function.
5. A tiny harness that calls one function with a fixture.
6. For a flake: a loop that raises the hit rate until the bug is debuggable. A 1% failure is not a loop yet.

Tighten what you have. Faster, more deterministic, asserting on the exact symptom. Pin time, seed randomness, cut the network when you can.

**Done with this step** when you can name the command, show its red output, and honestly say it would go green only if *this* bug were gone.

If you cannot build a loop, stop. List what you tried. Ask for access, a redacted log, or permission to add temporary instrumentation. Do not skip ahead to a guess.

## 3. Narrow

Run the loop. Shrink the input and the number of moving parts one cut at a time, re-running after each cut. Keep only what the failure still needs.

Form a short list of causes. Test them against the loop, one at a time. The check that would *refute* a cause is the one worth running. Drop what the evidence kills. When two failed fixes shared a premise, write that premise down and test it directly.

Read the code to place a probe, not to narrate a theory you have not run.

## 4. Fix the first break

Change the place the system first went wrong, not the place that later complained. A guard is a fix only when the state it handles is legitimate. "Belt and braces" that might help is another hypothesis; it does not ship.

Keep the regression test, or the scripted repro if a test is the wrong layer. The red signal from step 2 should now be green. Show both outputs.

If the fix needs product judgement, stop and ask. Do not invent the intended behaviour.

## 5. Close

- Re-run the original symptom, not only the tiny harness.
- Invoke or follow `verify` when the change is more than a one-line test flip.
- Say what was wrong, what you changed, and how to turn the bug back on if someone reverts the test.

Do not leave stray prints, skipped tests or a disabled check in the tree.
