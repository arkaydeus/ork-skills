---
name: security-review
description: 'Use when asked to security-review a diff, branch or pull request, or when a playbook wants exploitable issues in changed code. Report only high-confidence findings with severity and a fix. Prefer this over a full-repo audit. Do not start a multi-model panel unless the user asked for one.'
license: MIT
---

# Security review

One pass over **the change**, not the whole tree. Ship a short list of issues you would bet are exploitable. A long list of maybes is a failed review.

If they asked to cover every surface in the repository, stop and tell them to invoke `security-audit`. If they only wanted scanners run, invoke `security-scan`. If the subject is lockfiles or Actions, invoke `security-supply-chain`.

## 1. Scope

Take, in order:

1. Paths or a pull request they named.
2. The current branch against its base (`git diff <base>...HEAD`).
3. The files already in the conversation.

If the scope is empty, ask. Read enough context around each hunk to see the real sink: a helper two calls down, a middleware that already authenticates, a template that already escapes.

State the intent in two or three sentences: what the change claims to do, and which trust boundary it touches.

## 2. What to hunt

Walk attacker-controlled data from the new or changed entry to a sink. Typical sinks in a diff:

- query, command, template, or header construction
- file, URL, or redirect targets built from input
- authn, authz, session, or CSRF checks added, removed, or skipped
- deserialization, dynamic evaluation, or template engines
- crypto, randomness, password storage, cookie flags
- new endpoints, jobs, webhooks, or admin paths
- secrets, tokens, or PII newly logged, stored, or returned
- authorization that uses a client-supplied id without a server-side check

Read the surrounding module. A "safe" helper does not count if the diff bypasses it.

## 3. What not to report

Drop it unless you can show impact **in this change**:

- missing rate limits, generic DoS, or "could be slow"
- defense-in-depth extras when another layer already blocks the path
- style, lint, naming, or missing comments
- framework behaviour you have not checked
- issues only in untouched files (mention once under Residual, do not file)
- scanner hits you have not opened and confirmed (if you ran tools, follow `security-scan` triage rules)

If you are unsure the input reaches the sink, it is not a finding. Put a one-liner under Residual or omit it.

## 4. Confidence gate

A finding is reportable only when all of these are true:

1. **Input.** Something the attacker can set (request, webhook, file, job payload, untrusted CI event) reaches this code.
2. **Sink.** That value hits a dangerous operation, or a check that used to stop them is gone.
3. **Impact.** You can name the result: read another tenant, run a query, steal a session, overwrite an object, leak a secret.
4. **No blocker.** You looked for the middleware, policy, encoder, or allow-list that would stop it, and it does not apply here.

Confidence:

| Label | Meaning |
| --- | --- |
| **High** | You traced input to sink in the diff and saw no blocker. |
| **Medium** | The path is plausible; one fact (a filter you could not read, a config default) is unverified. |
| **Low** | Hypothesis. Do not list it as a finding. Residual at most. |

Do not file Low. Medium needs the missing fact named.

## 5. Severity

Rate the **exploitable result**, not the CWE number.

| Severity | Use when |
| --- | --- |
| **Critical** | Unauthenticated or cross-tenant take-over, RCE, secret theft that yields production access. |
| **High** | Auth bypass on a sensitive action, injection with a real sink, stored XSS in an authenticated app, IDOR on private data. |
| **Medium** | Limited injection, CSRF on a state-changing action, missing auth on a low-value endpoint, secret in a non-prod path. |
| **Low** | Hardening that still matters (cookie flag on a new session cookie, verbose error that leaks a path). Skip if it is taste. |

If impact is "attacker spends CPU", it is not a finding in this skill.

## 6. How to write a finding

Each issue is one block. No combined "auth is messy" notes.

- **Title** — what goes wrong, in one line
- **Severity** and **confidence**
- **Where** — path, symbol, and the changed line or hunk
- **Path** — how untrusted input reaches the sink, in this change
- **Impact** — what the attacker gets
- **Fix** — a concrete change (check, encoder, deny-by-default, drop the field). Not "sanitize input".
- **Why not a false positive** — the blocker you looked for and did not find

Do not attach a working exploit, a curl that pops a shell, or a payload string meant to be replayed. The path and the impact are enough.

## Reply

```markdown
## Intent
<what this change is for, and the boundary it crosses>

## Findings
### <title>
- Severity: …
- Confidence: high | medium
- Where: `path` / symbol
- Path: …
- Impact: …
- Fix: …
- Why not a false positive: …

## Residual
- <out of scope, unverified, or pre-existing; one line each, or "none">

## Checks
<commands you ran, or "none">
```

If there are no reportable findings, say that plainly. Do not pad with nits.

Do not apply edits unless the user asks after they have seen the list. If they want independent models to argue, that is `panel`; for a security-sensitive change, `panel`'s break-it reviewer should still follow this skill.
