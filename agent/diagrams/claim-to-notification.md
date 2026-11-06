# Claim-to-Notification Lifecycle Diagram

## Purpose

This is the recommended end-to-end design for team discussion. It follows candidate work through verification, admission, shared-context lifecycle, relevance analysis, and a targeted notification when human supervision may be required.

The Mermaid source is `src/claim-to-notification.mmd`.

## Provenance

- `DEC-005`: the evidence gate is fail-closed.
- `DEC-010`: important handoffs are durable, retryable, and idempotent.
- `DEC-011`: repository members may access verified context, while notifications are relevance- and threshold-gated.
- `DEC-013`: the immutable claim and its changing Shared Context lifecycle are separate.
- `contracts/change-manifest.md` and `contracts/evidence-and-verification.md` define the boundary artifacts.

## Stage 1 — Observe and verify

| Node | Responsibility |
|---|---|
| `WORK_OBSERVED` | Workspace detects candidate in-progress work. |
| `CHANGE_MANIFEST` | Workspace describes the work with metadata and evidence references. The manifest remains untrusted input. |
| `VERIFICATION_GATE` | Verification performs deterministic contract, identity, repository, evidence, freshness, policy, and idempotency checks. |
| `VERIFICATION_DECISION` | Durable result containing status, checks, evidence references, reasons, and explanation. |
| `REJECTED` | The submitted manifest fails a terminal rule; no admitted claim is produced. |
| `NEEDS_MORE_EVIDENCE` | The current submission cannot pass until additional evidence is supplied. |
| `ADMITTED_CLAIM` | Immutable verified projection produced only for a `verified` decision. |

Rejection and missing evidence never silently become success. Additional evidence results in an idempotent retry or a new versioned observation; it does not rewrite the earlier decision.

## Stage 2 — Store and manage lifecycle

Shared Context stores two related records:

1. The immutable admitted claim: what Verification proved at a specific time and repository state.
2. The lifecycle record: what happened to that claim afterward.

| Lifecycle state | Meaning |
|---|---|
| `ACTIVE` | Current verified in-progress context that may affect other workspaces. |
| `SUPERSEDED` | A newer verified claim replaces this version of the work. |
| `WITHDRAWN` | The owner explicitly abandons the work. |
| `EXPIRED` | The claim no longer satisfies repository freshness policy. |
| `INTEGRATED_PENDING_COVERAGE` | The work is verified in mainline, but at least one relevant workspace does not yet contain the integration revision. |
| `CONVERGED` | Every relevant workspace contains the verified integration revision. |
| `REVERTED` | Later verified mainline evidence shows that the integrated result was reverted. |
| `HISTORICAL_CONTEXT` | Inactive claims remain available for audit and provenance but are not treated as current work. |

A workspace is covered only when repository evidence proves that the integration revision is in its reported repository state. Running `git pull` by itself is not proof.

Repository members and their authorized agents may retrieve relevant verified context. Permission does not mean every claim is automatically inserted into every agent prompt.

## Stage 3 — Analyze and notify

`ACTIVE` claims and integrated claims awaiting workspace coverage are eligible for relevance analysis.

1. Conflict Analysis identifies affected workspaces and produces an evidence-backed risk assessment.
2. If no workspace is affected, the claim remains queryable without an interruption.
3. If a workspace is affected but risk is below repository policy, the finding remains context-only.
4. If risk meets or exceeds the threshold, Orchestration creates a targeted alert.
5. The Notification boundary delivers the alert to the affected user or agent.
6. Temporary delivery failures retain the alert and retry idempotently.
7. Acknowledgement, dismissal, coordination, or follow-up is recorded as an `AlertOutcome`.

Notification is a side effect of a relevant claim; it does not automatically close the claim. The lifecycle continues until the work is superseded, withdrawn, expired, converged, or reverted.

## Edge meanings

| From | To | Payload or control meaning |
|---|---|---|
| `WORK_OBSERVED` | `CHANGE_MANIFEST` | Observation or publish intent becomes a versioned manifest. |
| `CHANGE_MANIFEST` | `VERIFICATION_GATE` | Untrusted manifest and evidence references enter the evidence gate. |
| `VERIFICATION_GATE` | `VERIFICATION_DECISION` | Deterministic checks produce one structured decision. |
| `DECISION_STATUS` | `ADMITTED_CLAIM` | Only `verified` creates the immutable claim. |
| `ADMITTED_CLAIM` | `SHARED_CONTEXT` | Durable handoff of the verified claim. |
| `ACTIVE` | `INTEGRATED_PENDING_COVERAGE` | Verified repository evidence proves mainline integration. |
| `INTEGRATED_PENDING_COVERAGE` | `CONVERGED` | Repository evidence proves coverage by all relevant workspaces. |
| `ACTIVE` | `CONFLICT_ANALYSIS` | Current in-progress context is checked for impact. |
| `INTEGRATED_PENDING_COVERAGE` | `CONFLICT_ANALYSIS` | A behind workspace may still require supervision. |
| `RISK_MEETS_REPOSITORY_THRESHOLD` | `ORCHESTRATION` | Above-threshold risk becomes a targeted alert. |
| `ALERT` | `NOTIFICATION_BOUNDARY` | Durable delivery request. |
| `NOTIFICATION_BOUNDARY` | `DURABLE_RETRY` | A temporary failure retains the alert rather than losing it. |
| `NOTIFICATION_BOUNDARY` | `ALERT_OUTCOME` | Delivery and subsequent response are durably recorded. |

## Invariants

- A manifest is untrusted input; only `verified` produces an admitted claim.
- The admitted claim is immutable; lifecycle changes are stored separately.
- Lifecycle transitions require authorized deterministic policy or evidence-backed repository events.
- Only current or still-relevant claims participate in conflict analysis.
- Shared-context access and notification are different.
- Only an affected recipient whose risk meets the repository threshold receives an alert.
- Important handoffs and alert delivery are durable, retryable, and idempotent.
- Notifications do not silently change or close claims.
- Inactive claims remain available for audit and provenance.
- The removed evaluation layer is absent.
