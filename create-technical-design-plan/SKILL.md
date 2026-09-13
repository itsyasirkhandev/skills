---
name: create-technical-design-plan
description: create technical design plan from spec.md. use when user aks to create the technical design plan.
---

# Create Technical Design Plan

Transform a specification document (`spec.md`) into an  technical design plan (`design.md`).

## 1. Context & Spec Intake

1. Locate and read the target specification (`.agents/specs/[ID]-[feature-slug]/spec.md`).
2. Inspect relevant codebase areas using `read` and search tools (`bash` with `find`/`rg`) to understand existing architecture, conventions, and dependencies.
3. If technical trade-offs require clarification, use `ask_user_question` with structured options.

## 2. Technical Design Structure

Read [EXAMPLE.md](EXAMPLE.md) try to create design.md like this.

## 3. Storage Location

Save the completed design document alongside the spec:
`.agents/specs/[ID]-[feature-slug]/design.md`
