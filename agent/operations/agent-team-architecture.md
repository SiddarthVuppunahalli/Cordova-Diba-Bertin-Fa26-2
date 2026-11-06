# Build-Agent Architecture

This document governs the agents implementing InSync. It does not describe InSync runtime agents.

## Roles

- Lead: owns decomposition, contract integrity, sequencing, and synthesis.
- Builder: implements one bounded task in owned files.
- Reviewer: fresh read-only check against the contract and proof criteria.
- Integrator: runs cross-boundary checks and reports failures; deterministic scripts remain authoritative.

## Scaling rule

Start with one lead and one reviewer. Add builders only when:

- Work can start now without a missing decision.
- File ownership is disjoint.
- Each task has an independent proof command.
- The lead can review and integrate the returned work.

Use no more builders than the smallest of independent pieces, file ownership groups, executable checks, available review attention, and budget.

## Handoff card

Every delegated task includes:

- Goal
- Done looks like
- Inputs and authoritative sources
- Owned files
- Do not touch
- Proof command
- Expected output format
- Stop and escalation condition

## Completion

An agent report is not proof. Completion requires the task's deterministic proof plus a fresh review when judgment is involved.

