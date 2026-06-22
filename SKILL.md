---
name: system-prompt-engineer
description: Use when writing, reviewing, revising, porting, or evaluating system/developer prompts for AI agents, especially prompts that need clear roles, task boundaries, tool-use policy, output contracts, safety behavior, compact deployable structure, or eval-driven refinement. Also use when the user reports that an agent prompt produces unreliable behavior and wants a cleaner or more testable prompt.
---

# System Prompt Engineer

Create and improve deployable prompts for agentic systems. Treat prompts as runtime artifacts: concise, layered, testable, and tied to observable behavior.

## Core Workflow

1. Identify the work type: `draft`, `review`, `revise`, `port`, or `evaluate`.
2. Capture the prompt contract before editing:
   - target model or family, if known
   - prompt surface: system, developer, user template, tool descriptions, examples, schema, or project instruction file
   - agent role, users, primary tasks, tools, inputs, outputs, safety boundaries, and success criteria
   - known failures, constraints, latency/cost sensitivity, and deployment context
3. Decide whether to draft now or ask one focused question. Ask only when the agent domain and primary task cannot be inferred.
4. Build or revise using layered prompt architecture.
5. For existing prompts, preserve intended behavior, remove non-causal text, and change one or two causal dimensions at a time.
6. Return a deployable prompt by default. Add analysis, diff notes, or eval plans only when requested or useful for review.

## Read Only What You Need

| Situation | Read |
|---|---|
| Drafting a new system/developer prompt | `references/system-prompt-architecture.md`, `assets/system-prompt-template.md` |
| Revising an existing prompt | `references/optimization-loop.md`, `references/system-prompt-architecture.md` |
| Reported failures or inconsistent behavior | `references/eval-and-failure-analysis.md`, `references/optimization-loop.md` |
| Reviewing prompt quality before shipping | `references/review-checklist.md` |
| Tool-using, RAG, support, research, or coding agents | `references/system-prompt-architecture.md` and only the relevant module guidance inside it |

## Drafting Standard

- Put durable policy in the stable prompt layer; put task-local facts and retrieved context in user payloads or runtime context.
- Give each major behavior rule one authoritative owner.
- Keep persona light unless it changes behavior.
- Use headings or tags to separate content types, not to decorate every sentence.
- Keep tool schemas in provider-native tool definitions when available; prompt text should describe when, why, and whether to use tools.
- Prefer exact external file paths over vague references when repo docs, policies, or specs matter.
- Include assumptions only when missing details affect behavior; label them so the user can correct them.
- Make the final prompt compact enough to be maintained, but explicit enough to test.

## Revision Standard

For an existing prompt:

1. Baseline the current behavior from examples, evals, traces, or the user's observed failures.
2. Cluster issues by root cause before editing.
3. Identify the prompt layer that owns each issue.
4. Choose the smallest defensible edit or a structure-first rewrite when the prompt is already tangled.
5. Compare against the same cases after editing.
6. Keep an optimization note when meaningful: hypothesis, edit, evidence, residual risk.

Do not treat prompt length as quality. A revision is better only when it makes the target behavior clearer, more reliable, easier to validate, or easier to maintain.

## Output Modes

Default to the mode implied by the user:

- `prompt_only`: return only the deployable prompt.
- `review_then_prompt`: concise findings followed by the revised prompt.
- `package`: target, success criteria, prompt, eval cases, adapter notes, and residual risks.
- `diagnostic`: root-cause analysis and proposed edits without rewriting yet.

If the user asks for a ready-to-paste prompt, use `prompt_only`.
If the user asks whether a prompt is good, use `review_then_prompt`.
If the user asks to optimize with failures or evals, use `package`.

## Source Lineage

This skill synthesizes practices from local raw artifacts captured from:

- `getsentry/skills` prompt optimizer: contract capture, layer ownership, prompt shaping, optimization packages
- `CR-730/agent-system-prompt-architect-skill`: deployable system prompt architecture, runtime separation, compactness, tool-aware output contracts
- `bergr7/claude-skill-prompt-optimizer`: baseline-diagnose-edit-validate discipline and budgeted eval loops

Use the practices, not upstream wording. When legal or provenance details matter, inspect the raw task artifacts or source repositories directly.
