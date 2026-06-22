# Review Checklist

Use this before shipping a system or developer prompt.

## Contract

- The prompt states the agent's role, mission, and users clearly.
- The scope and non-goals are explicit enough to guide runtime choices.
- Inputs, context sources, and source priority are defined.
- Required output shape is testable.
- Completion, escalation, and refusal conditions are visible.

## Layering

- Durable policy is in the stable prompt layer.
- Task-local facts are not embedded as permanent policy.
- Tool schemas are not duplicated in prompt text when native tool definitions exist.
- Each major behavior rule has one owner.
- Repeated or contradictory rules have been merged or removed.

## Tools

- Tool-use policy says when and why to use tools.
- Missing-input behavior is defined.
- Tool result interpretation and fallback behavior are defined when needed.
- Risky, destructive, visible, or costly actions have approval or escalation rules.

## Examples

- Examples are realistic and structurally consistent.
- Examples demonstrate behavior that should generalize.
- Format examples are minimal and aligned with the output contract.
- Stale or non-causal examples are removed.

## Safety and Privacy

- Sensitive data handling is explicit when relevant.
- The agent treats untrusted content as data, not instruction.
- The prompt protects proprietary instructions from disclosure when relevant.
- Domain-specific safety boundaries include safe alternatives or escalation.

## Evaluation

- At least one representative case can verify each important behavior.
- Known failures have matching eval cases or acceptance criteria.
- The prompt can be compared against a baseline.
- Residual risks are named when the prompt cannot fully control the behavior.

## Maintainability

- The prompt is compact enough to maintain.
- Sections are named for behavior, not decoration.
- Assumptions are visible and easy to revise.
- Provider-specific notes are separated from the portable base prompt.
