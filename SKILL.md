---
name: multi-agent-architecture-review
description: Review multi-agent implementation code and determine whether Calling, Delegation, and Coordination patterns are correctly identified, necessary, and safely implemented. Use for repository reviews, pull requests, architecture assessments, agent runtime implementations, agent bus designs, and framework-neutral multi-agent code audits.
---

# Multi-Agent Architecture Review

## Objective

Analyze implementation code rather than architecture labels. Determine:

1. Which components are actual agents, tools, controllers, task managers, state stores, and message transports.
2. Which interaction pattern applies to each agent-to-agent edge.
3. Whether the use of multiple agents is justified.
4. Whether the implementation provides the controls required by the selected pattern.
5. Whether a simpler function, tool, workflow node, Calling edge, or Delegation edge would be preferable.

Do not infer architecture type from class names, filenames, prompts, or framework terminology alone. Base every conclusion on control flow, data flow, lifecycle, state ownership, permissions, and runtime behavior.

## Architecture vocabulary

### Calling

A caller invokes a bounded capability and retains control of the workflow.

Typical evidence:

- Explicit target selection
- Request-response interaction
- Caller controls when and why the target runs
- No independent long-running task lifecycle
- No ongoing negotiation
- Result returns to the caller for validation or continuation

### Delegation

A principal transfers a goal and limited autonomy to another agent.

Typical evidence:

- Goal-oriented task rather than a single operation
- Delegate chooses intermediate steps or tools
- Task identity and lifecycle
- Progress, completion, failure, or escalation states
- Resource, permission, time, or recursion limits
- Deliverable acceptance or rejection by the principal

### Coordination

Multiple agents have continuing, bidirectional dependencies around shared state, plans, resources, or decisions.

Typical evidence:

- More than one participant changes behavior based on another participant's output
- Shared or synchronized state
- Proposals, objections, approvals, revisions, arbitration, or dynamic replanning
- Concurrency or consistency handling
- Explicit convergence and termination rules

Parallel execution followed by aggregation is not Coordination unless the agents influence one another before final aggregation.

## Required review process

### Step 1: Establish review scope

Identify:

- Repository, module, pull request, or paths reviewed
- Languages and frameworks
- Whether runtime, infrastructure, or external services are outside the available code
- Any generated code or test fixtures that should not be treated as production design

### Step 2: Inventory architectural entities

Extract:

- Agents and agent-like executors
- LLM clients and model configurations
- Tools, skills, functions, and services
- Routers, supervisors, orchestrators, planners, schedulers, and coordinators
- Task, job, session, context, memory, event, and checkpoint objects
- State stores, queues, event buses, registries, and transports
- Human approval or policy enforcement points

For each agent-like component, capture:

- Identifier and code symbol
- Model and prompt configuration
- Tools and permissions
- State ownership
- Entry methods
- Lifecycle ownership
- Execution environment

### Step 3: Build the Agent Interaction Graph

Represent every relevant edge with:

- Source
- Target
- Trigger
- Payload type
- Response or event type
- Sync or async mode
- State read/write behavior
- Control owner before and after the interaction
- File, symbol, and line evidence

Classify each edge independently. A system may contain all three interaction patterns.

### Step 4: Detect pseudo multi-agent designs

Flag components as pseudo agents when they differ only by prompt or label and do not have a meaningful separation in one or more of:

- Tools
- Data access
- Permissions
- Model capability
- Execution environment
- State ownership
- Organizational responsibility
- Lifecycle
- Failure domain

Also flag:

- Simple deterministic logic implemented as an agent
- Formatting or validation steps implemented as full agents without justification
- Multiple identical agents sampled solely to vote without diversity or evidence controls
- Parallel fan-out/fan-in incorrectly described as peer coordination

### Step 5: Apply common rules

Load and apply `rules/common.md`.

### Step 6: Apply pattern-specific rules

For every edge:

- Calling: apply `rules/calling.md`
- Delegation: apply `rules/delegation.md`
- Coordination: apply `rules/coordination.md`

### Step 7: Evaluate simplification opportunities

For each agent and edge, test whether it can be safely reduced to:

1. Deterministic function
2. Tool or service
3. Workflow node
4. Calling
5. Delegation

Recommend Coordination only when continuing bidirectional dependency is proven by code or explicit runtime contracts.

### Step 8: Score and rate risk

Use a 100-point score:

- Common architecture quality: 40
- Pattern-specific implementation quality: 50
- Complexity proportionality: 10

Rating:

- 85-100: Reasonable, with production-grade foundations
- 70-84: Generally reasonable, remediation required
- 55-69: Functional but high architectural risk
- 40-54: Major over-design or missing controls
- 0-39: Architecture invalid or critically unsafe

Hard failures cap the result at `high` risk even if the numeric score is higher.

### Step 9: Produce evidence-grounded output

Use `schemas/assessment-output.schema.json` as the canonical output structure.

Every finding must include:

- Rule identifier
- Severity
- Conclusion
- Why it matters
- File
- Symbol
- Line range when available
- Evidence confidence
- Recommendation

Evidence confidence values:

- `direct`: the code explicitly proves the conclusion
- `strong_inference`: multiple code paths jointly support the conclusion
- `weak_inference`: based mainly on naming or partial evidence
- `unknown`: the reviewed scope does not contain enough evidence

Do not state that a capability is absent when it may be implemented outside the reviewed scope. Write `not found in the reviewed code` and identify the missing evidence.

## Mandatory hard-failure checks

Always check for:

- No timeout on remote or model-backed calls
- No structured validation before high-risk actions
- Retry of non-idempotent operations without idempotency controls
- Delegated long-running work without task identity or termination bounds
- Unlimited recursive delegation or child-agent creation
- Coordination loops without convergence limits
- Shared mutable state without consistency or concurrency control
- Unverified claims propagating into final decisions
- Irreversible actions executable by multiple agents without arbitration
- Missing traceability for actor, task, message, or decision origin

## Output expectations

The final review must contain:

1. Executive conclusion
2. Architecture inventory
3. Agent Interaction Graph summary
4. Per-edge classification with confidence
5. Design necessity assessment
6. Common-rule findings
7. Pattern-specific findings
8. Hard failures
9. Design smells
10. Simplification opportunities
11. Prioritized remediation plan
12. Unknowns and review limitations

Do not produce a generic architecture essay. The result must be anchored in the reviewed implementation.
