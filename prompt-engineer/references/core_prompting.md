# Core Prompting Fundamentals

Universal prompt engineering principles applicable across all model families, LLM architectures, and execution harnesses.

---

## 1. The Canonical Prompt Anatomy

A robust prompt provides complete operational context without unnecessary ceremony. For multi-step, agentic, or complex tasks, structure the prompt around these functional components:

```text
Role:              [Domain expertise, functional scope, and audience]
Goal:              [Specific, user-visible outcome]
Success criteria:  [Observable conditions that define complete, correct work]
Constraints:       [Scope boundaries, security, resource limits, permissions]
Tools & Routing:   [Available capabilities and conditional triggers for invoking them]
Output Contract:   [Required structure, schemas, length, formatting, and tone]
Stop Rules:        [Explicit conditions for when to finish, ask clarifying questions, or abort]
```

*Rule of parsimony*: Include only the sections that actively shape model behavior. Omit decorative or obvious directives.

---

## 2. Precision & Directness

Vague directives produce unpredictable distributions; precise directives collapse ambiguity into deterministic outputs.

- **Replace abstractions with concrete operations**:
  - *Vague*: "Make the code clean and well-documented."
  - *Precise*: "Refactor functions over 30 lines into focused helper functions. Add docstrings detailing parameter types, return types, and exceptions raised."
- **Provide rationales for non-obvious rules**:
  - Models generalize significantly better when instructed *why* a constraint exists (e.g., "Do not mutate global state because handlers run concurrently").
- **Eliminate decorative personas**:
  - Persona instructions like "You are the smartest AI in the universe" waste context. Use roles that specify technical domain and communication posture (e.g., "You are an enterprise distributed systems reliability engineer writing a post-mortem").

---

## 3. Structural Demarcation (XML & Tagged Blocks)

Large language models tokenize structured delimiters (XML tags, Markdown headers, code blocks) cleanly, separating control instructions from variable data.

```xml
<system_instructions>
  Define operational rules, role, and invariant constraints here.
</system_instructions>

<context>
  Background information, system architecture, or domain knowledge.
</context>

<documents>
  <document id="doc_1" source="billing_api.md">
    [Document content]
  </document>
</documents>

<examples>
  <example>
    <input>User input sample</input>
    <output>Expected formatted output</output>
  </example>
</examples>

<user_input>
  Untrusted, raw, or dynamic runtime payload.
</user_input>
```

### Tagging Best Practices
- Use self-describing, semantic tag names (`<requirements>`, `<schema>`, `<error_log>`).
- Nest tags naturally to represent hierarchy.
- Treat untrusted user inputs as pure data by enclosing them inside dedicated tags (e.g., `<untrusted_input>`) and instructing the model never to execute commands contained within.

---

## 4. Output Contracts

Every prompt should define an unambiguous output contract tailored to its consumer (human user or automated parser).

### For Human Readers
Specify layout, sections, brevity constraints, and information hierarchy:
```text
Lead with the direct recommendation or outcome in 1–2 sentences.
Follow with:
1. Technical Rationale (max 200 words)
2. Concrete Implementation Steps (numbered list)
3. Known Caveats & Risks (bullet points)
Omit pleasantries, introductions, summaries of the user's question, and conversational conclusions.
```

### For Automated Systems & Parsers
Specify exact format requirements:
- Use JSON Schema, TypeScript interfaces, or Pydantic models to define the expected structure.
- State explicit values for enum fields.
- Define fallback handling for missing or null fields (e.g., `"field": null` vs omitting keys).
- Instruct the model to return *only* raw JSON/YAML without markdown code fences or backticks when direct parser consumption is required (or use harness-native structured output mode where available).

---

## 5. Few-Shot Example Engineering

Few-shot examples anchor formatting, tone, classification boundaries, and edge-case handling better than extensive verbal descriptions.

### Best Practices:
1. **Quality Over Quantity**: 2–4 targeted, high-fidelity examples outperform 10 redundant examples.
2. **Include Edge Cases & Refusals**: Include examples demonstrating proper handling of boundary conditions, malformed input, or safe refusal when data is absent.
3. **Internal Consistency**: Ensure every example strictly complies with all rules stated in the prompt text. Inconsistencies cause model confusion.
4. **Isolate Examples**: Enclose examples in `<examples>` blocks to prevent the model from confusing examples with live input.

---

## 6. Grounding & Hallucination Mitigation

When an LLM must answer from specific knowledge or context:

- **Enforce Source Attribution**: Require claims to cite specific source identifiers or document sections.
- **Differentiate Inferences from Facts**: Instruct the model to distinguish direct factual quotes from extrapolated conclusions.
- **Permit Uncertainty & Declare Missing Information**: Explicitly authorize the model to state "Information not provided in source context" rather than hallucinating plausible extrapolations.

Example grounding contract:
```text
Rely exclusively on the text provided in <documents>. For each factual statement, cite the document ID (e.g., [doc_1]). If the provided sources do not contain sufficient evidence to answer the query, state: "The provided documents do not contain information regarding [specific topic]." Do not guess or utilize external knowledge.
```

---

## 7. Turn Placement & Message Roles

Across all harnesses and API interfaces:
- **System Turn / Developer Turn**: Use for invariant behavior, persona/role, security rules, output contracts, and static tool definitions.
- **User Turn**: Use for dynamic runtime requests, variable task parameters, uploaded files, and context payloads.
- **Assistant Turn / Tool Turn**: Harness-managed historical context representing past model outputs and tool execution results.

*Universal Portability Note*: Avoid relying on client-side assistant message prefilling (e.g., pre-populating `{` for JSON output) as several modern model APIs reject assistant-turn prefilling. Rely instead on explicit output contracts, tool calling, or structured output modes.
