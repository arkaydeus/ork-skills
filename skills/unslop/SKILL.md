---
name: unslop
description: Use only when the user invokes /unslop or @unslop, or explicitly asks to unslop, humanise, de-slop, or strip AI tells from a draft. Do not load this skill for ordinary replies.
license: MIT
---

# Unslop

Strip AI tells. Keep the meaning. Do not invent a voice the facts do not support.

## Invoke only

Load this skill when the user runs `/unslop` or `@unslop`, or clearly asks to unslop, humanise, de-slop, or strip AI tells from a draft. Do not load it for ordinary replies.

## Modes

The default is a rewrite. Switch to audit when the user asks only to flag, to audit, or to leave the draft untouched ("just flag it", "audit", "don't change anything"). In audit mode, list the tells and return the source as you found it.

## Guards

- If an edit is not clearly required, leave the line alone.
- Keep every figure, date, person, quotation, citation, link, fenced snippet, unit, and any never/must/all that carries a legal or safety load, exactly as written.
- Do not smuggle in a new fact, story, certainty, or wrap-up the source never made.
- Quoted lines, blockquotes, and fenced code stay untouched unless the user asked you to annotate a tell inside them.
- Do not sprinkle canned "human" flavour. Chopping every sentence down to a bark is another tell.
- This skill's own comments use British spelling (colour, organise, towards). If the draft is already US English, do not restyle it while you strip tells.

## Process

Read [`references/patterns.md`](references/patterns.md) in full before you touch the draft. Rule numbers there are stable ids.

1. Scan the draft against those tells.
2. On a rewrite, repair only the spans that fail a tell. Leave every clean sentence as it stands.
3. Put voice back in (see Adding soul) when you are drafting or the user asked you to rewrite. Skip this on a flag-only pass.
4. Self-audit: "What makes this obviously AI generated?" Fix remaining tells.

## Adding soul

Removing patterns is half the job. Sterile, voiceless writing is just as obvious.

- **Have opinions.** React to facts instead of neutrally listing pros and cons.
- **Vary rhythm.** Short sentences. Then longer ones that take their time. Mix it up.
- **Acknowledge complexity.** "Impressive but also kind of unsettling" beats "impressive."
- **Use "I" when it fits.** First person isn't unprofessional.
- **Let some mess in.** Perfect structure feels algorithmic.
- **Be specific.** Not "this is concerning" but "there's something unsettling about agents churning away at 3am."

## Audit output

When the pass is flag-only, return a list, not a rewrite:

```markdown
# Flag-only

## Hits
- Rule <id> (<name>): "<excerpt>" — <why it is a tell>

## Unsure
- Rule <id> (<name>): "<excerpt>" — <why it might be a tell>

n hits, m unsure. Source not edited.
```

When rewriting, return the cleaned text only. Do not attach a methods lecture.
