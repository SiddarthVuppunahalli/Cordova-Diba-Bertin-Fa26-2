# InSync — Initial MVP Definition

## Purpose

This is the initial MVP definition before the Factory-specific runtime details are finalized. It defines the stable agentic workflow and the bounded integration points that can be built without knowing every SDK detail.

The MVP is intentionally a skeleton first. It should establish the repository, workspace, candidate-admission, manifest, evidence, agent handoffs, detection, and alert flow before adding sponsor-specific runtime behavior.

## Product promise

Given two developers working in separate worktrees of one repository, InSync can verify one developer's local change, identify a relevant risk to the other developer, and send an explainable targeted alert before push or merge.

## MVP status labels

- **[BUILD NOW]** — implement immediately.
- **[DROID-DEPENDENT]** — placeholder only until Factory guidance.
- **[SCENARIO-DEPENDENT]** — implement using a temporary fixture, then replace the scenario.
- **[DEFERRED]** — do not implement for the first skeleton.

## Stable MVP core

### 1. Repository and workspace setup — [BUILD NOW]

- One local repository fixture
- Two Git worktrees
- Two developer identities
- Branch and base-commit tracking
- One-command reset to a clean starting state

### 2. Local observation — [BUILD NOW]

Capture:

- Repository identifier
- Workspace identifier
- Developer identifier
- Branch
- Base commit
- Current commit, if present
- Changed files
- File operations
- Observation timestamp
- Evidence references

Start with a manual `publish` command that invokes the Workspace Agent's admission checks. A file watcher can be added later if time permits.

### 3. Candidate admission — [BUILD NOW]

For uncommitted manual work, evaluate:

- Whether the relevant code is runnable or passes a focused test command
- How long the change has remained in the workspace
- Whether the diff is meaningful rather than a transient edit
- Whether the developer explicitly marked it as a candidate for sharing
- Whether the branch, workspace, and repository are identifiable

The admission result must record both the decision and the reasons. A later abandonment does not invalidate the historical reasoning for why the candidate was considered.

### 4. Evidence verification — [BUILD NOW]

The service should verify that:

- The workspace exists.
- The branch and repository match the registration.
- The referenced files are present.
- The reported diff or commit exists.
- The candidate satisfies the admission policy.
- The observation is not stale or duplicated.

Invalid or stale claims must be rejected visibly.

### 5. Manifest and service contracts — [BUILD NOW]

Create schemas for:

- Workspace registration
- Change observation
- Evidence item
- Risk finding
- Alert
- Alert outcome
- Agent decision and handoff records

Keep the manifest extensible so later Droid metadata can be added without redesigning the core.

### 6. Agentic detection and analysis — [BUILD NOW]

Implement the workflow in this order:

1. Deterministic candidate retrieval and direct file or symbol overlap
2. Verification Agent decision over validated evidence
3. Conflict Analysis Agent decision for one focused semantic or contract detector
4. Optional Droid-backed reasoning using structured output

The exact semantic detector is **[SCENARIO-DEPENDENT]** and should not be over-generalized. Droid may identify a risk outside the initial taxonomy, but InSync must label it as uncertain or require more evidence rather than silently treating it as fact.

### 7. Risk scoring — [BUILD NOW]

Use a hybrid score and decision record based on factors such as:

- Direct overlap
- Shared symbol or interface
- Dependency relationship
- Evidence freshness
- Test or integration impact
- Verification-agent decision
- Conflict-analysis-agent reasoning

The score should include human-readable reasons and evidence references. A model-generated confidence value is advisory, not an independently calibrated probability.

### 8. Targeted alert — [BUILD NOW]

Route an alert only to the affected developer or workspace. The alert must contain:

- Risk level
- Conflict category
- Affected artifact
- Supporting evidence
- Why the recipient was selected
- Suggested next step

### 9. Developer response — [BUILD NOW]

Record whether the developer:

- Acknowledged the alert
- Inspected the evidence
- Dismissed it
- Coordinated with the other developer
- Ran a focused test
- Updated their work

Automatic code edits remain deferred.

## Droid-dependent work

### Model B: Droid-powered verification and conflict analysis — [DROID-DEPENDENT]

InSync invokes Droid with verified repository evidence and a bounded task. Droid returns structured reasoning for candidate verification, impact analysis, or both.

### Model D: Human-approved adaptation — [DROID-DEPENDENT]

After a developer approves an alert, InSync may ask Droid to propose or apply an adaptation in an isolated worktree. The resulting diff and tests become new evidence. This is optional for the first demo.

Model C—Droid calling InSync through an SDK tool or MCP server—remains a possible context-delivery mechanism for Model B. Model A remains an optional way to create repository changes, not the core InSync agentic role.

## Initial acceptance criteria

The skeleton is ready when:

1. Two workspaces can register in one repository.
2. One workspace can publish a local change.
3. The Workspace Agent can admit or reject a candidate with reasons.
4. The service can verify or reject the observation.
5. The observation and agent decisions are persisted or logged with provenance.
6. A direct-overlap test produces a risk finding.
7. An affected workspace receives a targeted alert.
8. The alert can be acknowledged and recorded.
9. The entire flow can be reset and replayed.

## Explicitly outside the MVP

- Generic semantic conflict detection
- Multiple programming languages
- Multiple IDEs
- Real-time multi-machine collaboration
- Distributed multi-machine agent orchestration
- Autonomous code changes
- Learned scoring
- Complex dashboard
- Marketplace publishing
- Production-grade authentication

