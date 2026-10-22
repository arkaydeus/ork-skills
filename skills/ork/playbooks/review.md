# Review

Produce a verdict, not a rewrite. Do not apply fixes unless the user asks after they have seen the triage.

1. Agree the scope: a path, a diff, or the current branch against its base. If the scope is empty, stop and ask.
2. If the user asked for a multi-model or adversarial pass, or invoked this playbook after `panel`, invoke `panel`. Otherwise invoke `review` for a single-pass read on correctness and design.
3. Report the triage. For `panel`, use its four buckets. For `review`, list correctness findings then design findings. Do not edit the code in this playbook (`skip: review does not apply fixes`) unless the user now asks for a follow-up fix, in which case start the `bug` or `feature` playbook instead of stretching this one.
