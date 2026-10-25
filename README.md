# ork-skills

Ork's software engineering skills for Claude Code, Codex, Cursor, Grok Build and other agents that read the Agent Skills format.

The set is twenty-five skills. Ten run only when you name them. Fifteen fire from their descriptions when the work matches. They share a small set of artefacts in the *target* project, not in this repository: `docs/decisions/`, `docs/plans/`, `docs/handoffs/` and `docs/terms.md`.

There is no install script and no per-tool plugin tree. Install with the [skills CLI](https://github.com/vercel-labs/skills). Tool names differ by harness; see [docs/tool-mapping.md](docs/tool-mapping.md).

## Skills

| Name | Trigger | One-liner |
| --- | --- | --- |
| `ork` | User | Classify the task, pick a playbook, drive the todo list |
| `shape` | User | Interview in rounds; record decisions and terms |
| `plan` | User | Write a plan and wait for sign-off before code |
| `panel` | User | Multi-model review, then a lead triage |
| `handoff` | User | Leave a note another session can pick up |
| `lesson` | User | After a correction, add a durable guard |
| `explain` | User | Read-only how (flow) and why (history) |
| `prototype` | User | Throwaway spike, labelled not for merge |
| `unslop` | User | Strip AI tells from a draft, or flag them only |
| `debug` | Auto | Repro or failing test first, then cause, then fix |
| `bun-testing` | Auto | Deterministic bun:test suites and isolation bugs |
| `review` | Auto | One pass on correctness and design |
| `pr` | Auto | Branch, commits, PR body, open with `gh` |
| `terms` | Auto | Keep `docs/terms.md` and use those words |
| `verify` | Auto | Tests, types, lint, UI; evidence not claims |
| `refactor` | Auto | Behaviour-preserving, green, small steps |
| `perf` | Auto | Baseline, bottleneck, fix, re-measure |
| `principles` | Auto | No slop; ask, do not guess |
| `security` | Auto | Route a security request to the cheapest sibling |
| `security-review` | Auto | High-confidence issues in a diff or PR |
| `security-audit` | User | Full-repo audit in phases, then adversarial check |
| `security-scan` | Auto | Run installed scanners; triage, do not dump |
| `security-supply-chain` | Auto | Lockfiles, typosquats, install scripts, Actions |
| `security-threat-model` | Auto | Assets, actors, boundaries, STRIDE, checklist |
| `website-audit` | Auto | Live-URL pass/warn/fail launch and SEO report with fixes |

User-invoked skills say so in their `description` and should not be started unless you named them. Auto skills carry the when-to-use text the agent matches against.

`ork` playbooks live in [`skills/ork/playbooks/`](skills/ork/playbooks/): feature, bug, refactor, prototype, perf and review.

## Install

From the project you want the skills in:

```bash
npx skills add arkaydeus/ork-skills
```

To pin the four named agents:

```bash
npx skills add arkaydeus/ork-skills -a grok -a claude-code -a codex -a cursor
```

Security skills only:

```bash
npx skills add arkaydeus/ork-skills --skill security
```

Add each sibling the same way (`--skill security-review`, `security-audit`, `security-scan`, `security-supply-chain`, `security-threat-model`) or install the whole set as above.

`unslop`, `bun-testing`, or `website-audit` only:

```bash
npx skills add arkaydeus/ork-skills --skill unslop
npx skills add arkaydeus/ork-skills --skill bun-testing
npx skills add arkaydeus/ork-skills --skill website-audit
```

Add `-g` to install for your user rather than the current repo. Use skills CLI 1.7.2 or later if you need `-a grok`. The CLI copies or links each `skills/<name>/` folder into the agent directories it knows about: `.grok/skills/` for Grok Build, `.claude/skills/` for Claude Code, and `.agents/skills/` for Cursor and Codex.

Then invoke a user skill by name (`/ork`, "run shape") or let an auto skill match the work.

## Artefacts (in the target project)

| Path | Who writes it |
| --- | --- |
| `docs/terms.md` | `terms`, and `shape` when a word is settled |
| `docs/decisions/` | `shape`, sometimes `prototype` or `perf` |
| `docs/plans/` | `plan` |
| `docs/handoffs/` | `handoff` |

These folders are created when there is something to put in them. They are not part of this repository.

## Inspired by

The methods are a rewrite in our own words, not a copy of another skill file, except for `skills/unslop/references/patterns.md` (and the Adding soul passage), which adapt pstack unslop under MIT.

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) (MIT, © 2026 Lauren Tan), in particular playbooks as a todo list with skipped steps recorded, multi-model review with a lead verdict, durable guards after a correction, how/why explanation, verification against the real artefact, and the unslop pattern list.
- [Matt Pocock's skills](https://github.com/mattpocock/skills) (MIT, © 2026 Matt Pocock), in particular round-based interviews with a recommended answer, a shared glossary, a two-axis review, a repro-first debug loop, and a harness-neutral `SKILL.md` layout.

See [NOTICE.md](NOTICE.md) for the mapping. Both projects are MIT-licensed; this set is a new work except for that pstack unslop adaptation. Security skills credit further sources there (ideas only; no copied text or scanner rules). `unslop` also credits [theclaymethod/unslop](https://github.com/theclaymethod/unslop) as inspiration only.

## Licence

MIT, © 2026 Ark. See [LICENSE](LICENSE).
