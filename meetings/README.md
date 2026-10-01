# Meeting Log Book — CMPE 295A

**Project:** Verified Code-Change Awareness for AI-Assisted Software Teams
**Team:** Dhruv Verma · Anurag Bodapally · Siddarth Vuppunahalli · Shubham Baid
**Project Advisor:** Bertin Cordova Diba
**Course Instructor:** Prof. Jerry Gao

Meeting records covering **Workbook 1 → Workbook 2** (submission: December 2026).

**Project resources**
- [Abstract versions (Google Drive)](https://drive.google.com/drive/folders/188CRtjxIplUySTPhnxh9UzOlGY29cpUk?usp=sharing) — every draft and revision of the abstract
- [Shared working doc](https://docs.google.com/document/d/1TOybmzXKISLafO4h4UUBEd6wmVYEWW1DAyPc77wV7UI/edit?tab=t.w61wv9ea0kaf) — related-work reading list and each member's findings on the papers they read, the real-world scenario enumeration, and each member's abstract pitfalls write-up
- [Shared working doc — repository analysis tab](https://docs.google.com/document/d/1TOybmzXKISLafO4h4UUBEd6wmVYEWW1DAyPc77wV7UI/edit?tab=t.jvmv86yfccn0) — repository claims and merge-conflict scenarios mined from public GitHub projects

Required cadence:
- **Advisor meetings** (team + project advisor): ≥1 every two weeks
- **Team-only meetings** (students only): ≥1 every week

## Conventions

- One file per meeting: `YYYY-MM-DD-advisor.md` or `YYYY-MM-DD-team.md`
  (two of the same type on one day: `YYYY-MM-DD-team-2.md`).
- Structure follows the course-provided template verbatim — see [`TEMPLATE.md`](./TEMPLATE.md).
  To start a new note, copy `TEMPLATE.md` to `YYYY-MM-DD-team.md` or `YYYY-MM-DD-advisor.md`.
- **Commit each note within 24 hours of the meeting.** Git timestamps are the only
  evidence a grader has that notes were posted promptly. One commit per meeting.
- Stable IDs make action items traceable across notes:
  `AI-YYYYMMDD-NN` (action item), `D-YYYYMMDD-NN` (decision).
  **Every action item opened in §4 is closed in a later note's §1 carry-over table.**

## Cadence Tracker

| Week | Dates | Team-Only | Advisor | Notes |
|---|---|---|---|---|
| W1 | 2026-08-31 → 2026-09-06 | [09-01](./2026-09-01-team.md) · [09-03](./2026-09-03-team.md) · [09-04](./2026-09-04-team.md) · [09-06](./2026-09-06-team.md) | [09-02](./2026-09-02-advisor.md) | Kickoff, direction set with advisor, abstract submitted and resubmitted after instructor feedback |
| W2 | 2026-09-07 → 2026-09-13 | [09-12](./2026-09-12-team.md) | — (not due until 2026-09-16) | Findings on all assigned papers in; plan to mine real merge conflicts from public GitHub repositories |
| W3 | 2026-09-14 → 2026-09-20 | [09-15](./2026-09-15-team.md) | [09-16](./2026-09-16-advisor.md) | Repository analyses pooled and classified into five conflict categories; plan to recreate them on a Marketplace fixture presented to the advisor and endorsed; advisor marked the two-semester split — 295A scope/research/architecture, 295B build |
| W4 | 2026-09-21 → 2026-09-27 | [09-26](./2026-09-26-team.md) | — (not due until 2026-09-30) | Conflict categories ranked by priority and complexity; integration validation failure treated as a common fourth layer; five-stage process flow drafted. Planned 09-22 team meeting not held (member obligations); owner split deferred to 09-30 |
| W5 | 2026-09-28 → 2026-10-04 | [09-29](./2026-09-29-team.md) · [09-30](./2026-09-30-team.md) | [09-30](./2026-09-30-advisor.md) | PostgreSQL and JSON manifest chosen; demo scenarios presented to advisor, who asked for an architecture and component deep dive and a reworded priority/complexity ranking before the next advisor meeting; Workbook 1 finalized and submitted |

Legend: `—` = not due this week · `⚠️` = missed, with reason in Notes

## Open Action Items (rolling)

| ID | Action | Owner | Due | Status | Opened in |
|---|---|---|---|---|---|
| AI-20260930-01 | Deep-dive the architecture and finalize each component of the system | All — each member deep-dives their own components (D-20260930-05) | next advisor meeting (by 2026-10-14) | Open | [09-30 advisor](./2026-09-30-advisor.md) |
| AI-20260930-02 | Reword the priority/complexity ranking so it describes each category accurately | All | next advisor meeting (by 2026-10-14) | Open | [09-30 advisor](./2026-09-30-advisor.md) |
| AI-20260930-03 | Prototype Git observation and define the developer-agent interface | Anurag Bodapally | 2026-10-09 | Open | [09-30 team](./2026-09-30-team.md) |
| AI-20260930-04 | Define the API, evidence, verification and database contracts | Dhruv Verma | 2026-10-09 | Open | [09-30 team](./2026-09-30-team.md) |
| AI-20260930-05 | Convert the conflict taxonomy into positive and negative test scenarios | Shubham Baid | 2026-10-09 | Open | [09-30 team](./2026-09-30-team.md) |
| AI-20260930-06 | Select the first notification interface and replay scenario | Siddarth Vuppunahalli | 2026-10-09 | Open | [09-30 team](./2026-09-30-team.md) |
| AI-20260916-01 | Finalize model-selection criteria conditions and pick the top 5 models | All | before next advisor meeting | Open | [09-16](./2026-09-16-advisor.md) |
| AI-20260916-02 | Refine the initial idea into a finalized architecture | All | before next advisor meeting | In progress | [09-16](./2026-09-16-advisor.md) |
| AI-20260916-03 | Work out how the multi-part agent system is implemented (per-developer agent + orchestrator + verification agent) | Anurag Bodapally, Dhruv Verma, Siddarth Vuppunahalli — by component (D-20260930-05) | before next advisor meeting | In progress | [09-16](./2026-09-16-advisor.md) |
| AI-20260916-06 | Reverse engineer a situation per conflict category on a real repo and create a demo | All | before next advisor meeting | Open | [09-16](./2026-09-16-advisor.md) |
| — | Fork Campus Marketplace and freeze a `lab-baseline` SHA (never push to the Classroom original) | Dhruv Verma | before next advisor meeting | Open | [09-15](./2026-09-15-team.md) |
| AI-20260906-08 | Write the novelty-versus-prior-work comparison | Anurag Bodapally, Dhruv Verma | carried forward | Open | [09-06](./2026-09-06-team.md) |
| AI-20260906-09 | Assign owners for topics 7 and 8 | All | carried forward | Open | [09-06](./2026-09-06-team.md) |

Closed since last update: AI-20260916-05 (ranking, D-20260926-01), AI-20260926-01 and -02,
AI-20260916-04 (PostgreSQL, D-20260929-01) and AI-20260929-01 — all Done, see
[09-26](./2026-09-26-team.md) §1, [09-29](./2026-09-29-team.md) §1 and [09-30](./2026-09-30-advisor.md) §1.

## Related-Work Reading List

Two papers per member. Items 7–9 are **topic areas** to survey, not single papers.

| # | Paper / topic | Owner | Status |
|---|---|---|---|
| 1 | [AgenticFlict: A Large-Scale Dataset of Merge Conflicts in AI Coding Agent Pull Requests on GitHub](https://arxiv.org/pdf/2604.03551) | Dhruv Verma | Read — source of the 27.67% figure in the abstract |
| 2 | [Governed Shared Memory for Multi-Agent LLM Systems](https://arxiv.org/abs/2606.24535) (June 2026) | Anurag Bodapally | Read |
| 3 | [Collaborative Memory](https://arxiv.org/abs/2505.18279) | Anurag Bodapally | Read — findings posted 2026-09-09 |
| 4 | [How Much Coordination Gain Is Real?](https://arxiv.org/abs/2606.20695) | Shubham Baid | Read by Dhruv Verma / Anurag Bodapally in the pre-2026-09-04 push — confirm whether Shubham Baid still needs to cover it |
| 5 | Owhadi-Kareshk, Nadi, Rubin (2019), *Predicting Merge Conflicts in Collaborative Software Development* | Dhruv Verma | Read |
| 6 | Brindescu, Ahmed, Leano, Sarma (ICSE 2020), *Planning for Untangling: Predicting the Difficulty of Merge Conflicts* | Shubham Baid | Read — findings posted 2026-09-09 |
| 7 | *Topic:* follow-up work on social + technical features | Unassigned | Not started |
| 8 | *Topic:* failure attribution — bears on the replay/attribution direction, and on evaluation methodology either way | Unassigned | Not started |
| 9 | *Topic:* speculative execution and branch prediction | Siddarth Vuppunahalli | Surveyed — findings posted 2026-09-09 |
| 10 | [Cost-Aware Speculative Execution for LLM-Agent Workflows](https://arxiv.org/abs/2606.07846) | Siddarth Vuppunahalli | Read — findings posted 2026-09-09 |

Findings go in the [shared working doc](https://docs.google.com/document/d/1TOybmzXKISLafO4h4UUBEd6wmVYEWW1DAyPc77wV7UI/edit?tab=t.w61wv9ea0kaf).

## Decisions Log

| ID | Decision | Date | Note |
|---|---|---|---|
| D-20260901-01 | Explore Shubham Baid's proposed direction: coordination between developers each running their own AI coding agent | 2026-09-01 | [09-01](./2026-09-01-team.md) |
| D-20260901-02 | Carry two candidate directions into the first advisor meeting | 2026-09-01 | [09-01](./2026-09-01-team.md) |
| D-20260902-01 | Pursue the shared-context pre-merge coordination direction | 2026-09-02 | [09-02](./2026-09-02-advisor.md) |
| D-20260902-02 | Novelty bar: a few important SE innovations, and they must be evident | 2026-09-02 | [09-02](./2026-09-02-advisor.md) |
| D-20260902-03 | Intelligence/gating layer is optional and researchable, not a committed deliverable | 2026-09-02 | [09-02](./2026-09-02-advisor.md) |
| D-20260902-04 | Human approval stays in the loop for agent-initiated cleanup | 2026-09-02 | [09-02](./2026-09-02-advisor.md) |
| D-20260903-01 | Submit the Direction 1 abstract by Siddarth Vuppunahalli and Anurag Bodapally | 2026-09-03 | [09-03](./2026-09-03-team.md) |
| D-20260903-03 | Each member reads 1–2 related-work papers for the core idea (breadth over depth) | 2026-09-03 | [09-03](./2026-09-03-team.md) |
| D-20260904-01 | Locked final pitch structure and speaker split | 2026-09-04 | [09-04](./2026-09-04-team.md) |
| D-20260906-02 | Back the need with the 27.67% AI-agent merge-conflict figure | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-03 | Scope tracked artifacts to commits, branches, uncommitted diffs, interface changes, test results | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-04 | Rename project to "Verified Code-Change Awareness for AI-Assisted Software Teams" | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-05 | Frame the mechanism as an evidence gate rather than a merge queue | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-06 | Evaluate on reconstructed integration failures: time-to-notice and rework | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-07 | Revise the existing Direction 1 abstract rather than write a new one | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260906-08 | Revise live on the call as a full team; only final sign-off async in group chat | 2026-09-06 | [09-06](./2026-09-06-team.md) |
| D-20260912-01 | Ground scenario enumeration in real merge conflicts mined from public GitHub repositories | 2026-09-12 | [09-12](./2026-09-12-team.md) |
| D-20260912-02 | Use AI coding assistants to analyze repository git histories | 2026-09-12 | [09-12](./2026-09-12-team.md) |
| D-20260912-03 | Search for conflict types beyond the abstract, to broaden scope | 2026-09-12 | [09-12](./2026-09-12-team.md) |
| D-20260912-04 | Claim repositories in the shared doc before analyzing; document all work there | 2026-09-12 | [09-12](./2026-09-12-team.md) |
| D-20260912-05 | Team check-in 2026-09-15 before the 2026-09-16 advisor meeting | 2026-09-12 | [09-12](./2026-09-12-team.md) |
| D-20260915-01 | Adopt five primary groups as the classification of the mined corpus | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-02 | Record every incident as cause + interaction scope + outcome | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-03 | Mark change overlap and coordination/timing as small-repo-characteristic; coordination/timing subject to change | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-04 | Present three recurring use cases to the advisor, with the five-group table as backing | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-05 | Recreate the conflict categories on a codebase we can all run; large repos are problem definition, not evaluation | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-06 | Use a Campus Marketplace clone/fork frozen at `lab-baseline` as the replay fixture | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260915-07 | Four scripted lab tasks scored across four conditions (no feed / file overlap / ungated / gated) | 2026-09-15 | [09-15](./2026-09-15-team.md) |
| D-20260916-01 | Categorized corpus and recreate-on-a-fixture plan accepted as the project's direction | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-02 | Architecture is a multi-part agent system: per-developer agent above each coding agent, coordinated by an orchestrator | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-03 | Role-based shared context, enforced by a verification agent | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-04 | Rank conflict categories by complexity and priority before building; stay open to changing them | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-05 | Each category must be reverse-engineered on a real repo and demonstrated | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-06 | 295A = scope, problem definition, research, architecture (iterative); 295B = building. A prototype by end of 295A is welcome but not required — consolidation is the bar | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260916-07 | Campus Marketplace is the starting-point fixture; other codebases stay open as findings justify | 2026-09-16 | [09-16](./2026-09-16-advisor.md) |
| D-20260926-01 | Priority/complexity ranking: contract divergence 1, dependency propagation 2, change overlap 3, integration validation failure 4, coordination and timing 5 (subject to removal) | 2026-09-26 | [09-26](./2026-09-26-team.md) |
| D-20260926-02 | Integration validation failure is a common fourth layer checked across the other three, not an independent root cause | 2026-09-26 | [09-26](./2026-09-26-team.md) |
| D-20260926-03 | Five-stage pipeline: identification → validation before shared memory → scoring → notification → suggested action generation | 2026-09-26 | [09-26](./2026-09-26-team.md) |
| D-20260929-01 | PostgreSQL with JSONB columns for the shared-context store; MongoDB rejected | 2026-09-29 | [09-29](./2026-09-29-team.md) |
| D-20260929-02 | Local agents push context as a structured JSON manifest of metadata and evidence references — no raw source code | 2026-09-29 | [09-29](./2026-09-29-team.md) |
| D-20260930-01 | Spend the two weeks until the next advisor meeting on an architecture and component deep dive | 2026-09-30 | [09-30 advisor](./2026-09-30-advisor.md) |
| D-20260930-02 | Reword the priority/complexity ranking as the advisor asked; priorities unchanged unless the rewording shows otherwise | 2026-09-30 | [09-30 advisor](./2026-09-30-advisor.md) |
| D-20260930-03 | Workbook 1 chapter ownership — one named member does the final synthesis of each chapter | 2026-09-30 | [09-30 team](./2026-09-30-team.md) |
| D-20260930-04 | Detector order: change overlap first; contract divergence is the target detector for the 2026-12-04 demonstration; integration check planned for 295B | 2026-09-30 | [09-30 team](./2026-09-30-team.md) |
| D-20260930-05 | Prototype component ownership, one set of components per member | 2026-09-30 | [09-30 team](./2026-09-30-team.md) |
| D-20260930-06 | Architecture recorded in Workbook Ch5: layered, service-oriented, pipe-and-filter core; verification both local and central so raw code stays private | 2026-09-30 | [09-30 team](./2026-09-30-team.md) |
