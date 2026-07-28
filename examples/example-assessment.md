# Example Assessment

This example illustrates the expected reasoning style. File names and line numbers are fictional.

## Executive conclusion

The reviewed system primarily implements **Delegation** from `Supervisor` to `ResearchAgent`, with a separate **Calling** edge from `ResearchAgent` to `RiskValidator`. The design has a valid capability boundary because the research worker owns external retrieval tools, but it lacks bounded autonomy, cancellation, and explicit deliverable acceptance.

- Score: 64/100
- Risk: High
- Primary pattern: Delegation
- Recommended target: Controlled Delegation

## Interaction graph

```text
Supervisor
   |
   | Delegation: ResearchTask
   v
ResearchAgent
   |
   | Calling: RiskReport validation
   v
RiskValidator
```

## Edge classification

### EDGE-01: Supervisor -> ResearchAgent

- Classification: Delegation
- Confidence: 0.94
- Evidence:
  - `src/supervisor.py:Supervisor.start_research`, lines 71-94
  - The supervisor passes an objective and permits the worker to choose tools and intermediate steps.
  - The worker runs asynchronously and returns a report after multiple tool calls.

The edge is not Calling because the target owns a multi-step task rather than a bounded operation.

### EDGE-02: ResearchAgent -> RiskValidator

- Classification: Calling
- Confidence: 0.91
- Evidence:
  - `src/research_agent.py:ResearchAgent.finalize`, lines 143-151
  - A typed `RiskReport` is sent to a fixed validator and a `ValidationResult` is returned synchronously.

## Findings

### DEL-01 — Incomplete task contract

**Severity:** High

**Conclusion:** The delegated task contains an objective but no explicit scope, exclusions, deadline, allowed tools, or acceptance conditions.

**Evidence:**

- File: `src/models.py`
- Symbol: `ResearchTask`
- Lines: 12-19
- Confidence: Direct
- Summary: The model defines only `task_id` and `objective`.

**Why it matters:** The worker can expand the task indefinitely and the supervisor cannot determine whether the result is complete.

**Recommendation:** Add scope, constraints, deadline, allowed tools, deliverable schema, and acceptance criteria.

### DEL-03 — Unbounded autonomy

**Severity:** Critical

**Conclusion:** No maximum step, tool-call, runtime, or child-task limit was found in the reviewed code.

**Evidence:**

- File: `src/research_agent.py`
- Symbol: `ResearchAgent.run`
- Lines: 52-119
- Confidence: Strong inference
- Summary: The loop exits only when the model emits `done`; no independent bound is applied.

**Why it matters:** A model failure can create an unbounded loop, excessive cost, or repeated tool use.

**Recommendation:** Enforce `max_steps`, `max_tool_calls`, deadline, and cancellation independently of model output.

### DEL-07 — Deliverable is not accepted or rejected

**Severity:** High

**Conclusion:** The supervisor returns the worker's report directly without checking the declared business requirements.

**Evidence:**

- File: `src/supervisor.py`
- Symbol: `Supervisor.start_research`
- Lines: 91-94
- Confidence: Direct
- Summary: The awaited result is returned unchanged.

**Why it matters:** Transport completion is treated as task success.

**Recommendation:** Add schema validation, evidence checks, acceptance criteria, and a revision or rejection path.

### CALL-04 — Validator call is correctly bounded

**Severity:** Info

**Conclusion:** The validator edge uses typed input/output and the caller checks `passed` before proceeding.

**Evidence:**

- File: `src/research_agent.py`
- Symbol: `ResearchAgent.finalize`
- Lines: 143-158
- Confidence: Direct

**Recommendation:** Retain this pattern and add explicit timeout telemetry.

## Design smell

### Pseudo reviewer agent

`NarrativeReviewer` uses the same model, tools, permissions, and context as `ResearchAgent`; it only changes the system prompt.

Recommendation: implement this as a self-review workflow stage, or introduce a genuinely independent validator with separate evidence rules or model configuration.

## Remediation plan

### P0

- Add independent loop and resource bounds.
- Add cancellation and deadline enforcement.

### P1

- Expand the task contract.
- Add deliverable acceptance and revision handling.

### P2

- Replace the pseudo reviewer agent with a workflow validation stage.
- Add timeout, cost, and tool-call telemetry.

## Limitations

The reviewed repository does not include the production runtime implementation. Distributed task persistence, authentication, and cancellation may exist outside the reviewed code and should be verified before final deployment approval.
