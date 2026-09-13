---
name: prompt-engineer
description: prompt enhancement use when user needs to improve the prompt that will be send to AI agent.
---

# Prompt Engineering Skill

Create, optimize, debug, and evaluate prompts across any model family (OpenAI, Anthropic, Google Gemini, DeepSeek, open-weights/local models) and any agent harness or framework (Pi, Claude Code, Cursor, Windsurf, LangChain/LangGraph, LlamaIndex, custom API loops).

## Use This Skill For

- Authoring system, task, agent, tool, or few-shot prompts from scratch
- Refactoring legacy or bloated prompts using the delete-first methodology
- Optimizing prompts for reliability, deterministic tool routing, parser adherence, token efficiency, or reduced latency
- Adapting prompts across model archetypes (reasoning/thinking models, general frontier models, fast/compact models)
- Designing multi-turn agent loops, subagent delegation rules, and structured tool workflows
- Diagnosing prompt failures: hallucinations, prompt injection vulnerability, format drift, over-delegation, or premature stopping

## Universal Workflow

1. **Clarify the Core Contract**:
   Establish the desired outcome, target audience/consumer (human or parser), execution context (single-turn, RAG, multi-step agent loop), tool/environment access, and hard failure cases. Ask clarifying questions only if missing information fundamentally alters prompt architecture.
2. **Apply the Canonical Anatomy**:
   Structure complex prompts using the universal anatomy:
   `Role · Goal · Success Criteria · Constraints · Tools & Routing · Output Contract · Stop Rules`
   *(Omit sections that do not affect model behavior).*
3. **Apply Delete-First Optimization**:
   Eliminate redundant instructions, conversational filler, decorative personas, and obsolete workarounds before adding new text. Every token must carry behavioral weight.
4. **Calibrate to Model Archetype & Harness**:
   Consult relevant references below to align with model capabilities (e.g., native extended thinking vs instruction-tuned, tool calling behavior) and harness capabilities (e.g., programmatic tool use, subagent spawning, structured outputs).
5. **Run Empirical Verification**:
   Test surgical prompt modifications on representative inputs (happy path, edge cases, missing data, adversarial inputs). Measure task success, parser validity, tool accuracy, token count, and latency.
6. **Deliver the Output**:
   Provide the refined prompt in clean markdown code blocks along with a brief architectural breakdown, key trade-offs, and an evaluation/test strategy.

---

## Progressive Disclosure

Load detailed reference guides based on task requirements:

| Reference | When to Load | Covers |
| :--- | :--- | :--- |
| `references/core_prompting.md` | Authoring, restructuring, or clarifying any prompt | Core anatomy, directness, XML/markdown demarcation, few-shot design, output contracts, grounding, safety boundaries |
| `references/advanced_patterns.md` | Building agents, tool workflows, long-context RAG, or multi-model systems | Autonomous loops, tool routing, delegation damping, reasoning/thinking paradigms, long context optimization, multi-stage chaining |
| `references/quality_improvement.md` | Benchmarking, troubleshooting, optimizing cost/latency, or hardening security | Delete-first workflow, evaluation frameworks, hallucination mitigation, prompt injection defense, troubleshooting matrix |

When loading these documents, read the exact relative path: `references/core_prompting.md`, `references/advanced_patterns.md`, or `references/quality_improvement.md`.

---

## Universal Cross-Model & Cross-Harness Invariants

1. **Outcome Over Routine**: Specify the destination and observable completion criteria; let capable models determine routine execution paths unless a rigid sequence is required.
2. **Context & Rationale**: Provide the operational context and brief rationales for non-obvious constraints. Models follow rules more reliably when the underlying reason is evident.
3. **Calibrated Directive Language**: Reserve absolute imperatives (`MUST`, `NEVER`, `ALWAYS`) exclusively for non-negotiable invariants (safety rules, strict schemas, permission boundaries). Use conditional logic (`if / when / unless`) for tactical judgment calls.
4. **Explicit Output Contracts**: For human readers, specify sections, length limits, and tone. For machine consumers, define exact JSON/YAML schemas, enum values, and fallback behavior for missing data.
5. **Clear Information Architecture**: Separate instructions, reference data, examples, and untrusted user input using consistent delimiters (e.g., XML tags like `<context>`, `<documents>`, `<instructions>`, `<input>`).
6. **Grounding & Epistemic Humility**: For source-bound or retrieval tasks, instruct the model to ground claims in provided context, explicitly distinguish inferences from direct citations, and declare when evidence is insufficient rather than fabricating answers.
7. **Harness Separation of Concerns**: Keep deterministic validation, secret management, persistent state tracking, and permission checks in application code/harness logic—do not rely solely on system prompt prose to enforce hard system security.

---

## Quick Selection Guide

| Need | Primary Pattern | Reference |
| :--- | :--- | :--- |
| Unclear output or format drift | Explicit schema + `<output_format>` contract + 2–3 diverse examples | `references/core_prompting.md` |
| Variable or untrusted user inputs | XML tagging + explicit control/data boundary | `references/core_prompting.md` |
| Complex autonomous execution | Agentic state contract + observable success & stop rules | `references/advanced_patterns.md` |
| Tool hallucination or routing errors | Conditional routing rules + prerequisite validation | `references/advanced_patterns.md` |
| Long-document RAG / analysis | Data-first, query-last document ordering + citation grounding | `references/advanced_patterns.md` |
| Subagents over-spawning or looping | Delegation damping threshold + concurrency cap | `references/advanced_patterns.md` |
| High latency or runaway token cost | Delete-first trimming + reasoning effort calibration | `references/quality_improvement.md` |
| Inconsistent evaluation / regressions | Test matrix with golden inputs + automated schema checks | `references/quality_improvement.md` |
| Injection or jailbreak vulnerability | Input isolation + behavioral bounds + harness-level validation | `references/quality_improvement.md` |
