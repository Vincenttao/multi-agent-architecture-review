# Multi-Agent Architecture Review

An evidence-based AI skill for reviewing whether multi-agent implementation code is architecturally justified and correctly implemented.

The skill classifies each interaction between agents as one of three patterns:

- **Calling** — a caller invokes a bounded capability and retains control.
- **Delegation** — a principal transfers a goal and limited autonomy to another agent.
- **Coordination** — multiple agents maintain bidirectional dependencies around shared state or decisions.

It then evaluates whether the selected pattern is necessary, whether a simpler design would be preferable, and whether the implementation contains the controls required for production use.

## What the skill produces

- Agent and control-component inventory
- Agent Interaction Graph
- Per-edge Calling / Delegation / Coordination classification
- Architecture necessity assessment
- Pattern-specific compliance findings
- Hard-failure and design-smell detection
- Evidence references to files, symbols, and line ranges
- Risk rating, score, and prioritized remediation plan

## Repository structure

```text
.
├── SKILL.md
├── rules/
│   ├── common.md
│   ├── calling.md
│   ├── delegation.md
│   └── coordination.md
├── schemas/
│   └── assessment-output.schema.json
└── examples/
    └── example-assessment.md
```

## Usage

Ask a coding agent to load `SKILL.md` and review a repository, module, pull request, or architecture implementation.

Example request:

```text
Use the multi-agent architecture review skill to inspect this repository.
Identify all agent interactions, classify each edge as Calling, Delegation,
or Coordination, and determine whether the design is justified by code evidence.
```

## Core principle

The skill does not reward a system merely for using a multi-agent framework. It requires evidence that multiple autonomous components provide a capability, ownership, permission, data, execution-environment, or concurrency boundary that cannot be achieved more simply.

## Status

Initial specification. The rules are framework-neutral and can be applied to Python, Java, TypeScript, or mixed-language agent systems.
