# Advanced Prompting Patterns

Patterns for autonomous agent loops, tool routing, multi-stage pipelines, long-context engineering, and model-paradigm adaptations.

---

## 1. Autonomous Agent Loops & Harness Design

In an agentic loop, the LLM acts as the decision engine iteratively calling tools, evaluating observations, and producing artifacts.

### Agentic Contract Structure
```text
Role:              Autonomous code refactoring agent.
Goal:              Migrate the authentication service from callbacks to async/await across src/auth/*.ts.
Success Criteria:
- All callback patterns in target files are converted to async/await.
- TypeScript compiler (`pnpm check`) runs with zero errors.
- Unit and integration tests (`pnpm test`) pass cleanly.
Constraints:
- Do not modify API method signatures or external interfaces.
- Only edit files in `src/auth/`; ask before editing shared utilities.
Tools & Routing:
- Use `read` to inspect files before editing.
- Use `edit` for surgical replacements; avoid full-file rewrites.
- Run `bash` for tests and type checks only after editing is complete.
Stop Rules:
- Complete when tests pass and diff is verified.
- If a circular dependency is encountered, halt and report the exact cycle to the user.
```

### Key Agent Invariants
1. **Preamble and Cadence**: For multi-step tools, instruct the model to provide a brief 1-sentence intention before tool invocation, avoiding verbose step-by-step narration.
2. **Action Boundaries**: Explicitly distinguish safe local read/inspect actions from destructive, external, or high-cost actions (e.g., deleting branches, pushing to remote, executing external network calls).
3. **Completion vs Stalling**: Prevent the agent from stopping prematurely with an unexecuted plan. If actions are authorized, the agent must execute them.

---

## 2. Deterministic Tool Routing & Execution

Tool descriptions in agent schemas are prompts. Write them with unambiguous routing conditions.

### Principles of Tool Prompting
- **Describe Purpose & Side Effects**: State exactly what the tool does, when it should be chosen over alternative tools, and what side effects it produces.
- **Conditional Routing Rules**: Provide rules for sequencing and fallbacks:
  ```text
  1. Check local cached state first (`read_cache`).
  2. If cache is stale or missing, invoke `fetch_remote_data`.
  3. If a search query yields no results, broaden search keywords before declaring failure.
  ```
- **Parallel vs Sequential Tool Calling**:
  - Independent reads/queries should be executed concurrently.
  - Dependent actions (e.g., read file -> compute hash -> update record) must execute sequentially.
- **Error Recovery Loop**: Provide actionable guidance for handling tool errors:
  ```text
  When a tool returns an error:
  1. Analyze the error message and parameters.
  2. Attempt a targeted correction (max 2 retries).
  3. If the error persists due to permission or missing resources, halt and inform the user.
  ```

---

## 3. Model Paradigm Adaptations

Different model architectures require distinct prompting styles:

### A. Reasoning & Extended Thinking Models (e.g., OpenAI o1/o3, Claude Extended Thinking, DeepSeek-R1, Gemini Thinking)
- **Do not mandate visible Chain-of-Thought**: Avoid instructions like "Think step-by-step out loud in your response." The model natively performs latent chain-of-thought reasoning before generating tokens.
- **Focus on Goal & Boundaries**: Spend prompt budget on constraints, edge-case definitions, verification criteria, and stop rules. The model's reasoning process works backward from clear goals.
- **Calibrate Reasoning Effort**: Use low/medium effort for formatting, synthesis, or routine transformation; reserve high/max reasoning effort for architecture design, deep debugging, mathematical proofs, and complex coding.
- **Avoid Reasoning Extraction**: Do not ask the model to dump its internal reasoning scratchpad into response text; ask for verifiable evidence, calculations, and final conclusions.

### B. Standard Instruction-Tuned Frontier Models (e.g., GPT-4o, Claude Sonnet/Opus non-thinking, Gemini 1.5/2.0 Pro)
- **Explicit Stepwise Decomposition**: Provide clear workflow phases (Analyze -> Plan -> Draft -> Verify).
- **Strong Structural Scaffolding**: Use markdown headers, bullet structures, and output schemas to maintain focus across long context windows.
- **Few-Shot Guidance**: Include 2–4 diverse few-shot examples to lock in complex output formats or classification schemas.

### C. Compact & Open-Weights Models (e.g., 7B–14B, Mistral, Qwen, Llama 3/8B)
- **Single-Task Focus**: Avoid overly complex multi-objective prompts; decompose tasks into smaller, chained single-turn prompts.
- **Strict Formatting Delimiters**: Use strict JSON schemas or rigid XML templates.
- **Aggressive Example Anchoring**: Use 3–5 exact few-shot examples showing inputs and expected outputs.

---

## 4. Multi-Agent Delegation & Delegation Damping

When an agent harness supports spawning subagents or background workers:

### The Delegation Damping Rule
Models with high autonomy often over-delegate simple tasks or spawn excessive redundant agents. Enforce strict delegation thresholds:
```text
Delegation Decision Rule:
- Execute tasks locally using direct tool calls when the work can be completed within 3–5 actions.
- Delegate to a subagent ONLY when the task is completely independent, long-running, or computationally/contextually isolated.
- Never spawn a subagent merely to review or double-check your own recent output.
- Cap concurrent subagents at [N] (e.g., max 3 concurrent tasks).
- Synthesize all subagent results into the primary workflow before taking dependent actions.
```

---

## 5. Long-Context Prompt Architecture

When supplying massive reference files, repository dumps, or multi-document context (100k+ tokens):

### Data-First, Query-Last Layout
Place reference documents at the top of the context and the specific user query/task instruction at the bottom. LLMs attend more accurately to the prompt termination point for the immediate action contract.

```xml
<context_documents>
  <document id="doc_1" filename="spec.md">
    ...
  </document>
  <document id="doc_2" filename="architecture.md">
    ...
  </document>
</context_documents>

<task_instructions>
  Based exclusively on <context_documents>, answer the following question:
  [Specific Query Here]
</task_instructions>
```

### Retrieval Anchoring
- Request citation of specific line numbers, document IDs, or short verbatim quotes before summarizing.
- Avoid requesting excessive quotes for every sentence to prevent token bloat and context distraction.

---

## 6. Prompt Chaining vs Monolithic Prompts

Do not force a complex multi-stage process into a single massive prompt if stages have independent failure modes.

| Stage | Input | Output | Purpose |
| :--- | :--- | :--- | :--- |
| **Stage 1: Extract & Filter** | Raw unformatted data | Structured JSON facts | Eliminate noise and isolate core entities |
| **Stage 2: Reason & Plan** | Structured facts | Concrete step-by-step plan | Evaluate trade-offs and verify feasibility |
| **Stage 3: Execute & Generate**| Validated plan + raw context | Target deliverable (code, copy, report) | Produce final artifact |
| **Stage 4: Validate** | Final artifact + criteria | Validation report / pass-fail | External or harness-driven verification |

*Chaining Rule*: Chain prompts when stages require different tools, different system permissions, different output formats, or benefit from fresh context windows.
