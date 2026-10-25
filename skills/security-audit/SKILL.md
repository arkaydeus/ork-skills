---
name: security-audit
description: 'Use only when the user explicitly invokes this skill by name (for example /security-audit or "run security-audit"), or when the `security` router selected a full-repository audit. Map trust boundaries, hunt area by area, then validate each candidate with a fresh reviewer who tries to disprove it. Expensive in tokens; a diff review is security-review instead.'
license: MIT
---

# Security audit

A phased pass over the **tree**, not a pull-request review. It costs a lot of tokens. A second run against the same repo will still find things the first run missed. Say that before you start. If they only have a diff, stop and invoke `security-review`.

Do not apply fixes in this skill. Do not write exploit payloads. Do not dump raw scanner output; if you run tools, follow `security-scan` and treat hits as candidates.

## Cost and repeats

Tell the user, once:

- This maps the attack surface, hunts in slices, and re-reads every surviving finding.
- Budget is large (many files, several subagents, a second pass per candidate).
- Coverage is not "done" after one run. Later runs should target gaps from the coverage list, not replay the same files.

If they wanted a cheap check, switch. If they agree, continue.

## Phase 1 — Map

Build a short model of **this** repo. Read manifests, routing, auth middleware, CI, IaC, and the obvious entrypoints. Do not hunt yet.

Write down:

- **Assets.** Accounts, tenants, secrets, payments, admin, private data, signing keys, the CI identity.
- **Actors.** Anonymous, user, admin, another tenant, CI, a webhook sender, a dependency.
- **Trust boundaries.** Browser/server, service/service, job queue, file store, admin channel, CI.
- **Surfaces.** HTTP routes, RPC, GraphQL, HTML, webhooks, parsers, deserializers, upload paths, CLI flags, env, sockets, cloud roles.
- **Existing controls.** Authn, authz, CSRF, encoding, allow-lists, sandboxing, secret stores.

Group the tree into hunt **areas** (auth, injection, files, multi-tenant, jobs, CI, crypto, client, …). Drop areas the stack does not have (`skip: no native code`). Keep skipped areas on the coverage list.

Read `references/hunt-areas.md` next to this skill when you need prompts for an area. Do not paste that whole file into every hunter.

## Phase 2 — Hunt

Prefer read-only subagents, one area each, spawned in the same turn when the harness allows it. Give each hunter the map, their area, and instructions to return **candidates** only: title, location, hypothesized path, hypothesized impact. No patches. No "looks good".

If you cannot spawn, walk areas yourself, one after another. Finish A before B. Do not let later notes copy earlier certainty.

Hunters must:

- Ground every candidate in a path they opened.
- Record what they checked and what they skipped (that feeds coverage).
- Stay inside their area unless a sink they opened clearly belongs next door; then name it, do not silently expand.

Run cheap installed scanners in parallel if present (`security-scan` rules: triage, do not vendor rules). Scanner hits become candidates, not findings.

## Phase 3 — Disprove

Every distinct candidate goes to a **fresh** reviewer who did not find it. Spawn a new subagent when you can; otherwise switch persona and do not look at the hunter's certainty, only at the candidate and the source.

The reviewer's job is to **kill** the candidate:

- Is the input really attacker-controlled?
- Does a control on the path already stop it?
- Is the sink what they claimed?
- Is the impact real, or only a missing extra control?

Verdicts:

| Verdict | Meaning |
| --- | --- |
| **Confirmed** | Input, sink, missing blocker, and impact all hold in source you re-read. |
| **Unproven** | One exact fact is still missing (a default, a gateway, a production flag). No severity. |
| **Rejected** | Disproved. One line why. |

The hunter does not get to confirm their own item. Severity is only for Confirmed, and it needs impact, not "violates a checklist". A missing second control is not a finding when the first control holds; file it as hardening under Residual if it is still worth saying.

## Phase 4 — Report

Merge duplicates. Keep Rejected off the main list (a short count is enough). Confirmed findings use the same shape as `security-review`: title, severity, where, path, impact, fix, evidence (file and symbol). Unproven items list the missing fact only.

Coverage is a first-class output: every area, checked or skipped, with a reason. The next run starts from the gaps.

Write a file only if they asked for one. Default is the reply. Never commit secrets, tokens, or customer data. Redact quotes.

```markdown
## Scope
<tree, commit, what you excluded>

## Map
- Assets: …
- Actors: …
- Boundaries: …
- Surfaces: …

## Confirmed
### <title>
- Severity: …
- Where: …
- Path: …
- Impact: …
- Evidence: …
- Fix: …

## Unproven
- <candidate>. Missing fact: …

## Rejected
- <n> discarded. Notable: <one-liners if a near-miss teaches something>

## Coverage
| Area | Result |
| --- | --- |
| <area> | checked / skip: reason / gap: what a later run should open |

## Cost
- Subagents: <yes/sequential>
- Tools run: …
- Note: a later run should target Coverage gaps; it will not be empty.
```

## Honesty

Do not mark an area covered because you grepped one keyword. Do not upgrade Unproven to Confirmed to look finished. Do not start Phase 2 before the map exists. If a hunter returned a novel area, add it to Coverage rather than pretending it was in scope.
