# Feature

Build new or changed behaviour from a named intent. Stay in the lead: shape and plan first, then implement, then prove it.

1. Read `docs/terms.md` if it exists. Use those words in the plan and in code.
2. If the goal, audience or constraints are still fuzzy, invoke `shape` and wait until the open decisions are written to `docs/decisions/`.
3. Invoke `plan`. Do not write production code until the user accepts the plan, or clearly tells you to proceed without a written plan (`skip: user waived the written plan`).
4. Implement only what the accepted plan names. Keep each step small enough to verify. Use canonical terms. Do not sneak in refactors or extras.
5. Invoke `verify`. The new behaviour must be exercised on a real surface (test, command or UI), not inferred from a compile.
6. Invoke `review` on the diff.
7. If the user wants the change proposed for merge, invoke `pr`. Otherwise `skip: no pull request requested`.
