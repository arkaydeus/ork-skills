---
name: terms
description: 'Use when a domain word is introduced, used in two ways, or when docs/terms.md already exists. Keep that glossary current and use the canonical terms in code, plans and replies.'
---

# Terms

A project needs one word for one idea. `docs/terms.md` is that list. It is not a design doc and not a place for implementation notes.

## Read first

At the start of any task that names the domain, read `docs/terms.md` if it is there. Use those words in code, commits, plans, decisions, pull requests and speech. If the user says a synonym, translate it and, if it matters, ask which word they want to keep.

## Challenge a clash

When the user, a comment or a new identifier conflicts with the glossary, say so at once. Offer the canonical term and the colliding one. Do not silently pick.

When a word is vague ("account", "session", "job"), propose a tighter term and a one-line definition. Put the choice to the user if both readings would change the work.

When they describe behaviour the code does not match, say that. The glossary follows the agreed meaning, not a stale function name. Renaming the code is a separate change; note it rather than hiding the clash.

## File shape

Create `docs/terms.md` when the first term is settled, not before. Use this shape:

```markdown
# Terms

Words the project uses in a precise way. No implementation detail.

## <Term>

<one or two sentences: what it is, in the user's world>

Avoid: <rejected synonyms>

## <Another term>

...
```

Keep terms in a readable order, usually alphabetical. One heading per term. No tables of database columns. No "we might later". No stack names unless the term *is* the product name.

If the repo already has a glossary elsewhere, do not start a second one. Extend the file that exists, or, if it is in the wrong place and the user agrees, move it to `docs/terms.md` and leave a pointer.

## Write as you go

When a term is agreed, edit the file in that turn. Do not batch vocabulary until the end of a long session. If `shape` or `plan` is running, they will call on this file; keep it ahead of them.

When a term is retired, do not delete history without a note. Strike it with a one-line "Retired:" reason, or move it to a short Retired section at the bottom.

## Use the words in the tree

New code, tests and docs you write in the same sitting should use the canonical term. If a nearby identifier still uses the old word and changing it is in scope, change it. If it is out of scope, leave it and mention the drift.

## What this skill is not

It is not an invitation to rename the world. It is not a spec. Decisions that need a why belong in `docs/decisions/`. Plans belong in `docs/plans/`. The glossary only names things.
