---
name: security
description: 'Use when the user asks for security help without naming a more specific skill, or when they invoke this skill by name (for example /security or "run security"). Classify the request and load the cheapest matching sibling: a diff review before a full-repo audit, scanners when tools are the point, supply-chain when the subject is dependencies or CI, a threat model when they want assets and STRIDE. Do not start a full audit when a pull-request review would answer.'
license: MIT
---

# Security

You are the dispatcher, not the auditor. Pick one sibling, load it by **name**, and follow it in full. Installs of this set are flat: never point at `../security-review` or any relative path.

If a more specific sibling is already loaded (`security-review`, `security-audit`, `security-scan`, `security-supply-chain`, `security-threat-model`), stop. Do not wrap it.

## 1. Classify

Read the request, the open files, and whether a diff already exists (`git status`, `git diff`, an attached pull request). Pick **exactly one**:

| Sibling | Choose it when |
| --- | --- |
| `security-review` | The object is a change: a diff, a branch, a pull request, or "is this patch safe?" Default when a base branch exists and they did not ask to cover the whole tree. |
| `security-scan` | They want tools run: Semgrep, secret scanners, dependency audits, CodeQL. The output is triaged scanner results, not a design review. |
| `security-supply-chain` | The subject is lockfiles, registries, install scripts, pinned versions, GitHub Actions, or secrets in CI. |
| `security-threat-model` | They want a model: assets, actors, trust boundaries, STRIDE-style threats, mitigations. No hunt yet. |
| `security-audit` | They named a full-repo audit, a pen-test of the tree, or coverage of every surface, **and** a cheaper sibling would not answer. |

Cheapest path first. A pull request is `security-review`, even if they said "audit this PR". A whole-tree ask that is really "run the linters" is `security-scan`. Only `security-audit` when they accepted the cost of a phased, token-heavy pass.

If two rows could fit, pick the one that matches the **deliverable** (findings on a diff, scanner triage, a threat-model document, a coverage-backed audit) and say why.

## 2. Confirm the expensive ones

If you picked `security-audit`, say so in one sentence: it maps the tree, hunts in phases, and spends a lot of tokens; a second run will still find more. If they only wanted the current diff checked, switch to `security-review`.

If the request is only "are we OWASP compliant?" and there is no change in front of you, load `security-threat-model` (checklist plus a short model), not an audit.

## 3. Load

Invoke the chosen skill by name. If the session has no Skill tool, `Read` the installed `SKILL.md` for that name (under `.grok/skills/`, `.claude/skills/`, or `.agents/skills/`) and follow it.

Say the sibling name and one sentence of intent before you start its steps.

## 4. Standing rules

These apply whichever sibling you load:

- Findings need a path, a condition, and a real impact. Skip style, hypothetical hardening, and "an attacker might" with no sink.
- Do not write exploit payloads, live attack steps, or copy-paste PoCs. Describe the flaw and the fix.
- Redact secrets in any quote. Replace tokens with `<REDACTED>`.
- Do not apply fixes from a review or audit unless the user asks after they have seen the list.
- Do not vendor scanner rules into the repo. If they need Semgrep, follow `security-scan` and use the public registry via the CLI.

## Reply before loading

```markdown
## Route
- Sibling: `<name>`
- Why this, not the next-cheapest:
- Scope: <diff / tree / tools / model>
```
