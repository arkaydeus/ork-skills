# CI hardening notes

Read from `security-supply-chain` when reviewing workflow YAML. Original notes, not a copy of any vendor guide.

## Events and trust

| Event | Code that runs | Secrets |
| --- | --- | --- |
| `pull_request` from a fork (public) | The fork's tree if you check it out | Usually **not** repo secrets |
| `pull_request_target` | Default checkout is the **base** ref; easy to accidentally check out the PR head | **Yes**, base secrets |
| `workflow_run` | Your workflow, after another workflow; `head_sha` is still untrusted input | Yes |
| `issue_comment` / `pull_request_review` | Your workflow; **body and title are untrusted** | Yes |

Untrusted strings: PR title, body, label, branch name, commit message, review text, issue form fields, `workflow_dispatch` inputs from people you do not trust.

## Shell interpolation

Dangerous:

```yaml
run: echo ${{ github.event.pull_request.title }}
```

Safer patterns: pass values through `env:` and expand as quoted `"$VAR"` in the shell, or use a language step that does not invoke the shell. Never build a script from event JSON with unquoted substitution.

## Pins

- Actions: `uses: owner/action@<40-char-sha>` and a comment with the tag you intended.
- Container jobs: digest (`image@sha256:…`) rather than `latest`.
- Reusable workflows: SHA, not `@main`.

Resolve the SHA from the tag at review time; do not invent one.

## Permissions

Set a default at workflow level:

```yaml
permissions:
  contents: read
```

Add job-level writes only where needed (`pull-requests: write` for a comment job, `id-token: write` for OIDC). Avoid `contents: write` on jobs that execute untrusted trees.

## Checkouts

On privileged events, do not:

```yaml
- uses: actions/checkout@...
  with:
    ref: ${{ github.event.pull_request.head.sha }}
```

then run package managers or tests from that tree. If you must comment on a PR, use the API from trusted code and do not execute the fork.

## Secrets

- Prefer OIDC to cloud roles over long-lived access keys.
- Narrow each secret to the job that needs it.
- Do not pass secrets to `pull_request` workflows that run fork code "just in case"; they should not need them.
- Redact; never `echo` a secret to debug.

## Self-hosted runners

A public repo plus a self-hosted runner plus fork pull requests is a lateral-movement path onto that machine. Do not attach extra runners to public fork PRs.
