# Bug

Restore correct behaviour. Evidence first. A change that is not tied to a repro is a guess and does not ship.

1. Invoke `debug`. Capture the user's symptom, produce a red repro or failing test, narrow the cause, fix the first place the system went wrong, and keep the regression test.
2. Invoke `verify` against that same symptom. The original repro must now pass. A different nearby test going green is not enough.
3. Invoke `review` on the fix. Watch for a patch that hides the symptom without moving the cause.
4. If the user wants the fix proposed for merge, invoke `pr` and link the repro evidence in the body. Otherwise `skip: no pull request requested`.
