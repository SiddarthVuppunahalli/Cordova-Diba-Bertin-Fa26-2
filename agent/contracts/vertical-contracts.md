# Shared Vertical Contracts

This document identifies what crosses each final system boundary. Detailed field definitions live in dedicated contract documents.

| Producer | Consumer | Shared contract | Purpose |
|---|---|---|---|
| Workspace | Verification | `ChangeManifest` | Report an observed change and its evidence references without asserting central trust. |
| Verification | Shared Context | `VerificationDecision`, `AdmittedClaim` | Record the evidence-gate result and publish only the verified projection. |
| Shared Context | Conflict Analysis | `AdmittedClaim`, active-work query | Supply verified changes and permitted workspace context. |
| Conflict Analysis | Orchestration | `ConflictFinding`, `RiskAssessment` | Describe supported impact, uncertainty, affected workspace, and evidence. |
| Orchestration | Notification | `Alert` | Deliver a targeted explanation and next step. |
| Notification | Orchestration | `AlertOutcome` | Record acknowledgement, dismissal, coordination, testing, or follow-up. |
| Orchestration | Adaptation | `ApprovedAdaptationRequest` | Request isolated preparation only after human approval. |
| Adaptation | Orchestration | `AdaptationProposal` | Return a proposed diff, commands, checks, and limitations. |

## Contract rules

- Producers may not expose internal domain objects across a boundary.
- Consumers validate the supported contract version before use.
- Evidence and provenance identifiers survive every downstream transformation.
- A downstream artifact identifies the upstream artifacts from which it was derived.
- Unfinished consumers are replaced by local stubs implementing the same public contract.
- Shared contracts contain no developer-specific fixture paths or branch progress.
- Every handoff declares whether it is durable or safely reconstructible; durable is the default when loss could change a decision or erase unique evidence.
- Durable handoffs have a stable message or idempotency identifier, retry semantics, and an explicit terminal or operator-visible failure state.
- A producer retains a durable item until the consumer acknowledges durable, idempotent receipt.
- Delivery is at least once; consumers must make repeated delivery produce one logical result.
- The vertical that creates a domain artifact generates its identifier and idempotency key; consumers preserve and validate them.
- A transport delivery identifier supplements but never replaces the domain artifact identifier.

## Current detailed contracts

- `change-manifest.md`
- `evidence-and-verification.md`

Contracts for findings, alerts, outcomes, and adaptation proposals should be added to this directory when their owning verticals agree on the boundary.
