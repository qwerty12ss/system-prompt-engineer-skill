# Optimization Loop

Use this reference when revising an existing prompt or improving a draft based on observed behavior.

## Inputs

Collect the smallest useful evidence pack:

- current prompt or draft
- target model or runtime
- representative cases
- expected behavior or rubric
- observed failures or traces
- hard constraints: output shape, safety, tools, latency, cost, style

If examples or criteria are missing, create a small eval slice before rewriting.

## Loop

1. **Baseline**
   - Run or inspect the current prompt on the same representative cases.
   - Record prompt version, input, output, trace/tool behavior, score, and failure reason.

2. **Cluster**
   - Group issues by root cause:
     - ambiguity
     - missing constraint
     - conflicting rules
     - weak tool policy
     - weak output contract
     - bad or stale example
     - weak stop condition
     - provider mismatch
     - prompt bloat or duplicate ownership

3. **Choose edit strategy**
   - Minimal-diff repair when the prompt is mostly sound.
   - Structure-first rewrite when rules are contradictory or repeated across layers.
   - Examples-first variant when format or style is fragile.
   - Tool-rule variant when tool choice, evidence, or stop conditions are failing.
   - Provider adapter when the base prompt works but a model family diverges.

4. **Edit**
   - Change one or two causal dimensions per round.
   - Do not bundle unrelated improvements.
   - Prefer edits that are easy to compare against the baseline.

5. **Compare**
   - Use the same cases for before and after.
   - Track improvements, regressions, and unchanged failures.
   - Keep a low-risk candidate when trying a larger rewrite.

6. **Validate**
   - Replay original failures.
   - Test at least one holdout case.
   - Confirm happy-path behavior did not regress.

7. **Stop**
   - Stop when scores plateau, edits oscillate, the remaining issue is outside prompt control, cost rises without quality gain, or the prompt is overfitting the eval slice.

## Optimization Log

Use this short log when a prompt has meaningful iterations:

```markdown
| Round | Hypothesis | Edit | Evidence | Keep? |
|---|---|---|---|---|
| 1 | Tool policy lacked a stop condition | Added completion criteria to Tool Policy | Fewer extra tool calls, no format regression | Yes |
```

Record compaction and deletions too, not only additions.

## Return Package

For substantial prompt work, return:

1. Target and deployment context
2. Success criteria
3. Current diagnosis
4. Revised prompt
5. Eval slice or test cases
6. Main behavior changes
7. Residual risks or follow-up evals

For small edits, return the revised prompt and a brief note of the main change.
