# InSync Agent Instructions

## Mission

Build InSync as an evidence-gated coordination system that discovers relevant in-progress repository changes before push or merge, verifies them, analyzes their impact, and sends targeted evidence-backed guidance.

This file governs the complete shared architecture. It must remain useful to every project vertical and must not be shaped around one developer's current milestone.

## Read first

1. `agent/README.md`
2. `agent/context/decisions.md`
3. `agent/context/system-architecture.md`
4. The task-specific contract under `agent/contracts/`
5. The active task packet under `.agent-local/`, when that local directory exists

Do not load every agent document by default. Use the routing table in `agent/README.md`.

## Non-negotiable invariants

- Only a verified claim may cross the evidence gate.
- Repository facts must come from deterministic checks against an identified repository state.
- Metadata and evidence references are shared by default; raw unfinished source is not.
- Every decision records its rule, inputs, evidence references, timestamp, and reason.
- Missing evidence produces an explicit rejection or `needs_more_evidence`; never invent facts.
- Agent verticals communicate through versioned contracts, not each other's internals.
- Dependencies on unfinished verticals use named ports and deterministic test doubles.
- A stub must be unmistakably a stub and must not silently behave like production infrastructure.
- Human approval is required before any suggested adaptation changes developer code.
- The evaluation layer has been removed from the architecture and must not be reintroduced.

## Implementation workflow

- Work from a task packet with a goal, owned files, forbidden files, proof command, and stop condition.
- Give concurrent writers disjoint file ownership.
- Prefer scripts and tests for deterministic checks; use an agent only when judgment is required.
- A builder does not approve its own completion. Use a fresh reviewer for contract and boundary checks.
- Update the active `.agent-local/<vertical>/progress.md` when a personal milestone changes state.
- Record new architectural decisions before changing contracts or component boundaries.

## Shared and local material

- `agent/` contains the stable team blueprint intended for `main`.
- `.agent-local/` contains branch-specific plans, progress, task packets, and temporary vertical details. It is intentionally ignored by Git.
- Local material may specialize the shared blueprint but may not silently redefine shared contracts or architecture.
