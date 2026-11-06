# Agent Knowledge Map

This directory is the implementation-facing knowledge base for InSync. It is organized by the decisions an agent needs to make, not by workbook chapter.

## Source precedence

When sources disagree, use this order:

1. Explicit current user decisions in `context/decisions.md`
2. Accepted project decisions in `context/decisions.md`
3. Versioned contracts in `contracts/`
4. Architecture and workflow documents in `context/`
5. The archived Project Workbook, traced through `source-map.md`
6. Branch-local implementation material under `.agent-local/`

An open question is not permission to guess. Choose a reversible default only when the task packet permits it and record the assumption.

## Read by task

| Task | Required reading |
|---|---|
| Understand the product | `context/product.md`, `context/domain-model.md` |
| Change architecture | `context/decisions.md`, `context/system-architecture.md`, `diagrams/system-architecture.md` |
| Implement any vertical | `context/system-architecture.md`, `contracts/vertical-contracts.md`, then the matching `.agent-local/` plan |
| Implement verification | `contracts/change-manifest.md`, `contracts/evidence-and-verification.md`, then `.agent-local/verification/` |
| Implement a producer of manifests | `contracts/change-manifest.md`, `contracts/evidence-and-verification.md` |
| Implement a downstream consumer | `contracts/vertical-contracts.md` and the relevant detailed contract |
| Plan team-wide implementation | `operations/implementation-stages.md`, `operations/task-protocol.md` |
| Finalize the shared blueprint with the team | `operations/blueprint-finalization-agenda.md`, `context/open-questions.md`, `context/decisions.md` |
| Track personal or branch progress | `.agent-local/README.md` and the matching local vertical directory |
| Review work | `roles/reviewer.md`, the relevant contract, and the task packet |
| Add or change a diagram | `diagrams/README.md`, `diagrams/catalog.md` |

## Document classes

- `context/`: stable product knowledge and accepted decisions.
- `contracts/`: boundaries that independently implemented verticals must obey.
- `operations/`: team-wide delivery stages, task protocol, and reusable templates.
- `roles/`: build-time agent responsibilities. These are not InSync runtime agents.
- `diagrams/`: diagram-as-code and verified semantic companions.

## Runtime agents versus build agents

Runtime agents are parts of InSync: Workspace, Verification, Conflict Analysis, Orchestrator, and optional Adaptation verticals.

Build agents are the sessions implementing InSync: lead, builder, reviewer, and integrator. A runtime component does not automatically deserve a dedicated build agent.

## Local implementation layer

`.agent-local/` is optional and intentionally untracked. It is where an individual developer keeps the currently active vertical plan, progress, local decisions, diagrams, and task packets. Shared documents must not point to a local file as their only definition of a system contract.
