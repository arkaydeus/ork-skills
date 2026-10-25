# Hunt areas

Load one section per hunter. Skip any area the map says this repo does not have. These are prompts, not a claim that every bullet is a finding.

## Access control

Object identifiers in the request, missing checks on new routes, role tests that use client-supplied roles, admin actions reachable as a user, cross-tenant reads and writes, IDOR on export/download/share, mass assignment of privilege fields, insecure direct file or document references.

## Injection and parsers

SQL, NoSQL, and ORM raw strings; shell, template, LDAP, XPath; header or log injection; XML external entities; CSV/formula injection on export; user-controlled regex; deserialization of untrusted bytes; eval, `Function`, `pickle`, `yaml.load`, native `system`.

## Authn and session

Login, reset, invite, and MFA bypasses; user enumeration; session fixation; cookie flags and scope; "remember me"; JWT `alg`, `kid`, and audience; API keys in URLs; password storage; logout that does not kill the server session.

## Web and CSRF

State-changing GET; missing anti-CSRF on cookie sessions; CORS reflecting origin with credentials; postMessage without origin checks; open redirects that mint tokens; mixed content; clickjacking on sensitive flows only when a real action is framed.

## Files and URLs

Upload type and path; zip/path traversal; SSRF from URL fetchers, previews, webhooks, or importers; file:// and link-local targets; image/pdf processors; download endpoints that take a server path.

## Data isolation and lifecycle

Tenant column missing on a query; search indexes that leak; cache keys without tenant; export, backup, and support tools; delete that does not delete copies; restore that restores another tenant.

## Jobs, webhooks, RPC

Unverified webhook signatures; replay; confused-deputy callbacks; queue payloads trusted as internal; gRPC/tRPC methods without the HTTP auth wrapper; id-only authorization on a worker.

## Crypto and secrets

Home-rolled crypto; static IVs; `math/rand` for tokens; secrets in source, images, or logs; debug endpoints; error messages that dump stack or connection strings.

## Client and mobile

DOM XSS, prototype pollution, deep links, exported activities, WebView JavaScript bridges, local IPC with no auth, stored tokens in world-readable storage.

## Native and memory

Unchecked lengths, integer wrap, use-after-free at a boundary, unsafe FFI, path canonicalization on case-insensitive disks, setuid helpers.

## Cloud and config

Public buckets, `0.0.0.0` admin, default passwords, debug true in production config, overly broad IAM, unauthenticated management ports, world-writable install paths.

## Supply and CI

Treat as a pointer: invoke `security-supply-chain` for a dedicated pass, or hunt lockfiles, install scripts, and workflow trust here if that skill is not loaded.
