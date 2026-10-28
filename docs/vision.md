# InSync — Project Vision

## Status

This document defines the current product direction for the CalHacks 36-hour prototype. It is a decision document, not a complete implementation plan.

The realistic demo scenario is intentionally still open. The final scenario should be selected after evaluating which small-team workflow is most relatable, technically demonstrable, and visually convincing.

### Decision labels

- **[BUILD NOW]** — stable enough to implement before the Droid discussion.
- **[DROID-DEPENDENT]** — may change after speaking with the Factory team.
- **[SCENARIO-DEPENDENT]** — depends on the final realistic demo example.
- **[DEFERRED]** — intentionally outside the 36-hour prototype.

## One-sentence vision

InSync is a lightweight, evidence-gated coordination layer that helps small software teams discover relevant in-progress changes before those changes become integration failures.

## The problem

Small teams often move faster than their coordination process. Two developers may work on interdependent parts of the same repository, each using a separate branch, workspace, or coding agent. Their local tests can pass independently while their combined changes become incompatible.

The problem is not simply that developers fail to communicate. The deeper problem is that the repository does not provide a reliable, timely, and verified view of unfinished work.

Existing tools usually become useful after a change has been committed, pushed, reviewed, or merged. By then, a teammate may have built on an outdated assumption and must redo work.

## Target users

### Primary users

Small, fast-moving software teams, especially:

- Two-to-ten-person startups
- Student and hackathon teams
- Small open-source projects
- Internal prototypes
- Teams rapidly adopting AI-assisted coding

The core workflow involves two independent developers working on one repository. Each developer may work manually or use a separate coding agent. InSync should not depend on a particular agent platform.

### User persona for the prototype

Two developers are implementing related features in the same repository. Neither has a complete live view of the other developer's local changes. They need an early, trustworthy warning when one person's work affects the assumptions of the other person's task.

### Secondary users

- Developer-tool and platform engineers
- Engineering leads responsible for reducing integration rework
- Maintainers of growing repositories with incomplete coordination processes

### Non-target users for the hackathon version

- Enterprise-wide source-control governance
- Full project-management or task-assignment workflows
- A universal manager for autonomous coding agents
- Developers working on completely independent components

Mature companies and large open-source repositories may still experience these conflicts. However, InSync's initial wedge is teams that lack formal contracts, ownership systems, and sophisticated coordination infrastructure.

## Value proposition

InSync gives a small team a lightweight safety net between local work and integration:

1. Observe work that is sufficiently durable and runnable to be a likely candidate for commitment.
2. Verify the observation against repository evidence.
3. Use bounded agentic reasoning to understand its meaning and compare it with other active work.
4. Identify only the affected teammate or workspace.
5. Explain the risk and suggest a human-approved next step.

The goal is not to predict every possible semantic conflict. The goal is to catch meaningful, explainable risks early enough for a developer to adapt before pushing or merging.

## Product principles

### Evidence before context

InSync should share claims backed by a branch, commit, working-tree diff, test result, or other repository evidence. It should not treat an unverified message or intended change as fact.

### Targeted awareness

Do not broadcast every change to everyone. Notify only the developer or workspace whose current work is relevant to the detected risk.

### Developer control

InSync may recommend an action, but the developer approves changes to their plan or code. Automatic code adaptation is outside the initial scope.

### Metadata first

The system should share structured metadata and evidence references by default rather than copying an entire unfinished workspace.

### Commitment-likely work before shared context

InSync should not share every local edit. Uncommitted manual work becomes a candidate only when it is runnable, has remained in the workspace for a meaningful period, and meets an explicit or configurable commitment-likelihood policy. A developer can also explicitly publish a candidate. If the work is later abandoned, InSync should still be able to explain why it was considered at the time.

### Deterministic evidence, agentic reasoning

Repository identity, Git state, timestamps, test results, and evidence references must be checked deterministically. Droid may reason over verified evidence to decide whether a change is meaningful, classify its impact, request more context, and explain a risk. Droid's judgment must remain bounded by evidence and validated by InSync policy.

### Reproducibility over illusion

The prototype should use real Git worktrees, real diffs, real verification, and real tests. Timing and user actions may be scripted so the demo is repeatable.

## Product experience

The primary user-facing surface is a thin VS Code extension. It should provide:

- A workspace identity and watching status
- A command to publish or inspect verified local work
- A targeted warning when another developer's change affects the workspace
- An evidence view showing why the alert was generated
- A human-approved response or next step

The coordination logic lives in a local FastAPI service. A CLI remains available as a fallback and as the most reliable way to replay the demo.

## Agentic architecture

InSync is composed of cooperating logical agents. They may run as modules in one local application during the hackathon rather than as separate deployed services.

- **Workspace Agent:** observes local work and identifies commitment-likely candidates.
- **Orchestrator Agent:** manages the workflow, state, retries, and handoffs.
- **Verification Agent:** validates repository evidence and decides whether a candidate can enter shared context.
- **Conflict Analysis Agent:** reasons about the relationship between verified changes and active workspaces.
- **Adaptation Agent:** optionally asks Droid to propose or apply an approved change in an isolated worktree.

The developer or an external coding agent remains the source of local work. InSync's agents coordinate and reason about that work; they do not replace the developer.

## Droid SDK's provisional role

**[DROID-DEPENDENT]** Droid's primary role is to provide repository-level reasoning for the Verification Agent and Conflict Analysis Agent. It should be a meaningful runtime component of InSync, not merely a tool used to build InSync.

The preferred integration direction is:

1. A Workspace Agent observes a candidate change and collects repository evidence.
2. Deterministic checks verify the candidate's provenance, runnable status, and durability.
3. InSync invokes Droid with a bounded evidence bundle and a structured output schema.
4. Droid helps verify meaning, analyze cross-workspace impact, and explain the decision.
5. InSync validates the result and sends an evidence-backed alert to the affected developer.
6. After human approval, Droid may propose or apply an adaptation in an isolated worktree.

This keeps InSync useful for manual developers while giving Droid meaningful responsibility in the live demo. The exact SDK runtime, tool, session, permission, and structured-output details remain dependent on the Factory discussion.

The product vision therefore does not depend on Droid being the developer's coding agent. Droid is the reasoning engine and optional adaptation engine inside the broader developer-coordination workflow.

## 36-hour MVP boundary

### Must have

- One repository fixture
- Two Git worktrees representing two developers
- Workspace-agent observation of manual or agent-produced local work
- Commitment-likelihood admission for uncommitted work
- Structured change manifests
- Evidence verification against Git state
- One direct-overlap detector
- One focused semantic or contract-risk detector
- Logical verification, conflict-analysis, and orchestration agents
- Targeted alert routing
- Explainable evidence output
- A resettable replay scenario
- A CLI fallback for the live demo

### Should have

- Thin VS Code extension
- Risk level and confidence
- Alert acknowledgement and outcome logging
- Combined integration-test result
- A visually clear evidence panel

### Explicitly deferred

- Generic conflict detection across arbitrary repositories
- Support for multiple programming languages
- Multiple IDEs
- Distributed autonomous multi-agent orchestration
- Unapproved code modification
- Production-grade authentication and multi-machine collaboration
- Learned risk scoring
- Complex dashboards
- Marketplace publishing
- Final Droid session, tool, and deployment model until the sponsor discussion is complete

## Success definition

The hackathon prototype succeeds if a judge can see this complete chain:

```text
Developer work → workspace admission → verified evidence → agentic impact analysis → targeted alert → human decision → safer integration
```

The demo should make the value understandable without requiring the judge to understand the entire architecture.

## Longer-term direction

After the prototype, InSync could expand toward:

- Dependency-aware impact analysis
- More language-specific contract detectors
- Adapters for coding-agent platforms
- Historical replay and measurable coordination improvements
- Privacy-aware team collaboration across machines
- Suggested plan adaptation with explicit developer approval

These are future directions, not requirements for the CalHacks submission.

