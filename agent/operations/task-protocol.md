# Task Protocol

## Before work

1. Create a local task packet from `task-template.md` under `.agent-local/<vertical>/tasks/`.
2. Resolve every required decision or mark an allowed reversible assumption.
3. Assign disjoint owned files.
4. Define the proof command before implementation.
5. Identify every external vertical and the port or stub standing in for it.

## During work

- Stay inside owned files.
- Record contract or architecture conflicts immediately.
- Do not broaden a stub into hidden production behavior.
- Prefer small, reviewable changes that leave the repository runnable.

## Handoff

The builder reports:

- Files changed
- Behavior implemented
- Proof commands and results
- Assumptions
- Remaining risks or blockers
- Contract changes, if any

## Review

The reviewer checks behavior against the task packet and contracts. The reviewer returns `PASS` or findings with file locations and violated requirements. The builder fixes; the reviewer does not edit.

## Stop conditions

Stop and escalate when:

- A change requires an unresolved architecture decision.
- A task must modify files owned by another active builder.
- A contract cannot represent the required behavior.
- The proof command is unavailable or nondeterministic.
- Completion would require implementing a different vertical.

## Shared versus local records

- Reusable protocol and templates live under `agent/operations/`.
- Active tasks, handoffs, progress, and temporary implementation decisions live under `.agent-local/`.
- A local discovery that changes a shared contract must be proposed as a shared decision before implementation depends on it.
