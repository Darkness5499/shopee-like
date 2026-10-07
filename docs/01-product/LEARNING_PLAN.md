# Ordered learning plan

- Status: Accepted direction to narrow scope and defer unfinished features; technical details remain Draft.
- Owner: Project owner.
- Source: owner instruction on 2026-10-07 to focus on a few complex core features, close unfinished use cases for current work, and proceed to analysis/design.
- Authority: supersedes the seven-use-case Sprint 01 proposal and the broad all-P0 release proposal for current scheduling. Existing requirement IDs and recorded policy decisions remain intact.

## Current progress

Product scope, glossary, catalog, business rules, state models, NFRs and 18 detailed Draft use cases exist. Six other catalog entries have no detailed specification. All 24 use cases are **Not implemented**; no application code or executed acceptance evidence exists. Document completion is not feature completion.

## Ordered parts

| Part | Core problem and learning value | Use cases | Execution status |
|---|---|---|---|
| 1 — Publication and moderation | Immutable submissions, concurrent withdraw/approve/reject, idempotency, immediate hiding after risky edits, atomic publication and audit | UC-004/005/006; minimal UC-008/009 reads and UC-024 audit | Active: Phase 3 analysis; not implemented |
| 2 — Inventory and purchasing | Prevent overselling, reserve/release/consume stock, atomic checkout, price snapshots | UC-007/010/011 | Deferred; not implemented |
| 3 — Simulated payment | Verified callbacks, duplicates, late outcomes and recovery | UC-012 | Deferred; not implemented |
| 4 — Fulfillment and shipment | Independent order/shipment states, event ordering and recovery | UC-013/014/015 | Deferred; not implemented |
| 5 — Financial disposition | Refund caps, idempotent execution and physical return accounting | UC-016/017 | Deferred; not implemented |
| Optional foundations and extensions | Full account/Shop/category management, reviews, vouchers, sanctions, interventions and reporting | UC-001/002/003/018–023; advanced UC-008/009/024 | Deferred; not implemented; no scheduled sprint |

The order is a learning sequence, not a commitment to deliver every part. Finish a bounded analysis/design/build/check cycle before selecting another part. Do not expand deferred specifications or reopen their decisions automatically.

## Part 1 boundary

Use seeded Seller, Moderator and Buyer identities, Shop, categories and SKU data. Identity ownership and authorization checks remain required, but registration, address management, Shop staff administration and category CRUD are deferred. Start with one Product/SKU; existing risky-edit and moderation rules apply. Discovery/detail provide only enough read behavior to demonstrate that hidden content stays unavailable and approved content becomes visible. Audit covers only the active commands.

## Next phase

Start Phase 3 for Part 1 with [domain and concurrency analysis](../03-analysis/PUBLICATION_ANALYSIS.md). Then Phase 4: choose transaction boundaries, API/data contracts, conflict handling and a proportionate architecture with rationale. Then implement and test the selected behavior. Phase 3 start does not claim a passed whole-MVP Phase 2 gate or accepted unresolved assumptions.

## Scope control

Preserve detailed drafts as reference, with explicit Not implemented / Deferred status. Existing Accepted business choices retain provenance, including those for deferred journeys; their acceptance does not schedule implementation. New capabilities require an owner request before entering active work.

## Learner and assistant agreement

- Goal: develop the ability to analyze, design, implement and explain a system, including alternatives, failure modes and operational consequences.
- The owner is the learner and decision-maker. Write a first attempt, explain the reasoning, implement the core mechanism and run the experiments. AI-generated documents are starting material to challenge, not evidence of understanding.
- The assistant asks focused questions, reviews diagrams/code, introduces counterexamples, explains gaps and assists with explicitly requested scaffolding or fixes. It does not automatically finish the next phase or provide a full solution before the owner attempts the exercise, unless explicitly requested.
- Use one bounded exercise at a time. Progress depends on explanation and evidence, not the amount of documentation produced. Advanced technology is justified by a concrete problem, not added as a learning badge.
- Learner experience, preferred stack and available study time are not yet recorded. Adjust exercises when these are known; no duration or delivery deadline is promised.

## What each part teaches

| Part | Learner experiment | Design question to defend |
|---|---|---|
| 1 | Race approve against withdraw, repeat a command after losing its response, read immediately after risky editing | Where is the consistency boundary? How do state/version checks and operation identity prevent conflicting or repeated effects? |
| 2 | Two buyers compete for one unit; expire or release holds and reconcile totals | Which invariant prevents overselling? What must one checkout transaction own? |
| 3 | Replay and reorder simulated callbacks; crash before/after applying payment | How are trusted financial evidence, idempotency, recovery and purchase state separated? |
| 4 | Deliver shipment events out of order; recover gaps without regressing progress | What owns physical versus business state, and what consistency is required across them? |
| 5 | Race refund requests and inspect returned items | How are outstanding commitments, verified funds and stock receipt accounted without duplicate effects? |

Part 1 is the only current commitment. Parts 2–5 are an ordered optional backlog. Full registration, Shop/category administration, promotions and reporting are not required to learn these mechanisms.

## Phases for every selected part

| Phase | Owner's work | Minimal output and evidence | Assistant's work |
|---|---|---|---|
| 3 — Domain/process analysis (current) | Restate scenarios; draw entities and states; identify ownership and invariants; walk through competing actions | Own diagram, transition table, and explanation of happy/failure/race paths; assumptions linked | Review gaps and challenge with counterexamples |
| 4 — Solution design | Draw context/container and key sequence; compare two viable approaches; choose transaction and API/data boundaries; reason about scale, security, cost and operation | Small design, draft ADR with alternatives/trade-offs, schema/API outline and failure handling; owner can defend choices | Critique consequences and help test an uncertain mechanism with a bounded spike |
| 5 — Implementation | Select a stack with rationale; write the core mechanism; trace execution and inspect persistence; request scaffolding help when useful | Runnable slice, own code explanation, mapped acceptance scenarios and required checks | Review code and help diagnose targeted failures |
| 6 — Verification/release/learning | Execute races/replays and interruption tests; inspect audit/metrics; deploy a bounded demo with recovery plan; explain measured results | Reproducible commands, observed results, demo, rollback/recovery evidence and lessons | Review experiment design/results; assist deployment when requested |

Requirements clarification can occur within the active part when a real ambiguity appears. A drafted diagram is not a passed phase. Move on when the owner can explain the behavior and evidence, and the scoped prerequisites are sufficient.

## Part 1 exercises and checkpoints

1. **Domain exercise:** sketch Product versus Submission and show why submitted content needs a stable revision. Explain ownership and what is mutable.
2. **Race exercise:** simulate approve/withdraw/reject against one pending submission. Draw two arrival orders, the winning action, the refused action and the repeated winning request.
3. **Design exercise:** compare optimistic conditional updates and locking in a single transactional store. State conflict responses, transaction contents and retry identity. Consider distributed components only if the chosen requirements justify them.
4. **Implementation exercise:** build save/submit/withdraw/approve/reject plus minimal public reads, seeded permissions and audit. Explain the critical code rather than only presenting generated output.
5. **Verification exercise:** run simultaneous commands, lost-response replay and immediate-hidden-read scenarios. Inspect stored content, final state and the number of effective audit records.

First session: owner attempts exercises 1–2 before receiving a complete solution. Existing [publication analysis](../03-analysis/PUBLICATION_ANALYSIS.md) is a Draft reference to critique; change it when the owner's analysis exposes a gap.
