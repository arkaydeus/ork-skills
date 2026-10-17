# Notice

ork-skills is original text, written for this repository. It is not a fork and does not copy skill bodies from the works below. Those works are MIT-licensed and we thank their authors. If you use ork-skills, you must also respect their licences for any of *their* files you take yourself.

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

## What we left out

We did not add a TDD skill. We did not add a "small diffs" principle. We did not take pstack's Cursor-only setup, model tables, or generated harness trees. We did not take Matt's issue-tracker setup, ticket graph or plugin manifests.

## Our licence

ork-skills is MIT, © 2026 Ark. See `LICENSE`.
