---
name: security-supply-chain
description: 'Use when the work is dependency risk, lockfiles, typosquatting, install scripts, pinned versions, GitHub Actions hardening, pull_request_target, untrusted input in workflow run steps, or secrets in CI. Review manifests, lockfiles and workflow YAML; do not start a full-repo product audit.'
license: MIT
---

# Supply chain

Review how this repo **gets code and identity**, not the product's business logic. For application bugs in a diff, invoke `security-review`. For a whole-tree hunt, invoke `security-audit` and keep this as one area.

Read `references/ci-hardening.md` next to this skill when you reach workflows. Findings use the same confidence gate as `security-review`: attacker, path, impact, no silent blocker.

## 1. Inventory

List what exists; skip ecosystems that do not.

- Manifests and lockfiles: `package.json`, `package-lock.json` / `pnpm-lock.yaml` / `yarn.lock`, `requirements*.txt`, `poetry.lock`, `Pipfile.lock`, `pyproject.toml`, `go.mod` / `go.sum`, `Cargo.toml` / `Cargo.lock`, `Gemfile.lock`, `composer.lock`, `mix.lock`, Docker `FROM`, git submodules.
- Install hooks: `preinstall` / `postinstall`, `setup.py` extras, Makefile download targets, `curl | sh` in docs or scripts.
- CI: `.github/workflows/`, other vendors' pipeline files, reusable workflows, actions referenced by tag vs SHA.
- Release: publish jobs, signing, npm `publishConfig`, PyPI trusted publishing, container registries.
- Secrets: workflow `env`, repo scripts, sample `.env`, committed key material.

If they have scanners installed, you may run `osv-scanner`, `npm audit`, or `pip-audit` and triage as in `security-scan`. Tools are optional; the YAML and lockfiles are not.

## 2. Dependencies

Look for:

- **No lockfile** on an app that installs in CI or production. That is a finding.
- **Pins.** Ranges in a library's manifest can be fine; an application that installs without a lock is not. Git dependencies without a commit SHA. Image tags like `node:latest`.
- **Typos and confusion.** Names one character off a popular package; internal names that could be claimed on the public registry; mixed registries without an always-auth / scope config.
- **Install scripts.** `postinstall` that fetches the network, builds native code from an unpublished tarball, or runs `curl`.
- **Abandoned or huge** new additions on a sensitive path (auth, crypto, payments) without a reason in the diff.
- **Advisory overlap.** A known CVE on the **locked** version, if you can check. Do not file a CVE that only exists on a version they do not install.

Do not demand a pin on every transitive of a public library unless this repo is the application that ships.

## 3. Workflows and CI

Open every workflow file. Hunt:

- `pull_request_target` (and related: `workflow_run` that checks out the untrusted head, `issue_comment` that runs body text). These run with the **base** secrets against **fork** code if misused.
- `permissions:` missing (defaults too wide) or `write-all`. Prefer a top-level `contents: read` and job-level extras.
- Actions pinned to a **moving tag** (`@v4`, `@main`) rather than a full commit SHA. Tags can move.
- `run:` steps that interpolate untrusted event fields (`github.event.pull_request.title`, body, label, issue, branch name) **unquoted** into the shell. That is script injection.
- `actions/checkout` of a pull-request head on a privileged workflow, then `npm install` / `make` / test that executes that tree.
- `pull_request` workflows that still have `write` tokens, deploy keys, or package-publish rights they do not need.
- Secrets printed, passed to forks, or used in `pull_request` from public forks (they are often empty — do not confuse that with safety on `pull_request_target`).
- Dangerous `workflow_dispatch` inputs concatenated into shell.
- Self-hosted runners on public repos that accept fork PRs.

A workflow that only comments with `pull-requests: write` on `pull_request` is usually fine. A workflow that deploys on `pull_request_target` is not.

## 4. Secrets in CI and the tree

- Long-lived cloud keys in repo secrets used where OIDC could mint a short credential.
- Secrets in `echo`, debug logs, or `set -x` scripts.
- `.env`, PEM files, `id_rsa`, `*.p12`, `credentials.json` tracked by git.
- `NPM_TOKEN` with publish scope on a job that runs untrusted tests.

Live-looking material: tell them to rotate. Redact in the reply.

## 5. How to write a finding

Same shape as `security-review`: title, severity, where, path (who can trigger it), impact, fix, why it is not a false positive.

Severity hints:

- Untrusted code execution on a privileged workflow: **Critical** / **High**.
- Script injection into `run:` with a write token: **High**.
- Missing lockfile on a deployed app: **High** or **Medium** depending on whether CI is the production install.
- Action pinned to a tag: **Medium** (or **High** if that action holds deploy credentials).
- Typosquat candidate: **High** if it is newly added and the name is wrong; **Medium** if it is a confusion risk not yet exploited.

Fix guidance must be concrete: pin this SHA, drop `pull_request_target`, quote this expansion, add `permissions`, add a lockfile, remove the `postinstall`.

## Reply

```markdown
## Inventory
- Ecosystems: …
- Workflows: <n> files
- Tools run: …

## Findings
### <title>
- Severity: …
- Where: …
- Path: …
- Impact: …
- Fix: …

## Residual
- <optional hardening, or none>
```

Do not apply workflow edits unless they ask after the list. Do not "fix" a pin by guessing an SHA you did not resolve (`git ls-remote` or the action's tags page).
