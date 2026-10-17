---
name: verify
description: 'Use before declaring work done, and whenever a change needs proof. Run the relevant tests, typecheck, lint and a UI check where the change is user-visible. Report evidence, not claims.'
---

# Verify

Proof is a command you ran, an assertion you saw, or a screen you exercised. "It should work" is not proof.

## 1. Discover how this repo checks itself

Look for the commands the project already uses: `package.json` scripts, `Makefile`, `pyproject.toml`, `go test`, a `justfile`, CI config. Prefer those over a tool you happen to like. If several suites exist, run the ones that cover what you touched, then say which ones you skipped and why.

Read `docs/plans/<slug>.md` when this work has a plan. Its Verification section is the list you owe.

## 2. Automated checks

Run, in an order that fails fast:

- the targeted tests for the change
- the broader suite if it is cheap, or say why you did not
- typecheck, when the language has one
- lint or format, when the repo already runs them

Quote the invocation and the part of the output that carries the verdict. A green line from a run you did not start does not count.

If a check is broken on the base branch and unrelated to your diff, say so and do not hide it. If your diff broke it, it is yours.

## 3. The real surface

If a person would see the change in a UI, drive that path. Use the session's browser or UI tool when you have one. If you do not, say that the UI was not exercised and what a person should click.

If the change is a CLI or an API, run the command or request against a running instance or a fixture. Compile-only is not enough for a behaviour change.

If the change is a performance claim, this skill is not the measurement method: follow `perf` and then paste those numbers here as evidence.

## 4. What counts as done

You may say the work is verified only when:

- each check you named has a result from this session
- failures are either fixed or explicitly accepted by the user
- the original request's acceptance case was hit (the bug's symptom, the feature's path, the refactor's suite)

Label every claim:

- **Ran** — command and result
- **Looked at** — the UI path or file you inspected
- **Not run** — and why

Do not upgrade "not run" to a pass.

## 5. Reply

```markdown
## Evidence

- Tests: ` <command> ` — <verdict>
- Types: ...
- Lint: ...
- UI: <path and what you saw, or "not user-visible">

## Gaps

- <anything not run, or a flake you could not pin>
```

If verification failed, stop calling the task done. Fix or ask. Do not open a "green" pull request on a red suite.

## Honesty

Do not use `--no-verify`, a skipped test or a cached screenshot as a stand-in. If you cannot run a check in this environment, write **Not run** and what blocked you.
