---
name: ork
description: 'Use only when the user explicitly invokes this skill by name (for example /ork or "run ork"). Classifies a software task, picks one short playbook, turns its steps into a todo list, and records any skipped step with a reason.'
---

# Ork

You are the session lead. Classify the work, load one playbook, and drive it to a finish the user can inspect. Do not start coding, reviewing or opening a pull request until the playbook says so.

Read `docs/tool-mapping.md` in the ork-skills repository if a tool name in this set does not match the current harness. Prefer the harness-neutral words in that file.

## 1. Classify

Read the request, the open files and `docs/terms.md` if it exists. Pick exactly one playbook:

| Playbook | Choose it when |
| --- | --- |
| `feature` | New or changed product behaviour, an API, or a user-visible capability |
| `bug` | Something already shipped is wrong: a crash, a bad result, a regression |
| `refactor` | The behaviour must stay the same while the structure changes |
| `prototype` | A throwaway spike to answer a question, not a change meant to merge |
| `perf` | The complaint is speed, memory, throughput or cost, and it needs numbers |
| `review` | The user wants a critique of existing work, not new implementation |

If two playbooks could fit, pick the one that matches the *deliverable* (a fix, a spike, a verdict) and say why. If the request is only a question about existing code, stop and tell the user to invoke `explain`. If they asked only to record vocabulary, stop and tell them to invoke `terms`.

Say the playbook name and one sentence of intent before you open the file.

## 2. Load the playbook

Read `playbooks/<name>.md` next to this skill (`feature`, `bug`, `refactor`, `prototype`, `perf` or `review`). Use that file as the script for the rest of the session.

## 3. Open the todo list

Copy every numbered step from the playbook into the session todo list, in order, wording intact. Add task-specific todos only *after* those steps.

A step you will not do stays on the list. Mark it done with a `skip:` reason on the same line, for example `skip: no user-visible surface to check`. Never delete a skipped step. If you later do it, replace the skip note with what you did.

## 4. Run the steps

Work the list in order. When a step names another ork skill, invoke that skill if it is installed and follow it in full. If the skill is missing, follow the step as written here and in the playbook, and say that the named skill was not loaded.

Keep these standing rules while you run any playbook:

- Apply `principles` the whole time: no slop, and ask rather than guess.
- Use the words in `docs/terms.md`. If a new term is settled, update that file in the same sitting.
- Look up facts (files, commands, git history, failing tests) yourself. Ask the user only for preference, product calls, or access you cannot obtain.
- Do not declare work done without the evidence the playbook's verify step asks for.
- Do not merge a pull request unless the user asked you to merge.

## 5. When no playbook quite fits

Stay on the closest playbook and add extra todos beneath its steps. Do not invent a second parallel script. If the work is a long pause or a change of owner, invoke `handoff` at the end.

## Reply shape

Close with a short lead report:

1. Playbook you ran.
2. What changed, or why nothing changed.
3. Skipped steps and their reasons.
4. Paths of any files written under `docs/`.
5. The single next action, if any remains.
