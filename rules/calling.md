# Calling Review Rules

Calling means a caller invokes a bounded capability and retains end-to-end control.

## CALL-01 — Explicit target or auditable routing

The target must be explicit or selected by an auditable router.

If model-based routing is used, check:

- Bounded candidate set
- Capability metadata
- Confidence or fallback handling
- Route traceability
- Rejection of unsupported tasks

## CALL-02 — Bounded request-response contract

The interaction should have a clear request, response, and completion boundary.

Flag hidden autonomous loops, indefinite callbacks, or lifecycle behavior disguised as a simple call.

## CALL-03 — Minimal request context

The request should contain only the information needed by the target capability.

Flag full context forwarding when a typed request would be sufficient.

## CALL-04 — Response validation

The caller must validate the result before using it.

Recommended layers:

1. Parse and schema validation
2. Business invariant validation
3. Evidence, confidence, or provenance checks
4. Authorization before side effects

A successful transport response is not equivalent to a valid task result.

## CALL-05 — Timeout and degradation

Check explicit timeout, bounded retry, circuit breaking where relevant, and fallback or controlled failure.

## CALL-06 — Side-effect safety

Calls that mutate external state require idempotency, duplicate suppression, or transaction controls.

## CALL-07 — Agent-versus-tool justification

If the target has no autonomous planning, lifecycle, or meaningful capability boundary, recommend implementing it as a tool or service rather than an agent.

## Calling hard failures

- No timeout on remote/model-backed invocation
- Free-text response directly triggers high-risk action
- Retry can duplicate side effects
- Target identity cannot be traced
- No request/message identifier for distributed calls
- No error handling around the invocation

## Calling scoring — 50 points

- Targeting and routing: 10
- Request and response contract: 10
- Result validation: 12
- Timeout, retry, and degradation: 8
- Permission and context isolation: 5
- Agent-versus-tool proportionality: 5
