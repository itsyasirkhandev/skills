---
scope: universal
eagerness: available
provenance: adapted from State Change operating model
---

# Proportional review: preserve the goal, not the implementation strategy

Review is evidence about both a change and the design that produced it. A
review loop may keep repairing an implementation only while the findings stay
bounded inside the accepted design. Persistence is a commitment to the goal,
not an unlimited commitment to the first implementation strategy.

## The verdict vocabulary

Every substantive review lap resolves to one of four judgments:

- **CLEAN** — no substantive finding remains; proceed.
- **FIX** — findings are bounded defects inside the accepted design; repair
  and review again.
- **RESHAPE** — findings invalidate, disproportionately enlarge, or materially
  change the risk of the implementation strategy itself; stop implementation
  and bring back smaller or different design options.
- **ABANDON** — the feature no longer justifies the complexity, risk, or
  ownership burden required to deliver it; stop that candidate and state
  whether it is closed, superseded, or needs a smaller successor.

## The heuristic

If you keep repairing the repair, reconsider the design. Any one of these
means step back rather than take another local patch:

- three substantive laps of findings at the same architectural boundary;
- a fix that introduces machinery absent from the approved design;
- review/fix effort that overtakes or materially changes the feature's size or
  risk class; or
- a defect found in machinery built to repair an earlier finding.

When this fires, write a short prose comment: what is recurring, which premise
of the current design you now question, and at least one smaller or different
option. Then stop and let the responsible human or orchestrator decide what
runs next.

`RESHAPE` and `ABANDON` end the reviewed candidate or strategy. They do not
erase the evidence or automatically close every related piece of work.
