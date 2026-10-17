# Tool mapping

ork-skills are harness-neutral. This page is the one place that names vendor tools. If a skill says "spawn a subagent" or "ask the user" and your session uses different words, use the row below.

## Asking the user

| Skill phrase | Cursor | Claude Code | Codex | Grok Build |
| --- | --- | --- | --- | --- |
| Ask the user | `AskQuestion` if present, else a numbered question in the reply | `AskUserQuestion` if present, else the reply | A numbered question in the reply | A numbered question in the reply |

Do not invent a widget the schema rejects. A plain numbered list is always valid.

## Subagents

| Skill phrase | Cursor | Claude Code | Codex | Grok Build |
| --- | --- | --- | --- | --- |
| Spawn a subagent | `Task` | `Agent` (or `Task` on older builds) | The session's agent/spawn tool if it has one | The session's equivalent, or do the work inline |
| Read-only reviewer | `readonly: true` if the tool has it | An explore/read-only type if offered; otherwise tell the worker not to edit | Same idea | Same idea |
| Another model | Pass `model` when the tool accepts a slug you have | Pass `model` only if the schema lists it | Pass `model` only if listed | Pass `model` only if listed |

If spawn is missing, or a model slug is rejected, follow the skill's fallback (for `panel`, sequential personas). Do not retry a slug the schema just refused.

## Todo lists

| Skill phrase | Typical tool |
| --- | --- |
| Open a todo list | `TodoWrite` when the session has it |
| Mark `skip: <reason>` | Keep the item; put the skip text in its content or in the reply if the tool has no note field |

If there is no todo tool, keep the playbook steps as a numbered list in the reply and update it as you go.

## Invoking another ork skill

Prefer the session's Skill tool with the skill `name`. If that tool refuses a skill, `Read` the installed `SKILL.md` (under `.grok/skills/`, `.claude/skills/`, or `.agents/skills/` for Cursor and Codex) and follow it. When you tell the *user* to run a user-invoked skill, write `/<name>` or "run `<name>`".

## Git and pull requests

| Skill phrase | Tool |
| --- | --- |
| Inspect the tree | `git status`, `git diff`, `git log` |
| Open a pull request | `gh pr create` |
| Read a hosted pull request | `gh pr view` |

If `gh` is missing, write the body to a file and stop. Do not pretend a pull request exists.

## Browser and UI

When `verify` asks for a UI check, use the session's browser tool if it has one. If it does not, say **Not run** and name the path a person should click.

## Paths this set writes (target project)

These are in the project you are working on, not in the ork-skills clone:

- `docs/terms.md`
- `docs/decisions/*.md`
- `docs/plans/*.md`
- `docs/handoffs/*.md`
- `spikes/<slug>/` for a folder-based prototype
