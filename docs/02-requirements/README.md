# Requirements Specification

This directory captures what the system must do and the rules it must satisfy. It deliberately avoids database, API, framework, and microservice decisions.

- Status: Draft — Phase 2 in progress; exit gate not passed.
- Owner: Project owner.

## Available artifacts

- [Glossary](GLOSSARY.md): canonical terms and proposed working definitions.
- [Use Case Catalog](USE_CASE_CATALOG.md): 24 catalog entries covering 26 scope capabilities; P0/P1 priorities are Proposed. UC-024 coordinates cross-cutting audit coverage rather than a separate user flow.
- [Business Rules](BUSINESS_RULES.md): preserved `BR-PROD-001`; `BR-PROD-002` records Accepted hiding from successful save of an actual high-risk content change until revised content receives Moderator approval.
- [Business Rules](BUSINESS_RULES.md#br-prod-003): `BR-PROD-003` records Accepted withdrawal/edit/resubmit and deterministic competing/repeated actions.
- [Public listing and purchase eligibility](BUSINESS_RULES.md#br-prod-004): `BR-PROD-004` is Proposed for shared discovery/detail/cart/checkout guards.
- [Purchase rules](BUSINESS_RULES.md#br-inv-001): `BR-INV-001`, `BR-ORDER-001` and `BR-PAY-001` are Proposed for protected inventory, selected checkout/snapshots and verified simulated payment/recovery.
- [Fulfillment and closure rules](BUSINESS_RULES.md#br-order-002): `BR-ORDER-002`, `BR-CANCEL-001` and `BR-SHIP-001` propose Seller response/packing, cancellation/rejection/timeout with REQUIRED compensation, and ordered simulated delivery.
- [After-sales and financial disposition rules](BUSINESS_RULES.md#br-aftersales-001): `BR-AFTERSALES-001`, `BR-REFUND-001` and `BR-STOCK-RECEIPT-001` propose delivery completion, dispute intake windows, verified paid-funds caps, idempotent simulated refund execution, and physical return restock controls.
- [State Machines](STATE_MACHINES.md): Product model Draft with Accepted visibility policy; separate inventory, reservation, Shop-order purchase/fulfillment/closure, payment, Shipment, after-sales dispute (`SM-DISPUTE-001`), and refund execution (`SM-REFUND-001`) models are Proposed.
- [Non-Functional Requirements](NON_FUNCTIONAL_REQUIREMENTS.md): six measurable quality drafts/proposals.
- [Open Decisions](OPEN_DECISIONS.md): policy questions, proposals, and confirmation provenance.
- [Traceability](TRACEABILITY.md): scope coverage, acceptance links, missing work, and exit-gate checklist.

## Detailed use cases started

- [UC-004 — Create or update Product/SKUs](USE_CASES/UC-004-create-update-product.md)
- [UC-005 — Submit a Product for review](USE_CASES/UC-005-submit-product.md)
- [UC-006 — Review a Product submission](USE_CASES/UC-006-review-product.md)
- [UC-007 — Adjust and protect SKU inventory](USE_CASES/UC-007-manage-inventory.md)
- [UC-008 — Discover Products](USE_CASES/UC-008-discover-products.md)
- [UC-009 — View Product purchase conditions](USE_CASES/UC-009-view-product.md)
- [UC-010 — Manage a multi-shop cart](USE_CASES/UC-010-manage-cart.md)
- [UC-011 — Checkout a multi-shop cart](USE_CASES/UC-011-checkout.md)
- [UC-012 — Pay and process simulated payment outcomes](USE_CASES/UC-012-process-payment.md)
- [UC-013 — Confirm and fulfill a Shop order](USE_CASES/UC-013-fulfill-shop-order.md)
- [UC-014 — Create and track a simulated shipment](USE_CASES/UC-014-simulate-shipment.md)
- [UC-015 — Track, cancel, and complete a purchase](USE_CASES/UC-015-track-cancel-orders.md): tracking, cancellation, and delivery completion drafted.
- [UC-016 — Request a return, refund, or dispute](USE_CASES/UC-016-request-return-refund.md)
- [UC-017 — Resolve a refund/dispute and simulate refund execution](USE_CASES/UC-017-resolve-refund-dispute.md)

Acceptance criteria are embedded in each detailed use case using `AC-UC-###-##`. They are draft requirements, not executed test results. Baseline statements, new proposals, and owner-confirmed decisions are distinguished using the [documentation conventions](../DOCUMENTATION_ARCHITECTURE.md).

## Next work

Current work follows the connected cross-actor journey: Seller UC-004/005 → Moderator UC-006 → Buyer UC-008/009/010 → Inventory/Checkout/Payment UC-007/011/012 → Fulfillment/Shipment/Tracking UC-013/014/015 → After-Sales/Refund UC-016/017. All fourteen use cases now have detailed Draft specifications (229 criteria total).

WI-005 Draft preparation is complete, connecting delivery completion, dispute intake, Seller response/timeout auto-approval, physical return inspection/restock, Support adjudication, and simulated refund execution under verified paid funds caps. The review package for OQ-007, OQ-008, OQ-009, OQ-010, and OQ-015 is prepared for owner review.

Next review the linked open decisions with the project owner to obtain policy confirmation for the core MVP journey. [WI-003](../WORK_ITEMS.md#wi-003) remains the release-boundary and quality target review. Once requirements are agreed, Phase 3 (Domain and Process Analysis) can begin.

The full-MVP checklist in [TRACEABILITY.md](TRACEABILITY.md#phase-2-exit-gate-checklist) stays unpassed until the chosen MVP is fully specified and reviewed. Domain/UX/feasibility exploration can refine Draft requirements now; a selected slice can proceed to committed analysis/design when its own relevant rules, criteria and blocking decisions are sufficiently resolved and readiness evidence recorded. See the [handbook](../PRODUCT_DEVELOPMENT_PROCESS.md).
