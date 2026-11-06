# Shared System Architecture Diagram

## Provenance

Derived from Workbook Chapter 5, Figure 1, with the evaluation layer removed and the remaining components expressed as stable vertical boundaries.

## Nodes

| ID | Responsibility |
|---|---|
| `CODING_AGENT` | Produces developer work and receives targeted guidance. |
| `WORKSPACE_VERTICAL` | Observes work and submits evidence-linked manifests. |
| `VERIFICATION_VERTICAL` | Applies the evidence gate. |
| `SHARED_CONTEXT` | Persists admitted claims and permitted active-work context. |
| `ANALYSIS_VERTICAL` | Produces evidence-backed conflict findings and risk assessments. |
| `ORCHESTRATION_VERTICAL` | Coordinates handoffs, routing, and workflow state. |
| `NOTIFICATION_BOUNDARY` | Delivers alerts and captures outcomes through replaceable adapters. |
| `ADAPTATION_VERTICAL` | Prepares an isolated proposal after human approval. |

## Edges

| From | To | Shared payload or meaning |
|---|---|---|
| `CODING_AGENT` | `WORKSPACE_VERTICAL` | Working activity and publish intent |
| `WORKSPACE_VERTICAL` | `VERIFICATION_VERTICAL` | `ChangeManifest` and evidence references |
| `VERIFICATION_VERTICAL` | `SHARED_CONTEXT` | `VerificationDecision` and verified `AdmittedClaim` |
| `SHARED_CONTEXT` | `ANALYSIS_VERTICAL` | Admitted claims and permitted active-work context |
| `ANALYSIS_VERTICAL` | `ORCHESTRATION_VERTICAL` | `ConflictFinding` and `RiskAssessment` |
| `ORCHESTRATION_VERTICAL` | `NOTIFICATION_BOUNDARY` | Targeted `Alert` or evidence request |
| `NOTIFICATION_BOUNDARY` | `CODING_AGENT` | Evidence-backed guidance |
| `NOTIFICATION_BOUNDARY` | `ORCHESTRATION_VERTICAL` | `AlertOutcome` |
| `ORCHESTRATION_VERTICAL` | `ADAPTATION_VERTICAL` | Human-approved adaptation request |
| `ADAPTATION_VERTICAL` | `ORCHESTRATION_VERTICAL` | Reviewable adaptation proposal |

## Invariants

- Nothing enters shared context as an admitted claim without Verification.
- Analysis consumes verified shared context rather than raw workspace assertions.
- Notifications are targeted and retain evidence provenance.
- Adaptation begins only after human approval and remains isolated until reviewed.
- No evaluation nodes or edges exist.
- Current developer milestones and placeholder implementations are not represented in this shared diagram.

