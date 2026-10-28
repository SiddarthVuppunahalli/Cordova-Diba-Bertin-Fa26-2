# InSync — Initial Build Plan

## Purpose

This is the initial agentic skeleton plan. It separates stable work that can begin immediately from Droid runtime details and scenario choices that remain dependent on the Factory discussion or final demo scenario.

## Status labels

- **[READY]** — start now.
- **[IN PROGRESS]** — currently being implemented.
- **[DROID-BLOCKED]** — wait for Factory answers.
- **[SCENARIO-BLOCKED]** — wait for the final realistic demo example.
- **[DEFERRED]** — do not start during the first pass.

## Phase 0: Documentation and decisions

### Goal

Create a shared understanding of the product before implementation expands.

### Tasks

- [x] Write the project vision.
- [x] Record the first locked and open decisions.
- [x] Prepare the Droid sponsor brief.
- [ ] Ask the Factory team the integration questions.
- [ ] Update decisions based on the sponsor discussion.

### Exit condition

The project has a stable core and a documented list of Droid-dependent choices.

## Phase 1: Repository fixture and workspaces — [READY]

### Goal

Create the local environment in which the entire demo can run.

### Tasks

- [x] Choose a pinned Flask 3.1.3 repository fixture.
- [x] Create two Git worktrees.
- [x] Add Developer A and Developer B identities.
- [x] Add a reset script.
- [x] Add a baseline test command.
- [ ] Add a scripted placeholder conflict until the final scenario is selected.

### Exit condition

Two workspaces can start from the same base commit, diverge, reset, and run independently.

## Phase 2: Service skeleton — [READY]

### Goal

Create the smallest working coordination service.

### Tasks

- [x] Create the FastAPI project.
- [x] Add `/health`.
- [x] Add workspace registration.
- [x] Add observation submission.
- [ ] Add alert retrieval.
- [ ] Add outcome submission.
- [ ] Add structured logging.
- [x] Add a simple persistence layer.

### Exit condition

The service accepts a workspace and observation and returns a traceable identifier.

## Phase 3: Manifest and evidence gate — [READY]

### Goal

Make the service trust repository-backed, commitment-likely observations rather than arbitrary messages.

### Tasks

- [x] Define manifest schemas.
- [x] Read Git branch and commit state.
- [x] Capture changed files and diff metadata.
- [x] Measure candidate dwell time.
- [x] Run a focused runnable/test check.
- [x] Add explicit publish intent and admission policy.
- [x] Validate branch and repository identity.
- [x] Reject stale observations.
- [x] Reject invalid evidence references.
- [x] Add duplicate protection.
- [x] Add positive and negative tests.

### Exit condition

The service visibly admits valid observations and rejects invalid or stale ones.

## Phase 4: Detection and targeted routing — [READY]

### Goal

Produce one explainable alert for the affected developer.

### Tasks

- [ ] Define the detector interface.
- [ ] Implement direct file or symbol overlap.
- [ ] Add a transparent risk score.
- [ ] Identify affected workspaces.
- [ ] Generate supporting evidence.
- [ ] Route the alert to the selected developer.
- [ ] Record alert outcome.

### Exit condition

A runnable, durable change in Developer A's worktree produces an explainable alert only for the relevant Developer B workspace.

## Phase 4.5: Agent contracts and orchestration — [READY]

### Goal

Represent the agentic workflow without prematurely distributing the application into multiple services.

### Tasks

- [ ] Define Workspace Agent, Orchestrator Agent, Verification Agent, and Conflict Analysis Agent interfaces.
- [ ] Define structured decision and handoff records.
- [ ] Implement the orchestrator as a local state machine.
- [ ] Add deterministic fallback behavior when Droid is unavailable.
- [ ] Log each agent decision with evidence references.

### Exit condition

The complete workflow can move from candidate observation to verification, conflict analysis, alert routing, and outcome recording through explicit agent handoffs.

## Phase 5: Droid integration — [DROID-BLOCKED]

### Goal

Add Droid as the reasoning engine for verification and conflict analysis without making the core product fragile.

### Tasks after sponsor discussion

- [ ] Confirm Model B as the primary integration direction.
- [ ] Confirm SDK runtime and authentication setup.
- [ ] Confirm read-only context, tools, MCP, and permission behavior.
- [ ] Create a small TypeScript/Node adapter if needed.
- [ ] Send a verified evidence bundle to Droid.
- [ ] Receive and validate structured verification/conflict decisions.
- [ ] Show Droid's reasoning result and evidence references visibly during the demo.
- [ ] Prototype Model D in an isolated worktree if time permits.
- [ ] Add a no-Droid fallback path.

### Exit condition

Droid performs meaningful reasoning in the running product and the InSync flow remains understandable without hidden assumptions.

## Phase 6: Final semantic detector — [SCENARIO-BLOCKED]

### Goal

Replace the temporary placeholder conflict with the final realistic scenario and connect it to the agentic analysis flow.

### Tasks

- [ ] Select a relatable small-team scenario.
- [ ] Identify provider and consumer artifacts.
- [ ] Define the evidence needed to prove impact.
- [ ] Implement the narrow detector.
- [ ] Define the initial taxonomy and an `other`/`needs_more_evidence` path.
- [ ] Add the Droid conflict-analysis prompt and structured schema.
- [ ] Add a positive case.
- [ ] Add a negative case.
- [ ] Add a combined integration test.
- [ ] Add reset and replay support.

### Exit condition

The final scenario is realistic, explainable, repeatable, and works through the complete pipeline.

## Phase 7: Thin VS Code surface — [READY]

### Goal

Make the workflow visible where the developer works.

### Tasks

- [ ] Add watching status.
- [ ] Add publish command.
- [ ] Add active-alert command.
- [ ] Add warning notification.
- [ ] Add evidence panel.
- [ ] Add acknowledge and dismiss actions.
- [ ] Keep CLI fallback fully working.

### Exit condition

The two-window demo can be performed from VS Code, with the CLI available for recovery.

## Phase 8: Demo packaging — [SCENARIO-BLOCKED]

### Tasks

- [ ] Write the two-minute script.
- [ ] Prepare clean starting state.
- [ ] Test reset and replay repeatedly.
- [ ] Record before-and-after test results.
- [ ] Verify all secrets are excluded.
- [ ] Prepare README setup steps.
- [ ] Prepare a short explanation of what is real, simulated, and deferred.

## Work that should not begin yet

- Generic multi-language analysis
- Automatic code edits
- Distributed multi-machine agent orchestration
- Multi-machine networking
- Complex dashboard work
- Learned scoring
- Marketplace packaging
- Broad semantic detection before the scenario is chosen
- Droid-specific runtime implementation before sponsor answers

## Definition of done for the first skeleton

The skeleton is ready to build on when:

```text
two worktrees
  → observation manifest
  → evidence verification
  → risk finding
  → targeted alert
  → recorded outcome
```

works locally, can be reset, and has tests for both accepted and rejected observations.
