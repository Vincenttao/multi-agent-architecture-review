# Coordination Review Rules

Coordination means multiple agents maintain continuing, bidirectional dependencies around shared state, plans, resources, or decisions.

## COORD-01 — Genuine coordination necessity

Require code or protocol evidence that one participant's output changes another participant's subsequent plan, state, constraints, or action.

Parallel execution followed by aggregation is fan-out/fan-in Calling or Delegation, not Coordination.

## COORD-02 — Effective participant diversity

Assess whether participants differ meaningfully in data, tools, models, permissions, execution environment, organizational authority, constraints, or failure domain.

Flag homogeneous replicas that use the same model, tools, context, and prompt template without a diversity rationale.

## COORD-03 — Shared-state model

Shared state should be typed, versioned, attributable, and governed.

Check:

- State schema
- Ownership
- Version or sequence number
- Read/write permissions
- History or event log
- Recovery and replay behavior

An unguarded shared dictionary or mutable global object is insufficient for concurrent coordination.

## COORD-04 — Concurrency and consistency

Inspect optimistic locking, compare-and-set, transactions, distributed locks, event ordering, duplicate handling, idempotency, and stale-write detection.

The required consistency level should match the business risk.

## COORD-05 — Semantic message types

Prefer explicit message semantics such as:

- Proposal
- Evidence
- Objection
- Approval
- Rejection
- Constraint update
- Decision
- Task reassignment

Flag a universal untyped text message when message semantics drive control flow.

## COORD-06 — Conflict detection and resolution

Check how the system detects and resolves conflicting facts, plans, resource claims, or decisions.

Possible mechanisms:

- Authority hierarchy
- Evidence-weighted arbitration
- Policy rules
- Quorum or voting
- Dedicated arbitrator
- Human escalation

Simple majority voting among homogeneous agents is weak evidence of correctness.

## COORD-07 — Convergence and termination

Coordination loops require explicit stop conditions, such as:

- Acceptance criteria satisfied
- No unresolved conflicts
- Required approval obtained
- Maximum rounds
- Maximum time, tokens, or cost
- No material information gain for a bounded number of rounds
- Final schema complete

No termination bound is a critical finding.

## COORD-08 — Claim provenance and verification

Facts and opinions should be distinguishable. Claims should preserve source agent, evidence references, and verification status.

Check that repeated citation by other agents does not convert an unverified claim into accepted truth.

## COORD-09 — Error containment

Inspect independent validation, quarantine of conflicted claims, rollback or compensation, participant isolation, and degraded operation when a participant fails.

## COORD-10 — Irreversible action arbitration

Multiple agents must not independently execute the same irreversible or high-risk action. Require a single authority, transaction protocol, reservation mechanism, or approval gate.

## Coordination hard failures

- No convergence or termination condition
- Shared mutable state without concurrency control
- No conflict-resolution mechanism
- No message deduplication for event-driven coordination
- Missing source or evidence provenance
- Unverified claims can directly drive final decisions
- Multiple agents can perform the same irreversible action
- Coordination framework used where agents never influence one another

## Coordination scoring — 50 points

- Necessity and participant diversity: 10
- Shared state and consistency: 12
- Message semantics and provenance: 8
- Conflict resolution and arbitration: 8
- Convergence and termination: 7
- Error containment and degraded operation: 5
