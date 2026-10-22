---
name: pr
description: 'Use when the user asks to commit, open a pull request, or propose a change for review. Create a branch if needed, write commits, draft a PR body covering what, why and how to test, link plan and decision files, and open the PR with gh.'
---

# Pull request

Package the work so a reviewer can judge it. Open the pull request. Do not merge unless the user asked to merge.

## 1. See what you have

Run `git status`, `git diff` and `git log --oneline` against the base branch. Read any `docs/plans/<slug>.md` and `docs/decisions/` files this work produced. If the tree mixes unrelated changes, stop and say so. Do not dump two features into one request.

If nothing is committed and nothing is staged, you still own the split: one commit per coherent step, message in the imperative, reason in the body when the subject is not enough.

## 2. Branch

If you are already on a named working branch that is not the default, stay there. Otherwise create one from the up-to-date base. Follow the repo's existing branch pattern. If it has none, use a short `kebab-case` name that matches the work.

Do not commit onto `main` or `master`.

## 3. Commits

Write commits the reviewer can replay. Prefer the project's history style. A useful default:

- subject: imperative, specific (`Retry failed webhooks with backoff`)
- body: why, and any decision the diff does not show

Do not invent a conventional-commit prefix the repo does not use. Do not include secrets. Do not use a filler subject such as `update` or `fix stuff`.

## 4. Body

Write the pull request body in British English, using terms from `docs/terms.md`. Link the artefacts; do not paste them.

```markdown
## What

<the user-visible or API change in a few sentences>

## Why

<the problem or decision this serves, with links>

## How to test

- <exact commands>
- <the UI path if the change is visible>

## Artefacts

- Plan: <docs/plans/... or "none">
- Decisions: <docs/decisions/... or "none">
```

Add screenshots or command output when they are the proof. A before and after pair beats a paragraph that says it works.

If `verify` has already been run in this session, reuse that evidence. If it has not, run the checks that the repo expects and put the results under **How to test**.

## 5. Open it

Use `gh pr create` with the base branch, the title and the body. If `gh` is missing or unauthenticated, write the body to a file, say so, and stop rather than inventing a web submission.

Push the branch first. Follow the remote's usual upstream name.

Put the pull request URL in the reply as a full `https://github.com/<owner>/<repo>/pull/<n>` link when that is the host.

## 6. Do not

- Force-push to a shared branch.
- Merge, unless the user explicitly asked.
- Open a pull request for a `spike/` branch or a `spikes/` folder. Those are not for merge.
- Ignore hooks or `--no-verify` to get a green illusion.
