---
name: shape
description: 'Use only when the user explicitly invokes this skill by name (for example /shape or "run shape"). Interview in rounds of numbered questions, each with a recommended answer, look up facts yourself, then record decisions in docs/decisions/ and update docs/terms.md.'
---

# Shape

Turn a fuzzy request into settled decisions. You interview; you do not implement. Stop when the remaining work is execution, not choice.

## Before the first question

Read `docs/terms.md` and any files already under `docs/decisions/` and `docs/plans/`. Skim the code and git history that bear on the request. Facts you can observe (what exists, how it behaves, what a command prints) are your job. Do not ask the user to recall a filename, a test command or a stack trace you can obtain.

Write down, privately, every choice that would change the design, and which choices depend on others. A question is *ready* when every choice it depends on is already settled.

## One round at a time

Ask every ready question in a single message. Do not leak questions that depend on answers you have not yet heard.

Number the questions from 1. Each item has a short title, the question, and your recommended answer. Word the question so that "yes" accepts the recommendation.

```markdown
1. **<title>**
   <the question, including options when there are a few>
   Recommended: <your answer, with the reason in one sentence>
   Reply yes to take the recommendation, or say what you want instead.
```

Wait for the user's answers before the next round. Do not keep talking, and do not start coding in the gap.

After each reply, mark decisions settled, see which questions are now ready, and ask the next round. If a question in this round still hangs on another unanswered item in the same round, it was asked too early: withdraw it and save it for later.

## What not to ask

- Anything a search, a test run, a file read or `git log` can answer.
- Questions whose only purpose is to make the user feel consulted.
- Implementation trivia (library X vs library Y) before the product choice that would make that trivia matter.

If you need a fact while a round is open, look it up in parallel. Only the questions that depend on that fact wait. Ask the rest now.

## When a term is settled

If the user names a domain idea, or you need a word to talk precisely, add or edit it in `docs/terms.md` in the same sitting. Follow the `terms` skill. Use the canonical word from then on, including in later questions.

## When a decision is settled

Write a file under `docs/decisions/` as soon as the choice is real, not at the end of the interview. Use `docs/decisions/YYYY-MM-DD-<slug>.md`:

```markdown
# <Decision title>

- Date: YYYY-MM-DD
- Status: accepted
- Related terms: <names from docs/terms.md, if any>

## Context

<the problem and the options that were on the table>

## Decision

<what we chose>

## Why

<the reason, including what we rejected>

## Follow-on

<what this unblocks, or "none">
```

Create the directory if it is missing. Do not write a decision for a reversible default you can change later at no cost. Do write one when reversing the choice would waste work, surprise a later reader, or close off an option the user might still want.

## Done

The interview is finished when no ready question remains and no standing assumption is still silent. Restate the decisions in a short list, with paths to the files you wrote. Ask the user to confirm the picture. Do not write production code. If they want a plan next, tell them to invoke `plan`.
