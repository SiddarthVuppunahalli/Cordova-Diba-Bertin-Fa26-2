# Shared System Requirements

This document describes the final system outcome. Detailed acceptance criteria for one developer's active vertical belong under `.agent-local/`.

## System invariants

| ID | Requirement |
|---|---|
| SYS-001 | A claim must not enter shared context unless the Verification vertical returns `verified`. |
| SYS-002 | Every admitted claim and downstream finding must remain traceable to repository evidence. |
| SYS-003 | Unfinished source is not included in shared payloads by default. |
| SYS-004 | Vertical boundaries use versioned contracts and do not depend on another vertical's internal models. |
| SYS-005 | An unavailable vertical is represented through its public contract and an explicit test double. |
| SYS-006 | Rejection and uncertainty are recorded rather than silently discarded. |
| SYS-007 | Verified shared context is accessible to registered members of its repository and agents authorized to act for them. |
| SYS-008 | Suggested code adaptation requires human approval. |
| SYS-009 | Important or non-reconstructible information is durably recorded before handoff acknowledgement and remains retryable until durably received. |
| SYS-010 | Duplicate delivery must not duplicate the logical result; durable consumers process stable identifiers idempotently. |
| SYS-011 | Best-effort delivery is permitted only for explicitly classified observations that are cheap and safe to reconstruct. |
| SYS-012 | Failed durable delivery becomes an explicit recoverable or operator-visible state and is never silently discarded. |
| SYS-013 | Only affected recipients whose evidence-backed risk meets the repository alert threshold are notified. |
| SYS-014 | Below-threshold findings remain available as shared context without interrupting users. |

## Vertical responsibilities

### Workspace

- Capture uncommitted changes, unpushed commits, and requested scans.
- Attach repository, workspace, branch, revision, timestamp, and evidence references.
- Perform producer-side checks without claiming central admission.

### Verification

- Validate identity, provenance, freshness, evidence, visibility, and idempotency.
- Return a structured decision with stable rules and reasons.
- Emit an admitted claim only after every required gate passes.

### Shared Context

- Persist immutable admitted claims and their provenance.
- Maintain changing lifecycle state separately from the verified claim.
- Preserve supersession, visibility, and contract version information without rewriting the claim's verified facts.
- Prevent unverified manifests from appearing as admitted claims.
- Authorize queries by repository membership and return permitted context for future agent work.
- Apply lifecycle changes only from authorized evidence-backed events or deterministic policy.

### Conflict Analysis

- Consume verified claims and relevant active-work context.
- Produce evidence-backed findings for overlap, contract divergence, dependency impact, or integration risk.
- Return explicit `none` or `needs_more_evidence` outcomes when evidence is insufficient.

### Orchestration and Notification

- Compare evidence-backed risk with repository alert policy.
- Route above-threshold findings only to affected workspaces or users.
- Attach evidence and an actionable explanation.
- Record acknowledgement, dismissal, coordination, or follow-up action.

### Adaptation

- Operate only after human approval.
- Prepare changes in isolation and record the resulting diff and checks.
- Never silently modify a developer workspace.

## Shared quality requirements

- Deterministic repository facts are authoritative.
- External dependencies are accessed through named ports.
- Commands run with bounded working directories, timeouts, and captured results.
- Contracts, reason codes, and identifiers are versioned.
- Vertical implementations remain replaceable without redesigning the overall architecture.
- Each vertical owns local transactions; the system does not require a transaction spanning multiple verticals.
