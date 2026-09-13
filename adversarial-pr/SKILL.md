---
name: adversarial-pr
description: Independent adversarial code review and proportional PR evaluation. use for review the code that AI generated
---

# Adversarial PR review

This skill defines a review outcome and its guardrails. It does not prescribe a
launcher, model, or vendor. Use the capabilities available in the active
environment, and let the project choose its transport and technical setup.

The verdict vocabulary is defined in
[`references/proportional-review.md`](references/proportional-review.md).
Review must be genuinely independent of the author, must see the issue and the
exact proposed change, and must cover:

1. correctness, security, and maintainability;
2. fidelity to the issue and its broader concern; and
3. documentation fidelity, including nearby drift.

## Direct path

1. Inspect `git status`, the commits and diff against the actual base, and the
   issue. Run the relevant local checks before the first push.
2. Create and push a branch, then open a draft PR whose body contains a
   summary, exact local-test receipts, things to push back on, and `Closes #N`
   when appropriate. Keep it draft through repair laps.
3. Establish current reviewer facts: authorship family, candidate family,
   capability for this task, authentication, and fresh budget. Select a
   different-family reviewer. Never infer family or availability from a
   harness name.
4. Stage the issue body, important comments, PR description, and exact diff in
   a least-authority workspace. Inspect and redact the bytes before crossing a
   provider boundary. Do not send credentials, private-person data, unrelated
   files, or authority to publish, merge, deploy, or mutate the source tree.
5. Invoke the selected reviewer with its native interface as one bounded
   foreground call. Give it the three-part rubric above and ask for findings
   with file/line evidence plus one overall verdict. Preserve a provenance
   receipt naming the author family, reviewer/model family, exact model,
   transport, payload scope, budget source/freshness, the provider-completion
   and caller-owned total time bounds, and whether strength was reduced. A
   timeout is `NO VERDICT`, not an adverse verdict.
6. Apply the verdict:
   - `CLEAN`: proceed to owner review.
   - `FIX`: make a bounded repair, rerun affected checks, push, and obtain a
     fresh independent review of the new head.
   - `RESHAPE` or `ABANDON`: preserve the branch, post the reason on the PR,
     and stop implementation.
7. Before another repair lap, re-check budget and the repair-the-repair
   boundary. Three substantive laps at the same boundary, a repair that
   invents a subsystem, or review effort that materially reclassifies the
   feature is `RESHAPE`, not permission to continue.
8. On `CLEAN`, run owner review for the exact head SHA. Post that close-out on
   the PR, mark it ready, and merge only when the issue's routing authorizes
   merge. Leave durable evidence on GitHub.

## Review-loop discipline

- Run review synchronously and bound its duration. If a provider hangs, end
  that attempt and record `NO VERDICT`; never wait indefinitely.
- Transport failure does not silently waive review. Try another independently
  eligible candidate only after making that new selection explicitly.
- Same-family review is reduced strength and cannot be represented as
  independent. If no independent reviewer is available, leave the PR
  draft-pending with the receipt.
- A worker never self-waives a non-`CLEAN` verdict. Disagreement is evidence
  for the orchestrator, not merge authority.
- P3 nits do not force a lap. A small, clearly correct documentation repair may
  accompany a `FIX`; a design dispute does not.
- Refresh the base before each review when concurrent merges may have moved it.

## Optional affordances

Bounded tools may help with payload scrubbing, disposable isolation, process
cleanup, or provider telemetry. They remain optional only if substitutes are
allowed, the direct path above still works without them, and they return facts
or receipts rather than choosing the reviewer or next action.

Do not install repository-owned PATH shims or require a universal `review`,
`budget`, or reviewer-selection command. Technical setup belongs to the
project or active harness.

## Completion

Return the PR URL and head SHA; overall verdict; review provenance; exact local
checks; repair-lap count; owner-review receipt; merge state; and any follow-up
issue. Never call work done merely because a reviewer produced no text or CI is
green.

## When not to use this skill

- No change exists to review.
- The task is still an unresolved design decision; shape it first.
- The requested transport would expose secrets or grant the reviewer mutation
  authority.
- The user requested review only, without authorization to change or merge.
