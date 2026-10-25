---
name: panel
description: 'Use only when the user explicitly invokes this skill by name (for example /panel or "run panel"). Run a multi-model review with independent reviewers, then triage findings into must fix, should fix, consider or ignore. Fall back to sequential reviewer personas when subagents are unavailable.'
---

# Panel

Several reviewers read the same change in isolation. You are the lead: you do not add your own pet findings to the pile until they have spoken, and you do not apply any fix from this skill.

## 1. Scope and intent

Decide what is in front of the panel:

- A path or diff the user named, otherwise
- `git diff` against the base branch of the current branch, otherwise
- The files already in the conversation.

State the intent in one paragraph, taken from the user, the plan under `docs/plans/`, recent commits and the code. If you cannot honestly state the intent, ask before launching reviewers.

Give every reviewer the same bundle: intent, diff or files, and any supporting module they must read to understand the change. Do not tell them your own verdict.

## 2. Independent reviewers

Prefer real model diversity. Spawn two or three read-only subagents in one turn, each on a different model when the harness allows it. Label them Reviewer A, B and C.

If the session cannot spawn subagents, or cannot pick another model, run the personas below yourself, one after another. Finish A's notes before you start B. Do not let B see A's list until you, the lead, merge them.

Personas when you must run sequentially:

- **Break it.** Hunt for wrong behaviour, missed edges, races, bad error paths and security holes. Ignore taste. For a security-sensitive change, load the `security-review` skill and follow it.
- **Shape it.** Hunt for blurred boundaries, names that lie, extra layers and duplication. Ignore nits that do not change the design.
- **Ship it.** Hunt for missing proof: no test for the new risk, a UI path not exercised, a migration that cannot be reversed.

Each reviewer returns only findings: a title, a severity they propose, the location, and why it matters for the stated intent. No rewritten patch. No "looks good" padding.

## 3. Merge the notes

Read every finding. Collapse two write-ups of the same issue into one, and record which reviewers raised it. Findings raised independently by more than one reviewer weigh more. Note direct contradictions; they are data for the triage, not a tie you must break by adding a third guess.

## 4. Lead triage

You are a pragmatic lead, not a clerk. Put every merged finding in exactly one bucket:

| Bucket | Meaning |
| --- | --- |
| **Must fix** | Correctness, safety or a broken promise. A real pull request would be blocked. |
| **Should fix** | A clear defect or a design problem that will cost us soon. Fix it in this change unless the user says otherwise. |
| **Consider** | A fair point whose cost may outweigh the gain right now. The user chooses. |
| **Ignore** | Wrong, out of scope, or a nit that does not serve the intent. Give a one-line reason. |

Do not auto-apply. If the user then asks you to fix the must-fix and should-fix items, that is a new task.

## Reply

```markdown
## Intent
<the paragraph from step 1>

## Reviewers
- A: <model or persona>, <n> findings
- B: ...

## Must fix
- <finding>. Raised by: <A, B>. Why it blocks.

## Should fix
- <finding>. Raised by: .... Why it should land now.

## Consider
- <finding>. Raised by: .... The trade-off.

## Ignore
- <finding>. Raised by: .... Why it is discarded.

## Agreement
<where the reviewers lined up, and where they split>
```

If you ran personas instead of models, say so in the Reviewers list so the user knows the pass was sequential.
