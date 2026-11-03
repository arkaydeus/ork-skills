# Notice

ork-skills is original text, written for this repository. It is not a fork and does not copy skill bodies from the works below, except for the pstack unslop adaptation named in the exception in the pstack section. If you use ork-skills, you must also respect their licences for any of *their* files you take yourself. The engineering skills drew on MIT works. The security skills drew on a mix of MIT, Apache-2.0, CC-BY-SA-4.0 (ideas only), and the Semgrep Rules License (no rules or text); each is listed in its own section. `motion` drew on unlicensed X articles (ideas only) and Robert Penner's MIT easing equations.

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

### Exception: adapted unslop text

`skills/unslop/references/patterns.md` (and any adapted unslop text in `skills/unslop/SKILL.md`, including the Adding soul section) contains text adapted from [pstack unslop](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md) (MIT, © 2026 Lauren Tan). That adapted text is kept under the notice below, not as a silent copy.

MIT License

Copyright (c) 2026 Lauren Tan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## theclaymethod/unslop

- Project: [theclaymethod/unslop](https://github.com/theclaymethod/unslop)
- Licence: none published (GitHub `license` metadata is null)

**Inspiration only. No text.** The flag-only (audit) mode, the fact-preservation guards, and extra tells 34–41 in `skills/unslop/references/patterns.md` were rewritten in our own words from ideas in that repository. We did not copy or paraphrase its files.

## Skills moved from arkaydeus/skills

`bun-testing` and `website-audit` are Ark's own MIT work, moved unchanged from [arkaydeus/skills](https://github.com/arkaydeus/skills). `unslop` is the merged skill described above (pstack text kept with credit; claymethod ideas rewritten). `motion` is Ark's MIT skill, moved from the same repository as an ideas-only rewrite of the sources listed in the motion section below; we did not copy article prose, inlined scripts, or GPL easing tables.

`website-audit` credits [Florence (@flxrnc)](https://x.com/flxrnc) and [damkin (@damkina7)](https://x.com/damkina7). `motion` credits the same two authors as ideas only; see the motion section.

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

## Security skills

These six skills (`security`, `security-review`, `security-audit`, `security-scan`, `security-supply-chain`, `security-threat-model`) are original MIT text. We read other public skill collections for **ideas** (phased audits, cheap-first routing, high-confidence diff review, scanner triage, supply-chain and Actions hardening, lightweight STRIDE models). We did not copy their files, wording, prompts, schemas, or scanner rules.

Licences below were checked against each GitHub repository (`license` metadata and the `LICENSE` / `LICENSE.txt` file) on 2026-10-25.

### cloudflare/security-audit-skill

- Project: [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill)
- Licence: MIT
- Copyright: © 2025-2026 Cloudflare, Inc.

Ideas we rewrote: a full-tree audit in phases (map, hunt by area, then a second agent tries to disprove each candidate); structured findings plus a coverage list; a warning that one run is costly and a later run still finds more.

### anthropics/claude-code-security-review

- Project: [anthropics/claude-code-security-review](https://github.com/anthropics/claude-code-security-review)
- Licence: MIT
- Copyright: © 2025 Anthropic

Ideas we rewrote: review the diff rather than the whole tree; keep false positives low; attach severity and a fix to each issue.

### agamm/claude-code-owasp

- Project: [agamm/claude-code-owasp](https://github.com/agamm/claude-code-owasp)
- Licence: MIT
- Copyright: © 2026 (as stated in that repository's `LICENSE`)

Ideas we rewrote: a short application checklist grouped by common risk themes, kept out of `SKILL.md` in `references/`, written in our own words rather than as a copy of OWASP or ASVS.

### openai/codex-security

- Project: [openai/codex-security](https://github.com/openai/codex-security)
- Licence: Apache-2.0

Ideas we rewrote: treat validation of a candidate as a separate step from finding it; record coverage so a later pass has somewhere to start.

### openai/skills

- Project: [openai/skills](https://github.com/openai/skills) (`security-threat-model`, `security-best-practices`)
- Licence: Apache-2.0 (per-skill `LICENSE.txt`; the repository has no root licence file)

Ideas we rewrote: a compact threat model (assets, actors, boundaries, STRIDE-style questions, mitigations) and a best-practice lens as a checklist, not a vulnerability dump.

### getsentry/skills

- Project: [getsentry/skills](https://github.com/getsentry/skills) (`security-review`)
- Licence: Apache-2.0
- Copyright: © 2025 Functional Software, Inc. dba Sentry

Ideas we rewrote: a diff-scoped security pass that prefers exploitable bugs over nits.

### trailofbits/skills

- Project: [trailofbits/skills](https://github.com/trailofbits/skills)
- Licence: CC-BY-SA-4.0

**Ideas only.** We did not copy or paraphrase that repository's skill text. ShareAlike would apply to a derivative of their wording; our security skills are a new MIT work. Ideas we took at the topic level: hunt from entry points, check candidates for false positives, and treat supply-chain and CI trust as its own review.

### semgrep/skills

- Project: [semgrep/skills](https://github.com/semgrep/skills)
- Licence: [Semgrep Rules License v1.0](https://semgrep.dev/legal/rules-license) (GitHub `license` key `other` / `NOASSERTION`; `LICENSE` file points at that URL)

**No rules and no text.** We do not vendor Semgrep rules, query packs, or skill prose from that repository. `security-scan` only tells the agent to invoke an already-installed Semgrep CLI against the public registry (for example `semgrep scan --config auto`) and to triage the output.

## motion

`motion` (`skills/motion/`) is original MIT text. We read public write-ups for **ideas** (code-first `seek(t)` capture, a directed brief, banned defaults, closed-form springs, a critique loop, synthesised sound on a beat grid, a workspace fence, quoting from scope). We did not copy their files, wording, prompts, or scripts.

Licences below were checked against each source on 2026-11-01. GSAP's current free-including-plugins terms were re-checked on 2026-11-03.

### Florence (@flxrnc)

- Article: [Opus 5.5 Motion Lab: Make Claude Animate Like a Studio](https://x.com/flxrnc/article/2105311836190978392)
- Licence: none published (X article)

**Ideas only. No text.** Ideas we rewrote: a directed brief with named fields; a capture contract so every frame is a function of time; banned AI-motion defaults; one hero per shot; a render-look-fix loop on real frames; engine picking; loop frames 0…N−1; GIF delay rounding; sound one frame after the picture; secondary-motion ratios; numeric critique gates.

We did not vendor that article's prompts, `render.mjs`, freeze snippet, contact-sheet commands as printed, or any article table or script. CSS `cubic-bezier` control points in `maths.md` are numeric facts next to Penner's names, not a copy of that article's easing table.

### damkin (@damkina7)

- Article: [Autonomous Motion Studio on Claude Opus 5.5: From seek(t) Architecture to Your First $500 Reel](https://x.com/damkina7/article/2107924159397560795)
- Licence: none published (X article)

**Ideas only. No text.** Ideas we rewrote: timers banned so capture can jump to any `t`; video-diffusion kept off UI and type; four job shapes (drawn in code, composition framework, overlay, texture only); shared picture/sound timestamps; overnight stop conditions; quote from duration, aspects, and revisions rather than a viral rate card.

We did not vendor that article's dashboard storyboard, click synthesizer, inspection prompt, seven-layer names, marketplace prices as a rate card, or Telegram invite. Published dollar figures stay labelled as unverified anecdotes in `references/pricing.md` and `references/harness.md`.

### Thomas and Johnston, *The Illusion of Life* (1981)

- Work: Frank Thomas and Ollie Johnston, *The Illusion of Life: Disney Animation* (1981)
- Licence: All rights reserved

**Ideas only. No book text.** `skills/motion/references/principles.md` turns widely known character-animation principles into numbered prompt lines (part, duration, magnitude). We did not copy the book's wording or illustrations.

### Robert Penner easing equations

- Project: [Easing functions (equations)](https://robertpenner.com/easing_terms_of_use.html)
- Licence: MIT (also offered as 3-Clause BSD)
- Copyright: © 2001 Robert Penner

`skills/motion/references/maths.md` includes compact JavaScript implementations of Penner's ease-out expo/quint, ease-in-out quart, ease-in expo, and ease-out back, plus the classic 1.70158 overshoot constant. That adapted code is kept under the notice below.

MIT License

Copyright (c) 2001 Robert Penner

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

### easings.net (not copied)

- Project: [ai/easings.net](https://github.com/ai/easings.net)
- Licence: GPL-3.0

**No snippets and no table copy.** That site is copyleft. We did not vendor its YAML, CSS, or prose. Named eases in `maths.md` are Penner's equations. CSS `cubic-bezier` control points sit next to those names as numeric facts, not Sitnik's table or wording.

### Other facts, not files

Engine licence lines in `references/engines.md` (GSAP now free including plugins, with a no-code/Webflow restriction; Motion/MIT and no video renderer; three.js/MIT; Remotion company licence; HyperFrames Apache-2.0) are current-terms reminders, not copies of those projects. Contrast floors 4.5:1 / 3:1 and the three-flashes-per-second cap are WCAG figures, cited as numbers only.

## What we left out

We did not add a TDD skill. We did not add a "small diffs" principle. We did not take pstack's Cursor-only setup, model tables, or generated harness trees. We did not take Matt's issue-tracker setup, ticket graph or plugin manifests. We did not vendor easings.net (GPL-3.0) cubic-bezier tables or JavaScript snippets. We did not copy Florence or damkin article prose, prompts, or inlined scripts. We did not include damkin's Telegram invite or treat published client prices as a rate card.

## Our licence

ork-skills is MIT, © 2026 Ark. See `LICENSE`.
