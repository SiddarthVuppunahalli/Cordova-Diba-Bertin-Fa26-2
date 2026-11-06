# Shared System Architecture

## Purpose

This is the stable blueprint for the intended InSync system. It describes every final vertical equally and does not track which developer happens to implement one first.

## Architectural style

InSync is a centralized, service-oriented system organized as independent verticals over shared contracts:

```text
identify → verify → share → analyze and score → notify → optionally suggest
```

The evaluation layer has been removed.

## Vertical boundaries

### Workspace vertical

Observes in-progress repository work, performs producer-side checks, and submits a versioned `ChangeManifest` containing metadata and evidence references.

### Verification vertical

Owns the evidence gate. It validates provenance, repository facts, freshness, policy, and idempotency. It produces a `VerificationDecision` and, only on success, an `AdmittedClaim`.

### Shared Context component

This is provisionally a deterministic logical component rather than an AI agent. It persists immutable admitted claims and separately owns their changing lifecycle, repository visibility, provenance, and query behavior. It makes verified context queryable to registered repository members and their authorized agents for future work. It never promotes an unverified manifest or invents a lifecycle event on its own.

The initial boundary is a public port that can be backed by an in-memory implementation or explicit stub. Its eventual in-process or separately deployed form is intentionally unresolved.

### Conflict Analysis vertical

Compares admitted claims with relevant active-work context and returns structured findings grounded in evidence.

### Orchestration vertical

Coordinates handoffs between verticals, requests missing evidence, applies repository alert thresholds and routing policy, and records workflow state.

### Notification boundary

Delivers concise findings and evidence through replaceable adapters only to affected developers or coding agents whose risk meets the configured alert threshold. These alerts request attention or human supervision; they are not a broadcast feed of shared context.

## Access, retrieval, and notification

- Repository membership is the Version 1 access boundary for verified shared context.
- Registered repository members and agents authorized to act for them may query that context.
- Retrieval should select context relevant to the agent's current work; permission does not require injecting every available claim.
- Branch, component, and temporary-work metadata guide retrieval, analysis, routing, and lifecycle behavior rather than defining separate Version 1 authorization scopes.
- Conflict Analysis identifies affected workspaces and produces an evidence-backed risk assessment.
- Orchestration notifies an affected recipient only when that risk meets the repository's configured alert threshold.
- Below-threshold findings remain queryable shared context without interrupting the user.
- Raw unfinished source remains excluded unless a later explicit policy permits it.

### Adaptation vertical

After explicit human approval, prepares a proposed adaptation in isolation and records its diff and checks.

## Shared architecture artifacts

The following belong to the team and are safe to merge to `main`:

- Versioned boundary contracts
- Stable identifiers, statuses, and reason-code families
- Cross-cutting privacy, provenance, and authorization invariants
- Port ownership and vertical responsibility
- Overall diagrams and system terminology
- Reusable task and review protocol

Shared artifacts must not contain one developer's temporary progress, local fixture choices, personal task packets, or vertical-specific implementation notes.

## Vertical implementation rule

Each vertical owns its domain logic and adapters. It consumes upstream outputs only through shared contracts and exposes downstream outputs the same way. When another vertical is unfinished, the implementing developer uses a local stub or fake for that public contract.

Implementation proceeds in two distinct steps:

1. Prove the vertical's core behavior with the smallest useful runnable package, deterministic tests, fixtures, and explicit test doubles.
2. Add framework, persistence, transport, and neighboring-vertical adapters through ports after the core behavior works.

The shared architecture does not yet choose a final deployment topology. Verticals may later be composed in one process or placed behind process boundaries without changing their domain logic or public contracts.

## Handoff reliability

Cross-vertical handoffs are failure-tolerant rather than assumed to be failure-free.

- Important or non-reconstructible information is persisted before acknowledgement, identified by a stable idempotency key, and delivered with retries.
- The producer retains responsibility until the consumer durably acknowledges receipt.
- The consumer records receipt idempotently before performing or scheduling downstream work.
- Exhausted retries produce an explicit recoverable or operator-visible state; they are never silently discarded.
- No database transaction spans multiple verticals. Each vertical owns its local state and delivery obligation, using an outbox/inbox pair or an equivalent mechanism when integrated.
- Cheap observations may be best effort only when they are explicitly classified as safely reconstructible and a later observation recovers the required state.

For example, one missed periodic observation of a workspace diff may be acceptable when the next poll observes the current state. A requested publication, unique evidence, verification decision, admitted claim, finding, alert outcome, or human approval is durable.
