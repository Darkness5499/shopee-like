# Requirements Specification

Current scheduling follows [Ordered learning plan](../01-product/LEARNING_PLAN.md). All use cases are Not implemented. Requirements expansion is closed for current work; Phase 3 has started for UC-004/005/006, with minimal read/audit support. Account, Shop, category administration and all purchase/downstream journeys are Deferred. Earlier broader scope descriptions below are retained inventory, not the current queue.

This directory captures what the system must do and the rules it must satisfy. It deliberately avoids database, API, framework, and microservice decisions.

- Status: Draft requirements inventory; Part 1 is in provisional Phase 3. Whole-MVP requirements gate not passed.
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
- [Foundation rules](BUSINESS_RULES.md#br-access-001): `BR-ACCESS-001`, `BR-SHOP-001`, `BR-CATEGORY-001` and `BR-AUDIT-001` propose accounts/addresses with the Shop and internal permission matrix, Shop registration, category hierarchy/schemas, and the audit inventory. `SM-ACCOUNT-001` and `SM-SHOP-001` are Proposed in [State Machines](STATE_MACHINES.md#sm-account-001).
- [State Machines](STATE_MACHINES.md): Product model Draft with Accepted visibility policy; separate inventory, reservation, Shop-order purchase/fulfillment/closure, payment, Shipment, after-sales dispute (`SM-DISPUTE-001`), and refund execution (`SM-REFUND-001`) models are Proposed.
- [Non-Functional Requirements](NON_FUNCTIONAL_REQUIREMENTS.md): six measurable quality drafts/proposals.
- [Open Decisions](OPEN_DECISIONS.md): policy questions, proposals, and confirmation provenance.
- [Traceability](TRACEABILITY.md): scope coverage, acceptance links, missing work, and exit-gate checklist.

## Detailed use cases started

- [UC-001 — Manage account, profile, and delivery addresses](USE_CASES/UC-001-manage-account-addresses.md)
- [UC-002 — Register and manage a Shop and its operators](USE_CASES/UC-002-register-manage-shop.md)
- [UC-003 — Maintain categories and attribute definitions](USE_CASES/UC-003-maintain-categories.md)
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
- [UC-024 — Record and inspect auditable business actions](USE_CASES/UC-024-record-audit.md)

Acceptance criteria are embedded in each detailed use case using `AC-UC-###-##`. They are draft requirements, not executed test results. Baseline statements, new proposals, and owner-confirmed decisions are distinguished using the [documentation conventions](../DOCUMENTATION_ARCHITECTURE.md).



<a id="next-work"></a>
## Current handoff

Use [Learning Plan](../01-product/LEARNING_PLAN.md), not the older broad P0 queue. Core UC-004/005/006 go to [Part 1 analysis](../03-analysis/PUBLICATION_ANALYSIS.md); minimal UC-008/009 reads and scoped UC-024 audit support the demonstration. All other use cases are Deferred and Not implemented.

Recorded policy decisions are read from [Open Decisions](OPEN_DECISIONS.md); most OQs now carry Accepted provenance, while OQ-011/013 remain Open/deferred. Older Proposed wording in a Draft UC/rule/state does not reopen an accepted decision, and a decision does not automatically approve an entire specification. Technical details and state-code assumptions still need scoped review.

Next learner task: draw Product/Submission ownership and trace competing approval/withdrawal. Full downstream requirements consistency has not been certified; deferred specifications require a fresh review before their part is selected.
