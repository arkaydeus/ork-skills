---
name: security-threat-model
description: 'Use when asked for a threat model, STRIDE analysis, trust boundaries, assets and actors, or an OWASP / ASVS-style security checklist for a system. Produce a lightweight model and mitigations. Not a full-repo vulnerability hunt (that is security-audit) and not a diff review (that is security-review).'
license: MIT
---

# Threat model

A short model of **what you are protecting and from whom**. It is a document, not a hunt. If they asked "find vulns in this tree", invoke `security-audit`. If they asked about a patch, invoke `security-review`.

Read `references/owasp-asvs-checklist.md` next to this skill after the diagram exists, and walk the rows that apply. The checklist is in our own words; it is not the OWASP standard and it does not quote ASVS IDs.

## 1. Scope

Name the system in one paragraph: repo or service, who it serves, what "done" means for this model. If the tree is a monorepo, pick the component they named or ask which one.

Skim routing, auth, data stores, CI, and any existing `SECURITY.md` or `docs/` architecture notes. Facts from the repo beat guesses. Label a guess as a guess.

## 2. Assets

List what an attacker would want. Be concrete to this product:

- account and session identity
- tenant data, files, messages, payment records
- secrets: signing keys, API tokens, encryption keys, CI identities
- admin and support powers
- availability of a paid or safety-critical path
- the integrity of releases (store listing, package, image)

Drop assets you cannot point at in this system.

## 3. Actors

Who sits outside or beside the app:

- anonymous internet
- signed-in user
- another tenant
- staff / admin / support
- CI and deploy roles
- third-party webhooks and OAuth clients
- a compromised dependency or build worker

For each, say what they are **already allowed** to do. A threat is only interesting when they exceed that.

## 4. Trust boundaries

Draw the crossings in words (or a simple list). Typical:

- browser or app ↔ your HTTP API
- your API ↔ database / object store / queue
- your worker ↔ the API that enqueued the job
- you ↔ identity provider, payment, email, object preview
- developer laptop ↔ CI ↔ registry
- tenant A data ↔ tenant B data (logical boundary)

Anything that crosses a boundary is in scope for STRIDE. Internal function calls that never leave a process are usually not.

## 5. Threats (STRIDE-style)

For each boundary, ask the six questions. Skip a column that cannot apply (pure static site, no identity, …) and say you skipped it.

| Class | Question |
| --- | --- |
| **Spoof** | Can someone pretend to be a user, service, or admin? |
| **Tamper** | Can they change data, code, or a job in transit or at rest? |
| **Repudiate** | Can they deny an action because you cannot tell who did it? |
| **Disclose** | Can they read another tenant, a secret, or a token? |
| **Deny service** | Can they take down a shared resource in a way that is unique to this design (not generic flooding)? |
| **Elevate** | Can they gain a role, tenant, or function they were not issued? |

Write threats as **actor + action + asset**, with a condition. "User uploads a file" is not a threat. "User A fetches user B's object URL because the store is world-readable" is.

Do not invent a threat you cannot tie to a boundary you listed. Do not turn every OWASP row into a threat if this system has no such surface.

## 6. Mitigations

For each threat, name what **already** exists in the code or config, then the gap.

| Status | Meaning |
| --- | --- |
| **Covered** | A control you opened in source or config. |
| **Partial** | A control exists; a bypass or missing case is visible. |
| **Missing** | No control at this boundary. |
| **Accept** | Residual they may keep; say so plainly. |

Recommend a mitigation that matches the threat (server-side authz, signed URLs, tenant in the query, pin the action SHA). Do not prescribe a product by brand unless the repo already uses it.

## 7. Checklist pass

Walk `references/owasp-asvs-checklist.md`. For each applicable row: **ok**, **gap**, or **n/a**. Gaps that are already threats should link, not duplicate. Rows that do not apply stay `n/a` with a three-word reason.

## Reply

```markdown
## System
<paragraph>

## Assets
- …

## Actors
- … (allowed: …)

## Boundaries
- …

## Threats
| ID | STRIDE | Actor | Threat | Asset | Mitigation | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | Elevate | other tenant | … | … | … | missing |

## Checklist
| ID | Topic | Result |
| --- | --- | --- |
| C1 | … | ok / gap / n/a — … |

## Residual
- <what you did not model, or accepted>
```

Do not start implementing mitigations unless they ask after they have the model. Do not claim the system is "OWASP compliant"; the checklist is a lens, not a certificate.
