# Prototype

Answer one question with throwaway work. Isolation is the point. Nothing from this playbook is for merge unless the user later starts a fresh feature from what was learned.

1. Invoke `prototype`. Put the spike on its own branch or folder, labelled not for merge, and write down the question it is meant to answer.
2. Do not invoke `pr` against the default branch. `skip: spike is not for merge`. If the user wants a record of the attempt, point them at the spike branch or folder.
3. Write what you learned: a short note in `docs/decisions/` if a real choice followed, otherwise a paragraph in the reply. Offer `handoff` if another session will take the next step.
