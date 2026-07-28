# Delegation Review Rules

Delegation means a principal transfers a goal and bounded autonomy to another agent while retaining accountability and acceptance authority.

## DEL-01 — Explicit task contract

A delegated task should define:

- Task identity
- Objective
- Scope and exclusions
- Constraints
- Deliverable schema
- Deadline or time bound
- Allowed tools and resources
- Acceptance conditions
- Escalation conditions

Flag vague instructions such as `handle this` or `do what is needed` without enforceable boundaries.

## DEL-02 — Task lifecycle

Long-running delegated work should expose an explicit lifecycle, such as:

- CREATED
- ACCEPTED
- RUNNING
- WAITING
- ESCALATED
- COMPLETED
- FAILED
- CANCELLED

Check that state transitions are valid, persisted when necessary, and observable.

## DEL-03 — Bounded autonomy and resources

Check limits for:

- Maximum steps
- Tool calls
- Tokens or cost
- Runtime
- Child tasks
- Recursion depth
- Data and network access
- Side-effect permissions

Unlimited delegation or recursive child-agent creation is a critical finding.

## DEL-04 — Cancellation and termination

The principal or runtime must be able to stop work.

Look for cancellation tokens, task cancellation APIs, deadline enforcement, cooperative interruption, and cleanup logic.

## DEL-05 — Progress, heartbeat, and checkpoints

For asynchronous or long-duration work, inspect:

- Progress events
- Last-updated timestamps
- Heartbeats
- Intermediate results
- Checkpoints
- Resume behavior

## DEL-06 — Escalation and approval

The delegate should escalate when encountering insufficient evidence, conflicting evidence, permission limitations, high-risk action, budget exhaustion, or inability to meet acceptance conditions.

## DEL-07 — Deliverable validation and acceptance

The principal or an independent validator must assess the deliverable against explicit criteria.

Check revision, rejection, or human-review paths. Returning the delegate's final text directly is not sufficient acceptance.

## DEL-08 — Accountability and provenance

Delegation must preserve principal, delegate, task, decision, and action provenance. Delegating execution does not transfer final accountability invisibly.

## Delegation hard failures

- No task identity for asynchronous or long-running work
- No lifecycle or status model
- No budget or termination bound
- No cancellation path
- No deliverable validation
- Unlimited recursive delegation
- High-risk actions without approval or policy check
- Failure requires full restart despite a stated resumability requirement

## Delegation scoring — 50 points

- Task contract and scope: 10
- Lifecycle and state management: 10
- Resource, permission, and autonomy limits: 10
- Cancellation, recovery, and progress: 8
- Escalation and approval: 5
- Deliverable validation and accountability: 7
