# Application checklist

Original notes grouped the way many teams already think about web and API risk. Names follow well-known public themes (access, config, supply chain, crypto, injection, design, authentication, integrity, logging, failures). This is not the OWASP Top 10 text, not ASVS, and not a certification.

Mark each applicable row `ok`, `gap`, or `n/a` in the threat-model reply.

## Access

- [ ] Every object read or write is authorized on the **server** using the session's identity and tenant, not a client-supplied role.
- [ ] Listing, export, download, share, and "get by id" use the same check as the UI.
- [ ] Admin routes are deny-by-default and not reachable by swapping a URL or a verb.
- [ ] Cross-tenant identifiers in the request cannot select another tenant's row.

## Configuration

- [ ] Debug, directory listing, default passwords, and sample users are off in the deployed config.
- [ ] Security-relevant headers (cookie flags, CORS, framing) are set in one known place, not accidentally by a reverse proxy you did not look at.
- [ ] CORS does not reflect arbitrary origins with credentials.
- [ ] Cloud stores, queues, and admin ports are not world-open unless that is the product.

## Supply chain

- [ ] Production installs from a lockfile (or an equivalent pinned graph).
- [ ] CI does not execute untrusted workflow input or fork code with privileged secrets.
- [ ] Release artifacts are built from a known commit, not a developer laptop copy.
- [ ] Registry scope for private packages is configured so a public name cannot shadow it.

## Cryptography

- [ ] TLS for data in motion; no home-rolled transport encryption.
- [ ] Passwords use a slow hash designed for passwords, not a general digest.
- [ ] Tokens and keys come from a CSPRNG; not `Math.random` or `time`.
- [ ] Secrets live in the environment or a secret store, not in source.

## Injection

- [ ] Queries go through bound parameters or the ORM's safe API; no string-built SQL.
- [ ] OS commands are fixed binaries plus an allow-listed argument list, or not used.
- [ ] HTML and JavaScript are encoded at the boundary; Markdown/SVG/PDF treated as active.
- [ ] Template and expression engines do not evaluate user strings.
- [ ] Uploaded files cannot become executable on the server or be served with a dangerous type.

## Design

- [ ] Sensitive actions have an explicit state machine (who can start, who can finish).
- [ ] "Optional" auth on a route is documented; default is authenticated.
- [ ] Dangerous operations (delete, pay, change owner) need a fresh intent (password, email, or similar), not only a session cookie on GET.
- [ ] Limits exist where a cheap loop would destroy another tenant's data, not only where it would be slow.

## Authentication and session

- [ ] Login, reset, and invite tokens expire, are single-use, and are not in server logs.
- [ ] Sessions rotate on login and privilege change; logout invalidates the server side.
- [ ] Cookies that carry a session are `Secure`, `HttpOnly`, and a strict `SameSite` unless a documented exception exists.
- [ ] MFA, if claimed, cannot be skipped by hitting an older endpoint.
- [ ] Self-contained tokens (JWT and friends) have audience, issuer, algorithm allow-list, and expiry checked.

## Integrity

- [ ] Unsigned or unverified payloads are not deserialized into code paths (pickles, object streams, YAML `load`).
- [ ] Webhooks verify a signature over the raw body.
- [ ] Updates (mobile, desktop, container) authenticate the publisher.

## Logging and errors

- [ ] Auth failures, access denials, and admin actions are logged without secrets or passwords.
- [ ] Someone would notice a burst of denials (even if that someone is an on-call doc, not a product).
- [ ] User-facing errors do not include stack traces, connection strings, or internal paths.

## Failures

- [ ] A thrown error does not skip an authorization check that was supposed to run next.
- [ ] Timeouts, retries, and partial writes cannot complete a payment or grant twice.
- [ ] "Fail open" is not the default on authz, TLS verification, or webhook signatures.

## If the product talks to a model

- [ ] Model output is never `eval`'d, piped to a shell, or used as a SQL string.
- [ ] Tools the model may call have the **user's** authorization, not the server's.
- [ ] Retrieved documents and prompt fragments are treated as untrusted text.

Skip any section whose surface is absent (`n/a — no sessions`, `n/a — no LLM`).
