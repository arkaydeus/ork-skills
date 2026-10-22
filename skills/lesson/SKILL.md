---
name: lesson
description: 'Use only when the user explicitly invokes this skill by name (for example /lesson or "run lesson"), typically after they have corrected you. Find the root cause of the mistake and add a durable guard (an AGENTS.md rule, a test or a lint rule).'
---

# Lesson

The user just corrected you. Fixing the one slip is not this skill. Make the same slip hard to repeat.

## 1. Name the mistake

Restate, in one or two sentences, what you did and what they wanted instead. If you are not sure, ask once. Do not start editing guards while the mistake is still fuzzy.

Look for the class, not the anecdote. "I used the old client" is an instance. "Nothing stops an agent from importing the legacy client" is the class. Search recent commits, review comments and agent instruction files for the same class. A class that has happened twice is worth a structural guard.

## 2. Find why it was easy

Agents copy the nearest example and take the first path that compiles. Ask:

- Was a dead API, a stale comment or a second helper still sitting there to copy?
- Did the types accept the wrong state?
- Was the only instruction a sentence in a markdown file that nothing enforces?
- Did a test pass even when the behaviour was wrong?

The root cause is the missing signal, not "I should have been more careful".

## 3. Pick the strongest guard that fits

Work this list from the top. Stop at the first level that would have caught *this* instance.

1. **Remove the footgun.** Delete the old helper, hide the internal, give the state one owner. A vanished API cannot be copied.
2. **A test.** If the mistake is wrong behaviour a user could observe, add or fix a test that fails on the old behaviour and passes on the new. Keep that test.
3. **A lint or type rule.** If the mistake is a detectable pattern (a forbidden import, a naming rule, a disallowed API), add a check whose error names the file or function to use instead.
4. **An agent rule.** Write a short rule in `AGENTS.md`, or `CLAUDE.md` if that is what the repo already uses, only for a judgement call that a machine check cannot see. Rules with no enforcement are last, not first.

Do not add all four. A rule that restates a test is noise. A test that restates a deleted API is late.

## 4. Prove the guard

Show that the guard fails on the old mistake. For a test, run it against the bad behaviour (revert locally, or write the test first) and then against the fix. For a lint, run it on a snippet that copies the old pattern. For a deleted API, show the compile or type error. Paste the command and the relevant output.

If you cannot prove it, you have written a comment, not a guard. Try a different level.

## 5. Keep the table

If `AGENTS.md` (or the repo's equivalent) already has a short list of rules and what enforces them, add a row. If the rule was already written and you still made the mistake, the rule is decoration: replace it with a higher guard in the same change.

## Reply

- The mistake class and the evidence you found.
- The guard you chose, and why a stronger one did not fit.
- The proof command and its result.
- Paths you edited.

Do not lecture. The repo change is the lesson.
