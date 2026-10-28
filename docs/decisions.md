# InSync — Decision Log

## Purpose

This file records what has been decided, what is provisional, and what must remain open until the Factory Droid discussion or the realistic demo scenario is finalized.

## Decision labels

- **[LOCKED]** — do not revisit during the hackathon unless it blocks delivery.
- **[BUILD NOW]** — safe to implement immediately.
- **[DROID-DEPENDENT]** — wait for information from the Factory team.
- **[SCENARIO-DEPENDENT]** — wait for the final demo example.
- **[DEFERRED]** — outside the current prototype.

## Locked decisions

### D-001: Target small, less-organized teams

**Status:** [LOCKED]

InSync targets small, fast-moving teams where two developers work on interdependent parts of one repository without complete formal coordination infrastructure.

### D-002: Two independent developers are the core workflow

**Status:** [LOCKED]

The primary demo represents two separate developers. Each developer may work manually or with a separate coding agent.

### D-003: InSync is developer-first and agent-agnostic

**Status:** [LOCKED]

The core product must remain useful without a direct integration with any coding-agent platform. Agent support is an adapter or participant, not the identity of the product.

### D-004: Evidence-gated shared context

**Status:** [LOCKED]

Only observations supported by repository evidence should become shared context. Evidence may include a working-tree diff, branch, commit, changed symbol, test result, timestamp, or other recorded repository state.

Uncommitted manual work is not automatically shared. It becomes an observation candidate only when it is runnable, has remained in the workspace for a meaningful duration, and satisfies the commitment-likelihood policy or an explicit developer publish action. If the work is later abandoned, the observation must retain the reasons and evidence that justified considering it.

### D-005: Targeted alerts, not broadcast updates

**Status:** [LOCKED]

An alert should be sent only to the developer or workspace whose active work is relevant to the detected risk.

### D-006: Thin local implementation

**Status:** [LOCKED]

The initial stack is:

- Two Git worktrees
- Local collector or CLI
- Python/FastAPI coordination service
- Small persistent store or existing PostgreSQL setup
- Thin VS Code surface
- CLI fallback

### D-007: Deterministic evidence with bounded agentic reasoning

**Status:** [LOCKED]

Deterministic logic owns repository identity, Git provenance, timestamps, runnable checks, evidence references, and final alert-policy validation. Droid may reason over verified evidence to classify impact, request additional context, and explain a risk. Every alert must remain traceable to evidence that InSync independently validates.

### D-008: Human approval before adaptation

**Status:** [LOCKED]

InSync may suggest an action. It will not automatically modify a developer's code in the initial prototype.

### D-009: Real repository state, deterministic replay

**Status:** [LOCKED]

The demo will use actual Git branches, worktrees, diffs, verification, and tests. Scripted timing and reset commands are acceptable for reliability.

### D-014: SQLite for the local prototype

**Status:** [BUILD NOW]

The first local coordination service uses SQLite behind a replaceable storage boundary. This supports restartable deterministic replay without adding an external service. PostgreSQL remains an open future deployment choice and is not part of this prototype vertical.

### D-015: Publish intent does not bypass the evidence gate

**Status:** [LOCKED]

Explicit publish intent requests that InSync evaluate a candidate, but it does not bypass runnable, meaningful, durable, commitment-likely, dwell-time, or repository-evidence requirements. The admission decision records the request and the rejection reason when the gate is not satisfied.

## Provisional decisions

### D-010: Droid is a reasoning component in the InSync workflow

**Status:** [DROID-DEPENDENT]

The preferred integration is Model B: InSync invokes Droid with a bounded, verified repository context for verification and conflict analysis. Model D—human-approved Droid adaptation in an isolated worktree—is an attractive optional extension. Model A, where Droid simply acts as one developer's coding agent, may still be shown but is not the core Droid role.

The exact invocation, tools, permissions, and session lifecycle remain dependent on the Factory discussion. InSync must remain useful for manual developers and must retain a deterministic fallback.

### D-011: TypeScript boundary for Droid integration

**Status:** [DROID-DEPENDENT]

The current official SDK is TypeScript/Node-oriented. The integration should use a small Node/TypeScript Droid adapter beside the Python service, subject to confirmation of runtime, daemon, credentials, and tool-control details.

### D-012: Logical agent boundaries, local process implementation

**Status:** [LOCKED]

InSync will expose distinct logical roles—Workspace Agent, Orchestrator Agent, Verification Agent, Conflict Analysis Agent, and optional Adaptation Agent. During the hackathon they may run as modules in one local process. Separate networked microservices are not required.

### D-013: Conflict categories are a taxonomy, not a ceiling

**Status:** [LOCKED]

The initial conflict categories provide explainable labels and evaluation cases. They do not restrict Droid from returning `none`, `needs_more_evidence`, or a new/other category. InSync will only route an alert when the result is supported by verified evidence and passes policy.

## Open questions

- Should Model B use a read-only evidence bundle, an SDK tool, MCP, or a combination?
- Can the SDK run bounded reasoning against a specific Git worktree or supplied evidence context?
- Can a run or session return structured verification and conflict-analysis decisions?
- Can InSync expose a custom tool, MCP server, or hook that Droid can call?
- How should read-only analysis and human-approved adaptation permissions be represented?
- Can Model D safely operate in an isolated worktree and return a diff plus test evidence?
- Can the demo run locally with a sponsor-provided API key and without network instability?
- What level of Droid usage qualifies as meaningful use for the Factory track?

## Decisions intentionally deferred

- Final realistic demo domain and conflict scenario
- Exact Droid tool and session lifecycle
- Whether the final interface is primarily a VS Code extension, CLI, or both
- SQLite versus PostgreSQL if database setup becomes a schedule risk
- Whether to include Model D adaptation in the live demo
- Whether to support a second language or repository

## Decision record template

When a pending question is resolved, add:

```text
Decision ID:
Date:
Question:
Decision:
Reason:
Impact on MVP:
Impact on architecture:
Follow-up tasks:
```
