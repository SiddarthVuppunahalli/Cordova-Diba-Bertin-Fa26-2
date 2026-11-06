# Source Map

This ledger traces implementation-facing claims to the archived Project Workbook or to a newer explicit decision. It prevents derived documents from silently becoming unsupported requirements.

## Status

- `SOURCE`: directly stated in the workbook.
- `USER`: explicit decision that supersedes the workbook.
- `DERIVED`: implementation consequence of one or more source statements.
- `OPEN`: unresolved and not safe to present as decided.

| ID | Status | Claim | Origin | Canonical destination |
|---|---|---|---|---|
| SRC-001 | SOURCE | InSync warns about relevant in-progress changes before push or merge. | Workbook Chapters 1-3 | `context/product.md` |
| SRC-002 | SOURCE | Only evidence-backed claims become shared context. | Chapters 1, 2, 3, and 5 | `context/product.md`, `context/requirements.md` |
| SRC-003 | SOURCE | The pipeline is identify, verify, detect and score, notify, suggest. | Chapter 5, Figure 1 | `context/system-architecture.md`, `diagrams/` |
| SRC-004 | SOURCE | Verification occurs locally in the producer workspace and centrally at the evidence gate. | Chapter 5 | `context/system-architecture.md`, `contracts/evidence-and-verification.md` |
| SRC-005 | SOURCE | Metadata and evidence references are shared by default, not unfinished raw source. | Chapters 3 and 5 | `contracts/change-manifest.md` |
| SRC-006 | SOURCE | Verification levels are local, shared branch, mainline, and attested only. | Chapter 5 | `contracts/evidence-and-verification.md` |
| SRC-007 | SOURCE | Ingestion must be idempotent and stale or unsupported claims must be rejected. | Chapters 3, 5, and 8 | `contracts/evidence-and-verification.md` |
| SRC-008 | SOURCE | Deterministic checks are primary; semantic model checks are capped and logged. | Chapters 1, 4, and 5 | `context/requirements.md`, `context/open-questions.md` |
| SRC-009 | SOURCE | The architecture is centralized and service-oriented, with agent adapters at workspace boundaries. | Chapter 5 | `context/system-architecture.md` |
| SRC-010 | SOURCE | The implementation stack proposed by the workbook is Python, FastAPI, Docker Compose, PostgreSQL 16, and JSONB. | Chapters 5 and 8 | `context/decisions.md` |
| SRC-011 | USER | The evaluation layer has been dropped entirely. | User decision, 2026-11-05 | `context/decisions.md`, `diagrams/` |
| SRC-012 | USER | The shared blueprint describes the complete intended system rather than the current developer milestone. | User decision, 2026-11-05 | `context/system-architecture.md`, `operations/implementation-stages.md` |
| SRC-013 | USER | Each runtime agent is implemented as a separate vertical over shared architecture. | User decision, 2026-11-05 | `context/system-architecture.md` |
| SRC-014 | USER | Dependencies owned by unfinished verticals are represented through shared contracts and local stubs or placeholders. | User decision, 2026-11-05 | `contracts/vertical-contracts.md`, `.agent-local/` |
| SRC-015 | DERIVED | Agent verticals require explicit, versioned ports so implementations and test doubles remain substitutable. | SRC-013, SRC-014 | `context/system-architecture.md`, `contracts/` |
| SRC-016 | USER | A vertical first proves its core behavior with a minimal runnable implementation; framework and system integration follow through ports and adapters. Final deployment topology is deferred. | User decision, 2026-11-05 | `context/decisions.md`, `context/system-architecture.md`, `operations/implementation-stages.md` |
| SRC-017 | USER | Important or non-reconstructible information uses durable retryable handoffs, while explicitly classified observations may be best effort when later observations safely reconstruct them. | User decision, 2026-11-05 | `context/decisions.md`, `context/system-architecture.md`, `contracts/vertical-contracts.md` |
| SRC-018 | USER | Registered repository members and their authorized agents may access verified shared context; only affected recipients whose risk meets the repository threshold are notified. | User decision, 2026-11-06 | `context/decisions.md`, `context/system-architecture.md`, `context/requirements.md` |
| SRC-019 | USER | Registration authorities issue repository and actor identities; each vertical generates identifiers for the artifacts it creates, and consumers preserve those identifiers. | User decision, 2026-11-06 | `context/decisions.md`, `context/domain-model.md`, `contracts/` |
| SRC-020 | USER | Provisionally, Verification emits immutable claims while a deterministic Shared Context logical component owns post-admission lifecycle, repository access, and context queries behind a public port. | User decision, 2026-11-06 | `context/decisions.md`, `context/system-architecture.md`, `context/open-questions.md` |

## Unresolved source tensions

- The workbook mentions a five-category taxonomy but later lists four analysis categories and treats coordination/timing as a modifier.
- The workbook proposes PostgreSQL, while individual verticals may begin with in-memory adapters before integration.
