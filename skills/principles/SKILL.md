---
name: principles
description: 'Use on every coding or writing task in this repo. Refuse slop (filler comments, vague names, overengineering) and ask rather than guess when a genuine ambiguity would change the work.'
---

# Principles

Two standing rules. Apply them while other skills run. They do not replace a playbook.

## No slop

Ship the work, not the performance of working.

**Comments.** Write a comment only when the code cannot show a constraint: a legal or safety rule, a surprising vendor quirk, a number with a history. Delete comments that retell the next line, mark a "phase", or apologise. Do not leave commented-out code.

**Names.** A name says what the thing is in the project's words (`docs/terms.md` when it exists). Do not use `data`, `info`, `helper`, `util`, `manager` or `temp` when a precise word exists. Do not cycle synonyms for the same idea in one file.

**Shape.** Add a layer, an interface, a config flag or a wrapper when there are two real implementations or a real boundary. One call site does not need a framework. Delete the unused branch, the extra indirection and the half-finished abstraction you were proud of.

**Prose.** Short sentences. No filler ("in order to", "it is worth noting"). No chatbot closings. No claim without a command, a path or a labelled guess. Prefer the plain word. For a full prose de-slop on request, use `unslop`.

**Extras.** Do not repair neighbours, restyle untouched files or invent features the request did not contain. If you see dirt, mention it; do not sweep it into this diff.

## Ask, do not guess

Uncertainty that would change the diff is a stop, not a coin flip.

**Ask the user** when the choice is preference, product or policy: who may see a record, what the empty state should do, whether to break a public API, which of two agreed designs to keep.

**Look it up** when the uncertainty is a fact: what a file contains, what a command prints, what `main` does, whether a test exists. That is not a question for the user.

**Label a guess** only if you must move a single step to *form* the question, and you say it is a guess. Never present a guessed URL, API, requirement or historical motive as known.

**Do not** ask "should I proceed?" on a reversible step the user already assigned you. Do the step, show the result. Do not ask questions whose answer is in the repo. Do not hide a product decision inside a default and hope they notice.

If two readings of the request produce different code, stop and ask one precise question. Offer a recommendation, and treat "yes" as acceptance.

## When they conflict

If "just ask" would block a fact you can measure, measure. If "just ship" would invent a behaviour, ask. If you are writing a comment to paper over a poor name, rename instead. If a playbook step does not apply, keep it on the list with `skip: <reason>` rather than pretending you did it.
