# Product Context

## Problem

Developers and coding agents can work independently on interdependent parts of one repository. Their changes may pass in isolation while relying on incompatible assumptions. Conventional review and CI usually reveal this after work is pushed, when rework is already expensive.

## Product promise

InSync creates a verified pre-merge awareness channel. It observes commitment-likely work, verifies claims against an identified repository state, and makes evidence-backed context available to registered repository members and their authorized agents. It interrupts only affected recipients when risk crosses the configured alert threshold.

## Primary users

- Small software teams with parallel work in one repository
- Developers using independent coding-agent sessions or worktrees
- Teams that need earlier awareness without broadcasting unfinished source

## Product principles

1. Evidence before shared context.
2. Repository facts before model assertions.
3. Metadata and provenance before source disclosure.
4. Explicit uncertainty instead of fabricated certainty.
5. Targeted awareness instead of broadcast noise.
6. Human approval before code adaptation.
7. Replaceable verticals behind stable contracts.

## Intended product outcome

The completed system connects five cooperating verticals:

1. Workspace observes in-progress work and creates evidence-linked manifests.
2. Verification decides which claims are trustworthy enough to share.
3. Conflict Analysis determines whether verified work affects another active workspace.
4. Orchestration and Notification deliver targeted findings and record responses.
5. Optional Adaptation prepares human-reviewed suggestions without changing developer code automatically.

Individual verticals can be implemented independently, but the shared blueprint always describes this complete outcome.

Verified shared context is also an input to future coding-agent work. Access is repository-scoped, while retrieval remains relevance-aware so an agent receives useful context rather than an undifferentiated repository feed.

## Product scope boundaries

- InSync does not guarantee detection of every semantic conflict.
- It does not replace developer decisions about task ownership or merging.
- It does not share unfinished raw source by default.
- It does not automatically modify developer code.
- The evaluation layer and historical replay infrastructure are not part of the current architecture.
