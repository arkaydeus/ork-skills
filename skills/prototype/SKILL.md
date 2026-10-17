---
name: prototype
description: 'Use only when the user explicitly invokes this skill by name (for example /prototype or "run prototype"). Build a throwaway spike on its own branch or folder, clearly labelled not for merge, and record what was learned.'
---

# Prototype

A spike exists to answer a question cheaply. It is not a first draft of the feature. Keep it isolated, keep it labelled, and write down what it taught you.

## 1. Name the question

Write one question the spike must answer, for example "can we stream this without holding the whole file?" or "which of these two layouts can a user finish in fewer clicks?". If you cannot name the question, stop and ask. A spike with no question is a messy feature.

Look up anything the tree already proves. Do not spike a fact you can read off a test or a vendor doc.

## 2. Isolate it

Pick one home and stick to it:

- a branch `spike/<slug>`, or
- a folder `spikes/<slug>/` that the main build does not import

Do not mix spike files into the production tree "just for now". If the harness can give you a worktree, use it.

Label the work so a later agent will not merge it by habit:

- put `NOT FOR MERGE` in the branch name commit messages, or in a `spikes/<slug>/README.md`
- first line of that README: the question, the date, and the words `not for merge`

## 3. Build the smallest thing that can answer

Cut every concern that does not serve the question. Fake the rest. Hard-code data. Leave tests out unless the question *is* about a test harness.

Time-box your own ambition. If the spike is growing a real domain model, you have slipped into a feature. Stop, write what you know, and say so.

## 4. Read the result

Run the spike. Quote the output, screenshot or measurement that answers the question. If the answer is "no" or "not like this", that is success.

## 5. Record the learning

Write a short note the next session can use. Prefer `docs/decisions/YYYY-MM-DD-<slug>.md` when a real choice follows (status `accepted` only if the user agrees; otherwise `proposed`). If nothing was decided, keep the learning in the spike README and in your reply.

The note should say:

- the question
- what you tried
- what you observed
- what you would *not* repeat
- what a production pass would still have to do

Do not copy spike code into `src/` as part of this skill. A later `plan` or feature playbook can take the idea, not the files.

## 6. Leave it closed

Do not open a pull request into the default branch. Do not delete the spike unless the user asks. If another person will continue, invoke `handoff` and point at the spike path.

## Reply

- The question.
- Where the spike lives.
- The answer, with evidence.
- What must not be merged.
