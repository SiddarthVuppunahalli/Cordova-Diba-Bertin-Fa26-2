# InSync Project Instructions

## Project context

InSync is a CalHacks 36-hour prototype for small, less-organized software teams. It helps two independent developers working on the same repository discover relevant in-progress changes before push or merge.

Each developer may work manually or with a separate coding agent. InSync is developer-first and agent-agnostic.

## Source of truth

Read these files before making architectural or implementation decisions:

1. vision.md
2. decisions.md
3. droid-integration-brief.md
4. mvp.md
5. architecture.md
6. demo.md
7. plan.md

Original source material is in:

- project_sources/03-workbook-1-.pdf
- project_sources/01-Copy-of-13.0-Hacker-Guide_compressed.pdf
- project_sources/02-ShubhamBaid-TreeHacks-Resume.pdf

The Markdown documents represent the current project decisions. The workbook is background context, not an automatic source of truth.

## Decision labels

- [BUILD NOW] — safe to implement immediately
- [DROID-DEPENDENT] — do not finalize until the Factory discussion
- [SCENARIO-DEPENDENT] — depends on the final realistic demo scenario
- [DEFERRED] — outside the current hackathon scope

## Current implementation constraints

- Use two Git worktrees to represent two developers.
- Use a local FastAPI coordination service.
- Keep a CLI fallback even if a VS Code extension is added.
- Verify observations against actual Git evidence.
- Share metadata and evidence references by default.
- Use explainable rules before learned scoring.
- Send targeted alerts only to affected workspaces.
- Require human approval before code adaptation.
- Use deterministic replay and resettable demo state.

## Do not implement yet

Do not finalize or implement:

- The final Droid integration model
- The final semantic conflict detector
- The final realistic demo scenario
- Automatic code adaptation
- Multiple IDEs or programming languages
- Full agent orchestration
- Multi-machine collaboration
- Complex dashboards
- summary.md

When a new decision is made, update decisions.md before changing mvp.md or architecture.md.