# InSync — Droid SDK Integration Brief

## Purpose

This document prepares a short explanation and a focused question set for speaking with the Factory Droid team at CalHacks.

The integration is intentionally not finalized. The goal of the conversation is to identify the strongest meaningful use of Droid that can be implemented reliably within the remaining hackathon time.

## Two-minute explanation for the Factory team

> I am building InSync, a coordination layer for small software teams where two developers work in parallel on the same repository. One developer may be working manually and the other may be using an AI coding agent. The problem is that each workspace only sees its own unfinished changes. Both developers can pass local tests while one change silently invalidates the assumptions of the other.
>
> InSync captures a developer's in-progress change as a structured manifest, verifies it against the actual Git state, analyzes whether another active workspace is affected, and sends a targeted evidence-backed warning before the change reaches push or merge. The system shares metadata and evidence references rather than broadcasting unfinished source code.
>
> I want Droid to be a real reasoning participant in this workflow, not just a tool I use to build the project. My preferred integration is for InSync to provide Droid with verified repository evidence and ask it to help verify the meaning of a candidate change and analyze whether another developer's work is affected. Droid would return a structured decision and explanation that InSync can validate and surface in the affected developer's VS Code workspace. After human approval, Droid could optionally propose an adaptation in an isolated worktree.
>
> I am also exploring whether the SDK should receive a verified context bundle directly, call an InSync tool or MCP server for evidence, or support both. I want to choose the option that is most meaningful to Factory and most reliable to demonstrate live.

## What I currently understand

The official TypeScript SDK currently appears to support:

- Running Droid from a Node application
- Continuing multi-turn sessions
- Streaming work
- Structured output
- Tool control and permissions
- MCP-related integration
- Connecting to an existing Droid daemon
- Session and browser/daemon integration paths

The CalHacks SDK material describes a TypeScript/Node setup, `run()`, structured output, sessions, and a browser daemon client. The public SDK repository documents Node and daemon entry points, local Droid CLI execution, `FACTORY_API_KEY`, sessions, tools, permissions, MCP, hooks, and examples.

These capabilities suggest that a small Node/TypeScript adapter may be the cleanest boundary between Droid and the Python/FastAPI InSync core. The adapter would expose role-specific operations for verification, conflict analysis, and optional adaptation. The exact context-delivery and permission model remains open.

## Candidate integration models

### Model A — Droid as one developer's coding agent

Droid performs a real coding task in Developer A's worktree. InSync independently observes the workspace and alerts Developer B if the change affects B's work.

**Advantages:** simplest, preserves the agent-agnostic core, easy to explain.

**Risk:** Droid's role is indirect if InSync only observes the result; this is not the primary integration direction.

### Model B — InSync invokes Droid for verified analysis — preferred

After deterministic checks admit a candidate and verify its evidence, InSync invokes Droid to inspect the affected context and return a structured verification, impact analysis, explanation, or recommended next action.

**Advantages:** Droid contributes directly to the product's user-visible reasoning and decision-making.

**Risk:** introduces SDK runtime, prompt, credentials, permissions, cost, and latency dependencies.

### Model C — Droid calls InSync as a tool or MCP server

InSync exposes a tool or MCP interface that lets Droid request verified context about relevant teammate changes while performing Model B analysis.

**Advantages:** directly connects the product to the coding-agent workflow and preserves InSync's evidence gate.

**Risk:** requires understanding the SDK's custom-tool, MCP, permission, and context-injection behavior quickly.

### Model D — Human-approved Droid adaptation — optional extension

The affected developer approves an InSync alert, after which Droid proposes or applies a change in an isolated worktree. The resulting diff and tests are shown in the evidence trail before the developer adopts the result.

**Advantages:** creates a compelling end-to-end moment.

**Risk:** automatic adaptation is technically and conceptually the riskiest option for a 36-hour build.

## Preferred discussion position

Start by proposing Model B as the primary integration, with Model C as a possible context-delivery mechanism and Model D as an optional second-stage demo. Model A may be used to generate a real change, but it is not the core InSync agentic role.

The important principle is:

> Droid should do meaningful repository reasoning in the running product, while InSync remains the verified coordination layer between independent developer workspaces.

## Questions for the Droid team

### Runtime and execution

1. Can the TypeScript SDK start Droid programmatically during a live demo?
2. Can we provide a specific local Git worktree or working directory for a read-only reasoning session?
3. Can we start a session with a bounded verification or conflict-analysis task and observe its reasoning progress?
4. Can the SDK stream progress while the session is running?
5. Can the SDK return structured output containing an admission/verification decision, affected workspaces, conflict category, evidence references, confidence, and recommended action?

### Tools and context

6. Can an application expose a custom tool that Droid can call during a session?
7. Is MCP the recommended way for Droid to call an external local service such as InSync?
8. Can InSync provide a tool such as `get_verified_context(workspace_id)` or `get_active_workspaces(repository_id)`?
9. Can a tool return metadata and evidence references without returning full unfinished source code?
10. Can Droid request additional evidence when its first analysis is inconclusive?

### Permissions and human control

11. Can we require human approval before Droid edits a worktree or runs a sensitive command for Model D?
12. Can the application inspect or control the tools Droid is allowed to use?
13. Can a Droid analysis session be continued after InSync provides additional evidence?
14. What is the safest way to keep the final adaptation isolated until approval?

### Demo and sponsor-track expectations

15. What exactly qualifies as meaningful Droid usage for the Factory track?
16. Would Model B—Droid-powered verification and conflict analysis—qualify as meaningful runtime usage for the Factory track?
17. Which Droid output should be visible to judges: tool calls, structured decision, evidence references, adaptation diff, tests, or final result?
18. Are there recommended starter examples for two worktrees or developer-tool workflows?
19. Are there API-credit, rate-limit, network, or authentication risks during judging?
20. Is there a recommended fallback if the SDK or daemon is unavailable during the live demo?

## Information to bring back from the discussion

Record the answers under these headings:

```text
Recommended integration model:
Required runtime:
Required package and version:
Authentication method:
Worktree support:
Session and streaming behavior:
Tool/MCP integration:
Permission model:
Structured output format:
Meaningful-use expectation:
Demo recommendation:
Known limitations:
```

## Official references

- Factory SDK overview: https://github.com/Factory-AI/droid-sdk-typescript
- CalHacks Droid SDK examples: https://github.com/factory-davidgu/calhacks13
- Factory documentation: https://docs.factory.ai/

