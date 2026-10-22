# Refactor

Change structure only. Behaviour stays put. If behaviour moves, stop and treat that step as a failed refactor.

1. Invoke `refactor`. Confirm the existing checks are green, take one small step at a time, and re-run tests after each step.
2. Invoke `verify` on the same suite you started from, plus any characterisation tests you added. Report the commands and results.
3. Invoke `review`. The bar is "same behaviour, clearer shape", not "nice to have extras".
4. If the user wants the refactor proposed for merge, invoke `pr`. Otherwise `skip: no pull request requested`.
