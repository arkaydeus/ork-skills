# Notice

ork-skills is original text, written for this repository. It is not a fork and does not copy skill bodies from the works below, except for the pstack unslop adaptation named in the exception in the pstack section. If you use ork-skills, you must also respect their licences for any of *their* files you take yourself. The engineering skills drew on MIT works. The security skills drew on a mix of MIT, Apache-2.0, CC-BY-SA-4.0 (ideas only), and the Semgrep Rules License (no rules or text); each is listed in its own section.

## pstack

- Project: [pstack](https://github.com/cursor/plugins/tree/main/pstack) in [cursor/plugins](https://github.com/cursor/plugins)
- Licence: MIT
- Copyright: © 2026 Lauren Tan

Ideas we rewrote, rather than files we copied:

| ork skill | Idea |
| --- | --- |
| `ork` and `skills/ork/playbooks/` | A short playbook copied into a todo list; skipped steps stay visible with a reason |
| `panel` | Independent reviewers, then a lead who buckets findings; nothing auto-applied |
| `lesson` | After a correction, encode the lesson as a guard, not another reminder |
| `explain` | How from the running flow; why from recorded history |
| `verify` | Check the real artefact; a compile is not proof |
| `perf` | Measure before you change; report the same measurement after |
| `principles` (no slop) | Cut filler comments, vague names and extra machinery |

### Exception: adapted unslop text

`skills/unslop/references/patterns.md` (and any adapted unslop text in `skills/unslop/SKILL.md`, including the Adding soul section) contains text adapted from [pstack unslop](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md) (MIT, © 2026 Lauren Tan). That adapted text is kept under the notice below, not as a silent copy.

MIT License

Copyright (c) 2026 Lauren Tan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## theclaymethod/unslop

- Project: [theclaymethod/unslop](https://github.com/theclaymethod/unslop)
- Licence: none published (GitHub `license` metadata is null)

**Inspiration only. No text.** The flag-only (audit) mode, the fact-preservation guards, and extra tells 34–41 in `skills/unslop/references/patterns.md` were rewritten in our own words from ideas in that repository. We did not copy or paraphrase its files.

## Skills moved from arkaydeus/skills

`bun-testing` and `website-audit` are Ark's own MIT work, moved unchanged from [arkaydeus/skills](https://github.com/arkaydeus/skills). `unslop` is the merged skill described above (pstack text kept with credit; claymethod ideas rewritten).

`website-audit` credits [Florence (@flxrnc)](https://x.com/flxrnc) and [damkin (@damkina7)](https://x.com/damkina7).

## Matt Pocock's skills

- Project: [mattpocock/skills](https://github.com/mattpocock/skills)
- Licence: MIT
- Copyright: © 2026 Matt Pocock

Ideas we rewrote:

| ork skill | Idea |
| --- | --- |
| `shape` | One round of numbered questions at a time, each with a recommended answer; look up facts yourself |
| `terms` | A living glossary of canonical words |
| `shape` / `plan` artefacts | Decisions and plans as files the next session can read |
| `debug` | A tight repro or failing test before a theory |
| `review` | A single pass on correctness and on design |
| `handoff` | A compact note for the next agent |
| `prototype` | Throwaway work that answers one question |
| layout and install | `skills/<name>/SKILL.md` plus `npx skills add`, no per-tool forks |

Matt's `pr` skill credits Dex Horthy (HumanLayer) for the "show the work" habit. Our `pr` skill keeps a short What / Why / How to test body and links our own artefact paths; it is not that template.

## Security skills

These six skills (`security`, `security-review`, `security-audit`, `security-scan`, `security-supply-chain`, `security-threat-model`) are original MIT text. We read other public skill collections for **ideas** (phased audits, cheap-first routing, high-confidence diff review, scanner triage, supply-chain and Actions hardening, lightweight STRIDE models). We did not copy their files, wording, prompts, schemas, or scanner rules.

Licences below were checked against each GitHub repository (`license` metadata and the `LICENSE` / `LICENSE.txt` file) on 2026-10-25.

### cloudflare/security-audit-skill

- Project: [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill)
- Licence: MIT
- Copyright: © 2025-2026 Cloudflare, Inc.

Ideas we rewrote: a full-tree audit in phases (map, hunt by area, then a second agent tries to disprove each candidate); structured findings plus a coverage list; a warning that one run is costly and a later run still finds more.

### anthropics/claude-code-security-review

- Project: [anthropics/claude-code-security-review](https://github.com/anthropics/claude-code-security-review)
- Licence: MIT
- Copyright: © 2025 Anthropic

Ideas we rewrote: review the diff rather than the whole tree; keep false positives low; attach severity and a fix to each issue.

### agamm/claude-code-owasp

- Project: [agamm/claude-code-owasp](https://github.com/agamm/claude-code-owasp)
- Licence: MIT
- Copyright: © 2026 (as stated in that repository's `LICENSE`)

Ideas we rewrote: a short application checklist grouped by common risk themes, kept out of `SKILL.md` in `references/`, written in our own words rather than as a copy of OWASP or ASVS.

### openai/codex-security

- Project: [openai/codex-security](https://github.com/openai/codex-security)
- Licence: Apache-2.0

Ideas we rewrote: treat validation of a candidate as a separate step from finding it; record coverage so a later pass has somewhere to start.

### openai/skills

- Project: [openai/skills](https://github.com/openai/skills) (`security-threat-model`, `security-best-practices`)
- Licence: Apache-2.0 (per-skill `LICENSE.txt`; the repository has no root licence file)

Ideas we rewrote: a compact threat model (assets, actors, boundaries, STRIDE-style questions, mitigations) and a best-practice lens as a checklist, not a vulnerability dump.

### getsentry/skills

- Project: [getsentry/skills](https://github.com/getsentry/skills) (`security-review`)
- Licence: Apache-2.0
- Copyright: © 2025 Functional Software, Inc. dba Sentry

Ideas we rewrote: a diff-scoped security pass that prefers exploitable bugs over nits.

### trailofbits/skills

- Project: [trailofbits/skills](https://github.com/trailofbits/skills)
- Licence: CC-BY-SA-4.0

**Ideas only.** We did not copy or paraphrase that repository's skill text. ShareAlike would apply to a derivative of their wording; our security skills are a new MIT work. Ideas we took at the topic level: hunt from entry points, check candidates for false positives, and treat supply-chain and CI trust as its own review.

### semgrep/skills

- Project: [semgrep/skills](https://github.com/semgrep/skills)
- Licence: [Semgrep Rules License v1.0](https://semgrep.dev/legal/rules-license) (GitHub `license` key `other` / `NOASSERTION`; `LICENSE` file points at that URL)

**No rules and no text.** We do not vendor Semgrep rules, query packs, or skill prose from that repository. `security-scan` only tells the agent to invoke an already-installed Semgrep CLI against the public registry (for example `semgrep scan --config auto`) and to triage the output.

## What we left out

We did not add a TDD skill. We did not add a "small diffs" principle. We did not take pstack's Cursor-only setup, model tables, or generated harness trees. We did not take Matt's issue-tracker setup, ticket graph or plugin manifests.

## Our licence

ork-skills is MIT, © 2026 Ark. See `LICENSE`.
