---
name: security-scan
description: 'Use when asked to run security scanners, Semgrep, secret scanning, dependency audit, CodeQL, npm audit, pip-audit, osv-scanner, gitleaks, or trufflehog. Run only tools that are already installed. Triage hits into confirmed issues; do not paste raw output. Never copy or vendor scanner rules into the repo.'
license: MIT
---

# Security scan

Run **installed** tools, then read the hits. The deliverable is a triaged list, not a log dump. If they wanted a reasoning review of a diff, invoke `security-review` and only use this skill for extra signal. If no scanner is present, say so and stop; do not invent a ruleset and do not add Semgrep rules to the tree.

## Rules

- Detect with `command -v` / version flags. Missing tool → skip, named in the reply.
- Do not install packaged rule collections into this repository. Do not copy `.yml` rules, CodeQL queries, or another project's scanner config into the repo so the scan can run.
- Semgrep: use the CLI's public registry (for example `semgrep scan --config auto`). Do not vendor rules.
- Do not print secret values. Quote a redacted snippet (`<REDACTED>`) or a file:line only.
- Do not write exploits. Confirm by reading the source, not by attacking a host.
- Do not fail the session because a tool is absent. The other siblings still work.

## 1. Discover

Look at the language and manifests. Then check what exists on PATH (and common wrappers: `npx`, `pipx`, docker images the repo already uses). Typical sets:

| Kind | Tools (use what is installed) |
| --- | --- |
| SAST | `semgrep` (`semgrep scan --config auto`), CodeQL CLI (`codeql`) if a database or workflow already exists |
| Secrets | `gitleaks`, `trufflehog` |
| Dependencies | `osv-scanner`, `npm audit`, `pip-audit`, `cargo audit`, `govulncheck`, `bundler-audit` |

Also use the project's own CI script when it already wraps one of these. Prefer that invocation over a parallel flag set.

Scope: paths they named, else the repo root. For a PR, prefer the changed files when the tool supports it; otherwise scan the tree and filter to the diff in triage.

## 2. Run

Run only the tools you found. Fail-soft: a crash is "not run" plus the error, not a finding.

Suggested shapes (adjust to what `--help` actually offers):

```bash
semgrep scan --config auto --error --quiet
gitleaks detect --no-banner
trufflehog filesystem . --no-update
osv-scanner scan .
npm audit --json
pip-audit
```

CodeQL: only if a database is already built or the repo's workflow documents a one-command path you can run here. Do not upload to GitHub from this skill. Do not spend the session compiling a database unless they asked for that.

Cap the paste: keep the command, the exit code, and the count. Write long JSON to a temp file you read; do not flood the thread.

## 3. Triage

Open every hit that might be real. For each:

1. **Reachable?** Is this code on a path an attacker can hit, or test/generated/vendor noise?
2. **True sink?** Does the rule's story match the source (taint dies in an encoder, a constant, a dead branch)?
3. **Already known?** Lockfile pin, `# nosemgrep` with a reason, documented exception.

Bucket:

| Bucket | Meaning |
| --- | --- |
| **Confirmed** | You opened the code; the hit holds. Severity from impact, same scale as `security-review`. |
| **Needs a human** | Plausible; one fact missing. |
| **Discard** | False positive, vendor, test fixture, or unreachable. One-line reason. |

Do not list Discard items one-by-one unless there are few and they teach a config tweak. A count is enough.

Secret hits: treat any live-looking credential as Confirmed even if "it might be a test key", unless you can see it is a documented placeholder. Tell them to rotate. Do not repeat the secret.

Dependency hits: check whether the locked version is actually the one built, whether the advisory applies to the feature they use, and whether a fix version exists. Transitive noise with no reachable function is Needs a human or Discard, not a pager event.

## Reply

```markdown
## Tools
| Tool | Status | Notes |
| --- | --- | --- |
| semgrep | ran `…` / not installed | n findings before triage |
| … | | |

## Confirmed
- <title> — <tool> — <path> — <severity> — <why it holds> — <fix>

## Needs a human
- <title> — <missing fact>

## Discarded
- <count>, e.g. <one example and why>

## Not run
- <tool>: <why>
```

If you confirmed an issue in changed code, you may say that `security-review` would agree. Do not start a full `security-audit` from a scanner run unless they ask.

## Never

- Commit `.semgrep/`, copied `rules.yaml`, or CodeQL query packs "so the next run works".
- Check in scanner caches or secret reports with live values.
- Claim "clean" when the only tool ran on a subdirectory, or when triage was skipped.
