# Eval and Failure Analysis

Use this reference when a prompt is unreliable, when the user reports bad behavior, or when you need a lightweight eval plan.

## Eval Slice

Start small:

- 5 to 10 representative cases for a quick pass
- include common cases, edge cases, ambiguous inputs, and one holdout
- define expected behavior as acceptance criteria, not vibes
- store or present cases in a repeatable shape

For each case, capture:

```markdown
ID:
Input:
Expected behavior:
Must include:
Must avoid or escalate:
Pass criteria:
Failure notes:
```

## Scoring Dimensions

Choose only dimensions that matter for the task:

- instruction following
- output shape
- tool behavior
- evidence handling
- refusal or escalation correctness
- ambiguity handling
- brevity or detail level
- safety and privacy handling
- latency, cost, or tool-call efficiency

Use pass/fail for crisp requirements and 1-5 scoring for qualitative criteria.

## Failure Taxonomy

Map failures to prompt ownership before editing.

| Failure | Likely owner | Typical fix |
|---|---|---|
| Agent guesses instead of checking | Tool policy, context boundaries | Define when to gather evidence and when to answer from context |
| Agent over-asks | Clarification policy, workflow | Define when to infer, proceed, or ask |
| Agent acts too aggressively | Scope, safety, workflow | Add approval gates or stop conditions |
| Wrong tool choice | Tool policy | Strengthen tool criteria and priority |
| Output format drifts | Output contract | Add schema, sections, or a minimal format example |
| Verbose response | Output contract | Tighten length and section rules |
| Hallucinated facts | Evidence policy | Add source priority and uncertainty behavior |
| Contradictory behavior | Layer ownership | Remove duplicates and choose one owner |
| Fails after model change | Provider assumptions | Add adapter notes or simplify model-specific phrasing |

## Pass@K Interpretation

Use pass@k only when you can run repeated samples:

- pass@1 high: reliable first-try behavior
- pass@1 lower than pass@3: the model can do it but is inconsistent
- pass@3 or pass@5 at zero: likely missing rule, wrong tool policy, or unsupported capability

## Edit Principles

- Fix high-impact clusters first.
- Prefer precise decision criteria over emphasis.
- Add examples only when they improve format, style, or edge-case behavior.
- Tighten tool ordering before adding broad tool mandates.
- Remove unnecessary text when it dilutes important rules.
- Revert or revise any edit that improves one case but regresses previously passing cases.

## Lightweight Verification Prompt

When no automated runner exists, ask the model or evaluator to score outputs with a compact rubric:

```text
Evaluate the candidate response against the criteria below.
Return JSON with `pass`, `scores`, and `issues`.

Criteria:
- Instruction following:
- Output shape:
- Evidence/tool behavior:
- Safety/escalation:
- Brevity/detail:
```

Use the same rubric for baseline and candidate prompts.
