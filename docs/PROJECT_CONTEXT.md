# Project Context

- Status: Draft working snapshot, reviewed 2026-10-07 (Asia/Ho_Chi_Minh).
- Owner: Project owner.
- Purpose: current facts and the next learning action; history lives in [Work Log](WORK_LOG.md).

## Goal and working agreement

The owner is learning **system design and solution architecture through hands-on work** on a small marketplace demo. Focus on a few technically demanding behaviors. The owner explains requirements, proposes models, makes trade-offs, writes core code, runs experiments and defends the result. The assistant coaches, reviews and helps with bounded tasks; do not automatically deliver subsequent designs or implementation in place of the learner.

Use Vietnamese in discussion and English in repository artifacts. Follow the [learning plan](01-product/LEARNING_PLAN.md) and the [handbook](PRODUCT_DEVELOPMENT_PROCESS.md). New features and deferred journeys enter active work only when the owner selects them.

## Current state

- **Phase 3 — Domain and Process Analysis, in progress for Part 1 only.** [Publication analysis](03-analysis/PUBLICATION_ANALYSIS.md) is an assistant-prepared Draft to critique, not evidence that the learner completed the analysis.
- Active core: UC-004/005/006 Product editing, immutable submission, moderation and withdrawal. UC-008/009 provide minimal listing/detail reads; UC-024 supplies scoped audit.
- Seed actor identities, Shop, category and SKU references. Full account/Shop/category administration and purchasing/downstream journeys are Deferred.
- 24 catalog entries; 18 detailed Draft specifications with 284 AC definitions. **All 24 features are Not implemented**. Specification coverage is separate from implementation and learning progress.
- Phase 4 design, application implementation and executed acceptance/performance evidence are pending. No stack, database or deployment architecture has been chosen.
- Whole-MVP Phase 2 readiness has not passed. Part 1 analysis is provisional; technical contracts need scoped readiness and explicit treatment of assumptions.
- [Open Decisions](02-requirements/OPEN_DECISIONS.md) records Accepted policy provenance for OQ-001–010/012/014–021; OQ-011/013 remain Open and deferred. Do not call recorded Accepted choices Open, or infer that every detail in a Draft rule/state model is Accepted.
- All previous broad release/sprint proposals are reference material superseded for scheduling by the learning plan.

<a id="next-action"></a>
## Next session — 2026-10-08

Resume [WI-007](WORK_ITEMS.md#wi-007), Phase 3. Read only LEARNING_PLAN, UC-004/005/006 and BR-PROD-002/003 first. The owner draws Product/Submission relationships and allowed transitions, then traces simultaneous approval and withdrawal. Explain which action wins, what changes atomically, and what an identical retry returns. Bring a diagram and a short explanation before requesting an AI solution. The assistant reviews counterexamples and helps revise the owner's model.

Next move to Phase 4 after the owner can explain the model/invariants and outstanding assumptions. Do not preemptively choose technology or implement deferred features. Learner skill level, preferred stack and time budget have not been supplied; discover them during the next session.

## Resume and evidence

- [Learning plan](01-product/LEARNING_PLAN.md): ordered parts, learner/assistant roles and completion evidence.
- [Work Items](WORK_ITEMS.md): actual work and scoped readiness.
- [Traceability](02-requirements/TRACEABILITY.md): requirement-to-analysis/design/test coverage.
- [Open Decisions](02-requirements/OPEN_DECISIONS.md): policy choices; [ADRs](04-solution-design/ADR/README.md): future technical rationale.
- [Work Log](WORK_LOG.md): dated handoffs; commit/push results are verified from Git rather than predicted in this snapshot.
