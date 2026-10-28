# InSync — Hackathon Demo Plan

## Demo objective

Show that two developers can independently work on the same repository, pass their local checks, and still create a hidden integration risk. Then show InSync detecting that risk before the work reaches a shared branch.

The final domain scenario is not locked yet. The scenario must be realistic and relevant, but the product flow below should remain stable even if the domain changes.

### Decision labels

- **[BUILD NOW]** — demo infrastructure that can be developed immediately.
- **[DROID-DEPENDENT]** — integration behavior to confirm with Factory.
- **[SCENARIO-DEPENDENT]** — domain, files, and conflict example to choose later.
- **[DEFERRED]** — useful future capability that will not be part of the first demo.

## Audience takeaway

The judge should leave with one clear idea:

> InSync gives small teams verified awareness of relevant unfinished work before their code collides.

The judge should understand:

- Who is using the system
- What changed
- Why the other developer is affected
- What evidence InSync used
- What action the developer takes
- How the result is safer than waiting for merge or CI

## Demo setting

Use one laptop with two VS Code windows and two Git worktrees.

### Window A

Developer 1's workspace. This developer changes one part of the system, manually or with a coding agent.

### Window B

Developer 2's workspace. This developer is implementing a related feature using a separate branch and local assumptions.

The two windows represent independent developers. They are not required to be two autonomous agents. One or both developers may use an agent, but the core product remains useful for manual development.

## Provisional Droid placement

**[DROID-DEPENDENT]** The preferred live-demo hypothesis is to use Droid as InSync's reasoning engine for verification and conflict analysis. Model D may then use Droid for a human-approved adaptation in an isolated worktree.

This gives the demo a clean division of responsibility:

- A Workspace Agent observes a runnable, durable candidate change.
- InSync verifies the repository evidence.
- Droid reasons over the verified context and analyzes whether the other developer is affected.
- The Orchestrator Agent routes the result.
- The affected developer receives the targeted alert.
- After approval, Droid may propose or apply an adaptation in an isolated worktree.

If the Factory team recommends a different SDK integration, the runtime adapter may use a direct context bundle, a custom InSync tool, or MCP.

## Scenario requirements

The final scenario should satisfy all of the following:

1. It reflects a problem a small team would genuinely encounter.
2. The two developers work on different but related areas.
3. Their work is not necessarily a textual merge conflict.
4. Each developer can appear successful in isolation.
5. The combined result creates a meaningful integration risk.
6. InSync can identify the affected workspace using repository evidence.
7. The alert can be explained in less than thirty seconds.
8. The scenario can be reset and replayed reliably.

The earlier API-field example is only a placeholder. A more realistic and emotionally recognizable scenario should replace it if one is found without increasing implementation risk.

## Recommended demo flow

### 0:00–0:15 — Set up the problem

Show the two VS Code windows and explain:

> “These are two developers on a small team. They are working in parallel, but neither has a reliable view of the other's unfinished changes.”

Show that both workspaces start from the same base commit.

### 0:15–0:35 — Establish independent progress

Developer 1 changes a provider, interface, schema, configuration, or shared behavior. The Workspace Agent waits for the work to be runnable and sufficiently durable before admitting it as a candidate.

Developer 2 continues work based on the previous behavior.

Each workspace can run its own local checks successfully.

### 0:35–0:55 — Trigger InSync

In Developer 1's VS Code window, run:

```text
InSync: Publish Verified Change
```

The Workspace Agent creates a structured manifest containing branch state, changed files or symbols, dwell time, runnable status, evidence references, and admission information.

### 0:55–1:15 — Show the evidence gate

The FastAPI service verifies that the manifest is supported by Developer 1's actual repository state. Invalid, stale, or unsupported observations should not become shared context.

Show a concise status transition:

```text
Observed → Candidate → Verified → Analyzed
```

If enabled, show the Droid Conflict Analysis Agent's structured decision and evidence references rather than a generic chat response.

### 1:15–1:35 — Deliver a targeted alert

Developer 2 receives a VS Code warning:

```text
InSync detected a verified change affecting your workspace.

Risk: High
Category: [scenario-specific category]
Affected area: [file, symbol, interface, or dependency]
Action: View evidence
```

The alert should go only to Developer 2, not to every workspace.

### 1:35–1:55 — Inspect and decide

Open the evidence view and show:

- Developer 1's identity
- Repository and branch
- Commit or working-tree reference
- Changed artifact
- Why Developer 2 was selected
- Supporting diff or evidence reference
- Suggested next step

Developer 2 acknowledges the alert and manually adapts their work.

If Model D is included, Developer 2 explicitly approves Droid's assistance. Droid works in an isolated worktree, and the proposed diff and test results are shown before any adoption.

### 1:55–2:10 — Validate the outcome

Run the combined test or integration check again. Show that the issue is now avoided, or show a clear before-and-after comparison.

Close with:

> “InSync did not write code for the team. It gave the affected developer verified context early enough to prevent rework.”

## Demo surfaces

### Essential VS Code surface

- Status bar: `InSync • Watching`
- Command: `Publish Verified Change`
- Notification: `View Evidence`
- Evidence panel with a compact alert card

### Backend surface

The FastAPI service should expose only the endpoints needed for the demo:

- Register workspace
- Submit observation
- Verify observation
- Analyze risk
- Retrieve alerts
- Record alert outcome

### CLI fallback

The same flow should be runnable with commands similar to:

```bash
insync init --developer dev-a
insync publish
insync alerts --developer dev-b
insync demo reset
```

The CLI is not a separate product. It is the reliability path for development, testing, and judging.

## Demo modes

### Mode 1: Semi-live preferred mode

Make a real change in Developer 1's worktree, run the publish command, and allow the service to detect and route the alert. Developer 2 then inspects the evidence.

### Mode 2: Deterministic replay

Use a prepared fixture and scripted changes to guarantee the same sequence every time. This is acceptable because the Git changes, evidence checks, detector, and tests remain real.

### Mode 3: CLI emergency fallback

Run the complete flow from the terminal if the extension cannot be launched or a VS Code window becomes unreliable.

## Demo reliability requirements

- One-command reset to the initial repository state
- Fixed test data and known branch names
- No dependence on network access during judging
- No dependence on unconstrained LLM behavior or live code generation for the core alert
- No secrets in the repository
- A pre-tested fallback path
- Clear labels distinguishing real behavior, scripted timing, and future work

## What not to show

Do not spend demo time on:

- A large dashboard
- Generic AI chat
- Long architecture explanations
- Multiple IDEs
- Unapproved autonomous code edits
- A claim that InSync detects every semantic conflict
- A complicated database view

## Open demo decisions

These decisions should be made after the core pipeline works:

- Final domain and conflict scenario
- Whether Developer 1 or Developer 2 uses a coding agent
- Whether the final alert is a contract change, dependency impact, duplicate work, or integration-test risk
- Whether the evidence panel is a VS Code webview or a compact terminal/CLI view
- Whether a sponsor-specific agent integration is included in the final demo

