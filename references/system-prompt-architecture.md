# System Prompt Architecture

Use this reference when drafting a new agent prompt or restructuring an existing one.

## Layers

A deployable agent prompt usually needs these layers. Omit layers that do not change behavior.

1. **Role and mission**
   - Use a concrete professional identity plus the domain.
   - Tie the role to the agent's actual work, not to a product codename or pipeline step.

2. **Scope and non-goals**
   - State what the agent handles.
   - State boundaries that affect runtime choices, user expectations, or handoff.

3. **Inputs and context boundaries**
   - Name the input types the agent receives.
   - Separate user-provided data, retrieved context, memory, tool results, and fixed policy.
   - Explain source priority when inputs conflict.

4. **Tool-use policy**
   - State when to use each tool, what evidence to gather, and when to stop.
   - Keep runtime schemas outside the prompt when the platform already provides typed tools.
   - If only broad capabilities are known, avoid inventing tool names or parameters.

5. **Workflow**
   - Use a short decision process only when order matters.
   - Prefer action-oriented checkpoints over abstract values.
   - For costly or irreversible work, use plan, validate, execute.

6. **Output contract**
   - Define the response shape, required fields or sections, verbosity, and completion signal.
   - Give a format example only when the format is fragile.

7. **Safety, privacy, and escalation**
   - Include boundaries for sensitive data, irreversible actions, regulated domains, prompt leaks, and user handoff.
   - Prefer safe alternatives and escalation criteria over broad refusal language.

8. **Failure handling and uncertainty**
   - State what the agent does when required inputs are missing, sources conflict, tools fail, or confidence is low.

9. **Verification and self-check**
   - Add checks that are visible in final behavior: cite evidence, run validation, compare output to criteria, or report residual risk.

## Stable vs Variable Context

Keep durable behavior in the stable prompt layer:

- role
- authority boundaries
- tool-use policy
- output contract
- safety and escalation
- verification rules

Keep task-local material outside the stable layer:

- user request
- retrieved documents
- current project state
- examples for a specific task
- temporary constraints
- dynamic variables

## Prompt Markers

Use Markdown headings or XML-style tags only when they separate content types. Good blocks include:

- `Role`
- `Mission`
- `Inputs`
- `Context Sources`
- `Tool Policy`
- `Workflow`
- `Output Contract`
- `Safety`
- `Failure Handling`
- `Verification`

Avoid nested ceremony when a short prompt is enough.

## External Context Inventories

For repo or product prompts, list exact paths when the runtime can access files:

```text
External context:
- `AGENTS.md` - repository agent rules; load before changing code
- `docs/api.md` - API contract; reference before endpoint changes
- `SECURITY.md` - disclosure policy; reference for security issues
```

Mark files as loaded, referenced, or out of scope when that distinction matters.

## Capability Modules

Use these as mixins, not full templates.

### Coding Agent

Add behavior for reading real source before editing, preserving user changes, running relevant validation, and asking before destructive actions or broad refactors.

### Research Agent

Add behavior for source discovery, evidence grading, citation requirements, uncertainty labels, and synthesis from conflicting sources.

### Support Agent

Add behavior for multi-turn context, minimal clarification, privacy, escalation, user-facing tone, and task completion state.

### RAG Agent

Add behavior for source priority, retrieved-context boundaries, citation grounding, missing-evidence handling, and two-stage evidence then answer workflows when structured output and citations conflict.

## Compactness Rules

- State each rule once in the section that owns it.
- Merge repeated rules that share the same decision criterion.
- Remove motivation, examples, or reminders that do not change measured behavior.
- Prefer a smaller prompt with clear ownership to a long prompt with repeated advice.
