# Team-Wide Implementation Stages

This roadmap describes the complete system. It does not prescribe which developer must implement a vertical first or track personal branch progress.

## Stage A — Shared foundation

Establish the product model, architecture, cross-vertical contracts, diagrams, decision process, and task protocol.

Exit condition: every vertical has a clear responsibility and its neighboring boundaries are named.

## Stage B — Independent vertical development

Develop verticals against the shared contracts. A vertical first proves its core behavior with a minimal runnable package or demonstration script, deterministic tests, fixtures, and local test doubles for unfinished neighbors.

Framework, persistence, transport, and system-composition work are not required for this first proof. They are added afterward through the vertical's ports.

Parallel work may include:

- Workspace observation and local verification
- Central Verification Agent
- Shared Context persistence boundary
- Conflict Analysis and risk reasoning
- Orchestration and notification adapters
- Deferred human-approved adaptation

Exit condition: each implemented vertical demonstrates its core input-to-output behavior and passes its own contract tests without importing another vertical's internals.

## Stage C — Shared infrastructure integration

Connect the agreed persistence, transport, configuration, observability, and authorization adapters.

Exit condition: local test doubles can be replaced with real adapters through composition rather than domain rewrites.

## Stage D — Cross-vertical integration

Connect the pipeline from workspace observation through verification, shared context, analysis, orchestration, and notification.

Exit condition: an evidence-backed change can move through the complete flow, while invalid or irrelevant work stops at the correct boundary.

## Stage E — Product hardening

Harden privacy, restart behavior, idempotency, failure recovery, operator visibility, and developer experience. Add adaptation only after its approval and isolation rules are proven.

## Personal milestone tracking

Developers keep the detailed stages, proof commands, progress, and task packets for their current vertical under `.agent-local/<vertical>/`.
