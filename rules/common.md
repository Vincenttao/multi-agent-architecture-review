# Common Review Rules

Apply these rules to every multi-agent implementation before pattern-specific checks.

## COM-01 — Multi-agent necessity

**Question:** Does each agent boundary represent a meaningful separation?

Acceptable evidence includes different tools, data, permissions, models, execution environments, organizational ownership, state ownership, or failure domains.

Flag when agents differ only by role prompt or display name.

## COM-02 — Simpler primitive test

For every agent, test whether the behavior can be implemented more safely as a deterministic function, tool, service, validation stage, or workflow node.

Flag an agent when all are true:

- Stable input and output
- No autonomous planning requirement
- No independent lifecycle
- No distinct permission or execution boundary
- Limited semantic uncertainty

## COM-03 — Structured contracts

Agent boundaries should use typed or validated request and response contracts.

Check:

- Required fields
- Enum and type validation
- Schema version
- Parse failure behavior
- Business invariant validation
- Compatibility handling

Flag substring matching or unvalidated free text that controls business actions.

## COM-04 — Failure containment

Check that failures are classified, contained, and surfaced.

Required evidence may include:

- Structured error types
- Retryability classification
- Partial-success handling
- Fallback behavior
- Failure isolation
- User- or supervisor-visible status

## COM-05 — Timeout, retry, and idempotency

Remote, model, tool, and agent calls require explicit time bounds.

Retries must include:

- Maximum attempts
- Backoff or delay strategy
- Retryable-error filtering
- Idempotency protection for side effects

A retry around payments, messages, writes, approvals, or external mutations without an idempotency key is a critical finding.

## COM-06 — Context minimization

Check whether only task-relevant context is shared.

Flag:

- Full conversation history sent by default
- Sensitive fields sent to agents without need
- Credentials or secrets in prompts or messages
- Shared global memory without access controls

## COM-07 — Least privilege

Review model and tool permissions per agent.

Check:

- Allowed tools
- Allowed resources or domains
- Read versus write permissions
- Approval gates
- Credential scope
- Identity propagation

## COM-08 — Observability and provenance

Production interactions should expose, where applicable:

- Trace ID
- Task or session ID
- Source and target
- Message or request ID
- Start and end time
- Status and error code
- Model and tool usage
- Token or resource usage
- Decision and evidence provenance

## COM-09 — Test coverage

Look for tests covering:

- Agent-level behavior
- Interaction contracts
- Invalid model output
- Timeout and retry
- Partial failure
- Concurrency
- Cancellation
- Termination conditions
- Permission boundaries
- Idempotency

Do not treat prompt snapshot tests alone as sufficient.

## COM-10 — Evidence-grounded review

Every finding must cite code evidence. When infrastructure or runtime code is unavailable, record the capability as `unknown` rather than absent.

## Common scoring — 40 points

- Necessity and boundary justification: 8
- Capability and responsibility clarity: 8
- Structured contracts and validation: 8
- Failure handling and recovery: 6
- Permission and data isolation: 5
- Observability and testing: 5
