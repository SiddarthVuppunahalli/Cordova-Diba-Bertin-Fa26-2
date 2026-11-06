# Decision Log

## Status meanings

- `ACCEPTED`: implementation must follow it.
- `PROVISIONAL`: safe default, revisitable without invalidating product intent.
- `OPEN`: not decided.
- `SUPERSEDED`: retained for history but no longer active.

## Accepted

### DEC-001 — Evaluation layer removed

**Status:** ACCEPTED  
**Date:** 2026-11-05

The evaluation layer, replay harness, condition switch, test-runner layer, and evaluation metrics are not part of the current architecture.

### DEC-002 — Shared blueprint represents the final system

**Status:** ACCEPTED  
**Date:** 2026-11-05

The shared blueprint describes the complete intended InSync system. Current milestones, personal progress, and vertical-specific plans live under `.agent-local/` and do not redefine the shared architecture.

### DEC-003 — Runtime agents are independent verticals

**Status:** ACCEPTED  
**Date:** 2026-11-05

Each runtime agent owns its application logic and adapters behind explicit contracts. The overall architecture, identifiers, privacy invariants, and contracts are shared.

### DEC-004 — Unfinished collaborators use stubs

**Status:** ACCEPTED  
**Date:** 2026-11-05

When one vertical depends on an output or capability owned by an unfinished vertical, it depends on the shared public contract. Tests and development composition use deterministic local stubs or fakes for that boundary.

### DEC-005 — Evidence gate is fail-closed

**Status:** ACCEPTED

Only `verified` decisions emit admitted claims. Missing or contradictory evidence cannot be converted into success by a summary, attestation, or semantic model.

### DEC-006 — Metadata-first sharing

**Status:** ACCEPTED

Shared contracts contain metadata and evidence references by default. Raw unfinished source requires a separate explicit policy decision.

### DEC-007 — Workbook stack is the initial technical default

**Status:** PROVISIONAL

Python is the initial implementation language. FastAPI and PostgreSQL 16 with JSONB are provisional integration adapters, not prerequisites for proving a vertical's core behavior. A vertical may begin with a small runnable harness, fixtures, and in-memory adapters.

### DEC-008 — Contracts precede adapters

**Status:** ACCEPTED

Shared contracts and vertical domain types are defined before FastAPI, Git subprocess, and PostgreSQL adapters. Adapters must not leak framework-specific types across vertical boundaries.

### DEC-009 — Prove vertical cores before system integration

**Status:** ACCEPTED  
**Date:** 2026-11-05

Each vertical begins as a minimal, independently runnable and testable core. Its first proof may use a small package, a demonstration script, fixtures, and explicit test doubles. Framework, persistence, transport, and cross-vertical integration follow through ports and adapters after the core behavior works.

The final deployment topology is intentionally deferred. Public contracts must permit verticals to be composed in one process or placed behind process boundaries without rewriting their domain logic.

### DEC-010 — Handoff reliability follows information value and reconstructability

**Status:** ACCEPTED  
**Date:** 2026-11-05

Failures are expected at every vertical boundary. Information that is important, costly, or impossible to reconstruct must be durably recorded before its handoff is acknowledged. Its delivery is retryable, consumers are idempotent, and exhausted retries become an explicit recoverable or operator-visible state rather than a silent loss.

No transaction spans multiple verticals. A producer owns its local output and delivery obligation until the consumer durably acknowledges receipt. Each consumer then owns its local state and any next delivery obligation. An outbox/inbox pair or an equivalent mechanism is the default integration pattern for durable handoffs.

Cheap, safely reconstructible observations may use best-effort delivery when a later observation recovers the needed state. This exception must be explicit. It does not apply to requested publications, verification decisions, admitted claims, findings, alerts and outcomes, approvals, adaptation proposals, or unique evidence.

### DEC-011 — Repository membership controls context access; risk controls interruption

**Status:** ACCEPTED  
**Date:** 2026-11-06

All registered users with access to a repository, and agents authorized to act for those users, may access that repository's verified shared context. Access does not imply that every claim is broadcast or automatically inserted into every agent prompt; consumers retrieve the permitted context relevant to their work.

Notifications are narrower than shared-context access. Only affected users or workspaces receive an alert, and only when an evidence-backed risk assessment meets the repository's configured alert threshold. Findings below the threshold remain available as shared context without interrupting the user. An alert is an escalation for possible human supervision, not the mechanism by which shared context becomes accessible.

In Version 1, repository membership is the authorization boundary. Branch, component, and temporary-work attributes support relevance, routing, and lifecycle decisions rather than creating separate access-control scopes. The metadata-first rule still applies; repository membership alone does not grant access to unfinished raw source.

### DEC-012 — Identity authorities and artifact producers own their identifiers

**Status:** ACCEPTED  
**Date:** 2026-11-06

The repository registration and identity boundary issues stable repository, workspace, user, and producer identities. Each vertical generates the identifiers and idempotency keys for artifacts it creates. Consumers validate those identifiers within their declared scope, preserve upstream identifiers for traceability, and do not replace a valid producer identifier with a consumer-specific identity.

Identifier ownership follows artifact ownership: Workspace owns manifest and evidence identifiers; Verification owns decision and admitted-claim identifiers; Conflict Analysis owns finding and risk-assessment identifiers; Orchestration owns alert and approved-adaptation-request identifiers; Notification owns alert-outcome identifiers; and Adaptation owns proposal identifiers. Transport envelopes may add a delivery identifier without replacing the domain artifact identifier.

### DEC-013 — Shared Context provisionally owns post-admission state

**Status:** PROVISIONAL  
**Date:** 2026-11-06

Verification produces an immutable evidence-backed `AdmittedClaim`. A separate Shared Context logical component stores admitted claims and owns their changing post-admission lifecycle, repository-scoped access, and context-query behavior. It updates lifecycle only from authorized, evidence-backed events or deterministic policy; it does not infer repository facts with an AI model.

For initial development, Shared Context is a deterministic module behind a public port and may be represented by an explicit stub while another vertical is proved. Whether it later remains an in-process module, becomes a separately deployed service, or gains any narrowly scoped agent capability remains a team decision.
