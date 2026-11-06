# InSync Blueprint Finalization — Team Discussion Guide

## Meeting goal

Agree on the shared architecture and contracts that every vertical can build against. The meeting does not need to settle framework code, deployment scripts, or one developer's milestone plan.

By the end, each numbered topic should be marked `accepted`, `revised`, or `explicitly deferred`, with an owner for any follow-up.

## Set the stage

Start with these two views:

1. [Overall system architecture](../diagrams/system-architecture.md) — responsibilities and cross-vertical flow.
2. [Recommended claim-to-notification lifecycle](../diagrams/claim-to-notification.md) — the complete proposed flow from observed work to shared context and targeted notification. The editable source is [Mermaid diagram source](../diagrams/src/claim-to-notification.mmd).

Then state the product promise: InSync verifies in-progress repository facts before they become shared context, makes verified context available to authorized repository members and agents, and interrupts only affected recipients when risk crosses the repository threshold. See [product context](../context/product.md).

## Foundations already agreed

These should be treated as the starting point unless the team has concrete evidence for changing them:

1. The evaluation layer is removed.
2. The shared blueprint represents the complete intended system, not only the Verification milestone.
3. Runtime agents are independent verticals behind versioned contracts.
4. Each vertical first proves its core behavior with a minimal runnable implementation; infrastructure integration comes afterward.
5. Only a verified claim crosses the evidence gate.
6. Shared payloads are metadata- and evidence-first; unfinished raw source is excluded from Version 1.
7. Important handoffs are durable, retryable, and idempotent; cheap safely reconstructible observations may be best effort.
8. Registered repository members and their authorized agents may access verified shared context. Notifications are narrower and threshold-gated.
9. Registration owns repository and actor identities; each vertical owns identifiers for the artifacts it creates.
10. Verification emits an immutable claim. Changing post-admission state is stored separately.

Supporting artifacts:

- [Decision log](../context/decisions.md)
- [Shared requirements](../context/requirements.md)
- [Domain model and identifier ownership](../context/domain-model.md)
- [Vertical contracts](../contracts/vertical-contracts.md)

## Points to discuss

### 1. Claim lifecycle and repository convergence

**Present:** A verified claim is immutable, but its relevance changes as work is updated, merged, incorporated by other workspaces, reverted, abandoned, or allowed to become stale.

**Decide:**

- Final lifecycle states and transition names.
- Whether integration and workspace convergence are separate states.
- Which workspaces count when determining convergence: all registered, active, affected, or another set.
- What Git evidence proves that a workspace contains an integration revision.
- Treatment of inactive workspaces, multiple workspaces per user, partial integrations, squash merges, cherry-picks, and reverts.
- When a claim stops participating in analysis but remains available for audit.

**Recommended starting position:** Use the lifecycle shown in the claim-to-notification diagram: `active`, `superseded`, `withdrawn`, `expired`, `integrated_pending_coverage`, `converged`, and `reverted`. Preserve inactive claims as historical context.

**Required artifacts:**

- [Claim-to-notification lifecycle](../diagrams/claim-to-notification.md)
- [Open questions — lifecycle](../context/open-questions.md)
- [Evidence and Verification contract](../contracts/evidence-and-verification.md)
- [Domain model](../context/domain-model.md)

**Meeting output:** Agreed lifecycle states, transition triggers, convergence population, and evidence rules.

### 2. Final responsibility and form of Shared Context

**Present:** The current proposal makes Shared Context a deterministic logical component that stores immutable claims, separately manages lifecycle, enforces repository-scoped access, and answers context queries.

**Decide:**

- Whether these responsibilities belong together.
- Whether Shared Context remains deterministic or needs any narrowly scoped agent capability.
- Whether it starts as an in-process module or a separately deployed service.
- Which component verifies and publishes lifecycle-changing events.
- The commands, queries, and events exposed by `SharedContextPort`.

**Recommended starting position:** Keep it deterministic and behind a public port. Begin as a module; preserve the ability to deploy it separately later. Do not use an LLM for storage, authorization, or lifecycle facts.

**Required artifacts:**

- [System architecture — Shared Context component](../context/system-architecture.md)
- [Decision DEC-013](../context/decisions.md)
- [Claim-to-notification lifecycle](../diagrams/claim-to-notification.md)
- [Vertical contracts](../contracts/vertical-contracts.md)

**Meeting output:** Responsibility boundary, initial implementation form, and named public port operations.

### 3. Manifest intake, scheduling, and Orchestration

**Present:** Many Workspace agents may submit manifests simultaneously. The system needs concurrency control, backpressure, fairness, prioritization, and retry scheduling without moving Verification rules into Orchestration.

**Decide:**

- Whether Workspace submits directly to Verification or through an orchestration-controlled ingress.
- Whether Orchestration is in the data path or acts as a control plane over a durable queue.
- Which layer owns admission throttling, deduplication, retry scheduling, and worker concurrency.
- How one noisy repository or workspace is prevented from starving others.

**Recommended starting position:** Let Orchestration own scheduling policy and traffic control while Verification remains a stateless or horizontally scalable domain worker. Let durable transport carry payloads so Orchestration does not manually relay every message.

**Required artifacts:**

- [Open questions — Orchestration intake](../context/open-questions.md)
- [System architecture](../context/system-architecture.md)
- [ChangeManifest contract](../contracts/change-manifest.md)
- [Handoff reliability decision](../context/decisions.md)

**Meeting output:** Manifest ingress path and explicit ownership of queueing, backpressure, deduplication, and retry scheduling.

### 4. Publish–subscribe versus point-to-point communication

**Present:** Different handoffs have different communication shapes. Some ask one owner to perform work; others announce a durable fact that several components may consume.

**Decide:**

- Which interactions are commands or requests and which are events.
- Whether the architecture uses publish–subscribe, point-to-point delivery, or both.
- Required acknowledgement, ordering, retry, replay, dead-letter, and authorization semantics.
- Ownership of message envelopes, correlation IDs, topic or queue names, and contract versions.
- Whether transport can be replaced without changing domain code.

**Recommended starting position:** Use a hybrid model. Use point-to-point commands for work with one responsible handler, such as “verify this manifest.” Publish durable events for facts with possible multiple consumers, such as “claim admitted” or “lifecycle changed.” Use at-least-once delivery with idempotent consumers and outbox/inbox behavior for important messages.

**Required artifacts:**

- [Open questions — communication model](../context/open-questions.md)
- [Vertical contracts](../contracts/vertical-contracts.md)
- [Handoff reliability architecture](../context/system-architecture.md)

**Meeting output:** Communication pattern for every boundary and a transport-neutral message envelope.

### 5. Freeze the Version 1 Verification contracts

**Present:** The Verification input and output contracts are still marked as drafts. The minimal Verification core should not be implemented against ambiguous fields.

**Decide:**

- Exact fields, types, enums, and conditional requirements for `ChangeManifest`, `EvidenceReference`, `VerificationDecision`, `VerificationCheck`, and `AdmittedClaim`.
- Initial supported evidence kinds and locator shapes.
- Which failures are `rejected` versus `needs_more_evidence`.
- Required reason-code families and stable meanings.
- JSON serialization, timestamp, path-normalization, and identifier formats.
- Contract compatibility rules and what requires a new version.

**Recommended starting position:** Freeze only the smallest complete Version 1 needed to prove Verification. Keep framework, transport, database, and mutable lifecycle fields outside these contracts.

**Required artifacts:**

- [ChangeManifest draft](../contracts/change-manifest.md)
- [Evidence and Verification drafts](../contracts/evidence-and-verification.md)
- [Vertical contract rules](../contracts/vertical-contracts.md)
- [Shared requirements](../context/requirements.md)

**Meeting output:** Approved Version 1 schemas, validation rules, examples, and compatibility policy.

### 6. Conflict-analysis taxonomy

**Present:** The workbook refers to five categories in one place but lists four analysis categories elsewhere, with coordination or timing sometimes treated as a modifier.

**Decide:**

- The canonical list of conflict or risk categories.
- Whether coordination and timing are categories, modifiers, or risk factors.
- Which category names become stable contract values.

**Recommended starting position:** Prefer a small stable category set and represent timing or coordination as structured modifiers unless they require distinct handling.

**Required artifacts:**

- [Source map — unresolved source tensions](../source-map.md)
- [Shared requirements — Conflict Analysis](../context/requirements.md)
- [Product context](../context/product.md)

**Meeting output:** Canonical category enum and definitions.

### 7. First language and parser boundary

**Present:** Verification can remain repository- and language-agnostic, but symbol and dependency analysis eventually requires language-aware parsing.

**Decide:**

- The first supported language.
- Whether parsing belongs entirely to Conflict Analysis or to a reusable repository-analysis adapter.
- Whether Version 1 accepts producer-reported symbols, centrally parsed symbols, or both.
- Behavior when a language or parser is unsupported.

**Recommended starting position:** Keep Verification language-agnostic. Place parser adapters behind the analysis boundary and return explicit unsupported or insufficient-evidence outcomes.

**Required artifacts:**

- [Open questions](../context/open-questions.md)
- [ChangeManifest symbols field](../contracts/change-manifest.md)
- [Evidence and Verification contract](../contracts/evidence-and-verification.md)
- [System architecture — Conflict Analysis](../context/system-architecture.md)

**Meeting output:** First language, parser owner, supported output, and unsupported-language behavior.

### 8. Risk threshold, notification policy, and first adapter

**Present:** Repository members may query shared context, but only affected recipients whose evidence-backed risk meets the repository threshold should be interrupted.

**Decide:**

- Who configures the repository threshold and whether categories may override it.
- What makes a workspace or user “affected.”
- Whether threshold comparison happens in Conflict Analysis or Orchestration.
- Deduplication, cooldown, acknowledgement, escalation, and failed-delivery behavior.
- The first team-wide notification adapter.

**Recommended starting position:** Conflict Analysis produces explainable risk; Orchestration applies repository notification policy; Notification handles delivery only. Start with the adapter the whole team can reliably demonstrate and observe.

**Required artifacts:**

- [Decision DEC-011](../context/decisions.md)
- [Claim-to-notification lifecycle](../diagrams/claim-to-notification.md)
- [Shared requirements — Orchestration and Notification](../context/requirements.md)
- [Open questions](../context/open-questions.md)

**Meeting output:** Threshold ownership, routing rules, deduplication policy, and first adapter.

### 9. Permissioned source excerpts and privacy audit

**Present:** Version 1 excludes unfinished raw source. A future feature might allow bounded excerpts, but repository membership alone should not automatically grant that access.

**Decide:**

- Whether excerpts remain entirely out of scope or are planned behind explicit consent.
- Who may request, approve, retrieve, and retain an excerpt.
- Required audit fields, redaction, size limits, expiry, and deletion behavior.

**Recommended starting position:** Keep excerpts out of Version 1. Add them only through a separately reviewed contract and audit policy.

**Required artifacts:**

- [Decision DEC-006](../context/decisions.md)
- [ChangeManifest privacy constraints](../contracts/change-manifest.md)
- [Product scope boundaries](../context/product.md)
- [Open questions](../context/open-questions.md)

**Meeting output:** Explicitly excluded, deferred with conditions, or approved with a named policy owner.

### 10. Semantic-model checks

**Present:** Deterministic repository evidence is authoritative. A semantic model may assist with interpretation but must not invent evidence or override a failed evidence gate.

**Decide:**

- Which verticals may use semantic models.
- Whether outputs are advisory, decision inputs, or explanation-only.
- Token, latency, cost, retry, and concurrency limits.
- Prompt and output logging rules that preserve privacy.
- Required deterministic evidence references and fallback behavior.

**Recommended starting position:** Keep the initial Verification core deterministic. Introduce semantic checks later as bounded, logged, evidence-grounded assistance that cannot turn missing or contradictory evidence into success.

**Required artifacts:**

- [Decision DEC-005](../context/decisions.md)
- [Source map entry SRC-008](../source-map.md)
- [Shared quality requirements](../context/requirements.md)
- [Open questions](../context/open-questions.md)

**Meeting output:** Allowed uses, hard limits, privacy rules, and authority boundaries—or an explicit deferral.

### 11. Persistence and deployment defaults

**Present:** Python is the initial language. FastAPI and PostgreSQL with JSONB are provisional integration adapters. Individual cores may begin with in-memory adapters, and final deployment topology is intentionally deferred.

**Decide:**

- Whether PostgreSQL remains the intended shared persistence adapter.
- Whether any vertical needs a different store for its domain.
- Which decisions can safely wait until integration.
- What evidence would justify splitting a module into a separate deployed service.

**Recommended starting position:** Preserve one logical boundary per vertical without committing to one process per vertical. Use in-memory adapters for core demonstrations and PostgreSQL when durable integration begins.

**Required artifacts:**

- [Decisions DEC-007 and DEC-009](../context/decisions.md)
- [Implementation stages](implementation-stages.md)
- [System architecture](../context/system-architecture.md)
- [Source map — persistence tension](../source-map.md)

**Meeting output:** Accepted integration defaults and an explicit list of deployment decisions being deferred.

### 12. Preventing stubs from reaching production

**Present:** Stubs are required for independent vertical development, but a production composition must never silently select one.

**Decide:**

- How adapters identify themselves as production, development, or test implementations.
- Whether production startup fails when a stub is registered.
- Whether stubs are excluded from production packaging or merely blocked by configuration.
- CI checks required before deployment.

**Recommended starting position:** Keep stubs in test/development composition only, mark them explicitly, exclude them from the production registry, and fail startup if production configuration resolves any stub.

**Required artifacts:**

- [Decision DEC-004](../context/decisions.md)
- [Vertical contract rules](../contracts/vertical-contracts.md)
- [Task protocol](task-protocol.md)
- [Open questions](../context/open-questions.md)

**Meeting output:** Enforceable composition and CI rules.

### 13. Blueprint ownership and change process

**Present:** Once the blueprint is finalized, independent developers need a safe way to evolve it without silently breaking neighboring verticals.

**Decide:**

- Owner and reviewer for each shared contract.
- Required decision record for a boundary change.
- Compatibility window for consumers of an older contract version.
- Whether diagrams and semantic companions are required in the same change.
- What “blueprint finalized” means and which remaining questions are deliberately deferred.

**Recommended starting position:** Require an accepted decision before changing a component boundary, require contract-owner and consumer review for contract changes, version breaking changes, and update diagram source plus semantic companion together.

**Required artifacts:**

- [Agent knowledge map](../README.md)
- [Decision log](../context/decisions.md)
- [Diagram policy](../diagrams/README.md)
- [Task protocol](task-protocol.md)
- [Open questions](../context/open-questions.md)

**Meeting output:** Named owners, approval rules, versioning policy, and the list of explicitly deferred topics.

## Recommended meeting order

1. Present the overall architecture and claim-to-notification diagram.
2. Confirm the agreed foundations.
3. Resolve Points 1–5 because they define ownership and the contracts used by current implementation.
4. Resolve or explicitly defer Points 6–12.
5. Agree on governance in Point 13.
6. Update the decision log, contracts, diagrams, and open-question list immediately after the meeting.

## Finalization checklist

- [ ] Every component has one clear responsibility.
- [ ] Every boundary has a named producer, consumer, and contract.
- [ ] Version 1 Verification contracts are approved.
- [ ] Durable versus reconstructible handoffs are identified.
- [ ] Shared Context and Orchestration responsibilities do not overlap ambiguously.
- [ ] Access, relevance, threshold, and notification are treated as separate concerns.
- [ ] Lifecycle states and repository-convergence rules are defined or deliberately deferred without blocking Verification.
- [ ] Stub safety and contract-change governance are enforceable.
- [ ] Accepted decisions are recorded in `context/decisions.md`.
- [ ] Remaining unresolved items are retained in `context/open-questions.md` with an owner and review point.
