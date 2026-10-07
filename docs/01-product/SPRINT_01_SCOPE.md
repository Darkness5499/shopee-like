# Sprint 01 — Product publication learning slice

> Superseded for scheduling by [Ordered learning plan](LEARNING_PLAN.md) on 2026-10-07. Account management is deferred; the active core is UC-004/005/006 with minimal read/audit support. Historical proposal below is retained as reference.

- Status: Proposed working scope; not an owner-approved release boundary.
- Owner: Project owner.
- Phase: Phase 2 requirements handoff into Phase 3 domain and system design.
- Goal: learn system design through one small, demonstrable vertical slice: a verified user creates a product with one SKU, the product is approved, and a buyer discovers and views it.

## In scope

- UC-001 — account, verification, profile, and delivery address basics.
- UC-004 — product and single-SKU maintenance.
- UC-005 — product submission for review.
- UC-006 — moderator approval or rejection.
- UC-008 — product discovery.
- UC-009 — product detail and purchase conditions.
- UC-024 — audit records for the slice's state-changing actions.

UC-002, UC-003, and UC-007 are prerequisites or design references only in this sprint. Use one seeded Shop, one seeded category, and one seeded SKU where that keeps the learning slice small; do not build Shop administration, category administration, inventory reservation, checkout, payment, fulfillment, shipping, refund, or dispute behavior in Sprint 01.

## Deferred to later sprints

- Sprint 02: UC-002/003 plus the permission and category administration needed to replace seeded data.
- Sprint 03: UC-007/010/011/012 — inventory, cart, checkout, and simulated payment.
- Sprint 04: UC-013/014/015 — Seller fulfillment, simulated shipment, tracking, cancellation, and completion.
- Sprint 05: UC-016/017 — return, dispute, refund, and financial disposition.
- Later: UC-018 through UC-023 — reviews, promotions, violations, interventions, and reporting.

Deferred use cases remain in the product baseline and catalog. Their existing Draft specifications are reference material only; they are not Sprint 01 commitments.

## Design learning outputs

1. Model the bounded domain: User, Shop reference, Category reference, Product, SKU, Submission, and AuditEntry.
2. Produce a context/container/component sketch and a small state-transition table for Product and Submission.
3. Define API/application boundaries for create/update, submit, moderate, discover, and view.
4. Define persistence ownership, validation boundaries, authorization checks, and audit events.
5. Build the slice with seeded data and verify the acceptance criteria linked from the seven in-scope use cases.

## Explicit non-goals

No multi-Shop checkout, stock reservation, real or simulated payment integration, logistics integration, refund execution, production deployment, or acceptance of unresolved product policies is required for Sprint 01. New policy discovered during design goes to `OPEN_DECISIONS.md`; it is not silently committed in code.
