---
name: babysit-pr
description: Get a pr to merge. Use when asked to babysit, watch, or follow a PR.
---

# Babysit PR

The pr you are doing this for is:

- If you are in a branch with a remote pr on it already, that one
- If the user includes a specific pr in their request, babysit that one

The end state here is:

- The PR doesn't have merge conflicts with the target branch
- The CI checks are green
- The AI code review bots are all green, their reviews have been addressed (either by implementing them or by ignoring
them because they're not worth addressing)

Steps to take:

1. Watch the PR for CI and code review. For polling the review bots, fetching their full findings, and deciding
fix-vs-skip.
2. When issues come in, fix them locally then commit and push them up
3. Wait for the checks to run again, if there are more issues, repeat step 2, otherwise move on to the next step
4. Give the user a concise summary of the changes you made to fix the pr and a concise list of things the PR actually does

NOTE: 

If a review bot leaves feedback you believe is not worth addressing, reply and resolve the comment. Format comments left on Yasir's behalf as:

```md
[MODEL-SLUG] RESPONDING ON BEHALF OF YASIR

[actual reply]
```

Do not let review feedback expand the PR beyond the user's original goal. Address real shortcomings, but avoid scope creep.

If nothing has changed, stay quiet rather than posting filler comments. if the user asks for an extra review from you, use a subagent to do that review and treat it like one of the ai
code reviewer bots. Take it's findings and follow the steps above.
