# MVP Release Boundary and Quality Proposal

- Status: Proposed — review package prepared 2026-10-07 for [WI-003](../WORK_ITEMS.md#wi-003); no owner decision is recorded.
- Owner: Project owner.
- Source: [Product Vision](PRODUCT_VISION.md), [Functional Scope](FUNCTIONAL_SCOPE.md), [catalog priorities](../02-requirements/USE_CASE_CATALOG.md#prioritization-proposal), [OQ-014](../02-requirements/OPEN_DECISIONS.md#oq-014), [OQ-012](../02-requirements/OPEN_DECISIONS.md#oq-012).

This document helps the owner decide what the first end-to-end demo must prove. It does not accept P0/P1 priorities, remove a capability from the product baseline, or record a schedule. No effort budget has been supplied; slices are ordered by dependency, not by estimated cost.

## Recommended first demo ("Release 1")

**Demo statement:** a Seller opens a Shop and publishes a stocked Product; a Moderator approves it; a Buyer finds it, buys it with one multi-Shop checkout and a simulated payment; the Seller fulfills it; simulated logistics deliver it; the Buyer completes the Order or opens a dispute that Support resolves with a simulated refund — with every step auditable and every partner callback safe against duplicates.

### Delivery slices (dependency order)

| Slice | Journey | Use cases | Proves |
| :--- | :--- | :--- | :--- |
| A | Foundation and publication | UC-001, UC-002, UC-003, UC-004, UC-005, UC-006, UC-024 | Accounts, Shop isolation, dynamic category attributes, moderation, audit |
| B | Shopping | UC-008, UC-009, UC-010 | Category/attribute discovery, SKU purchase conditions, multi-Shop cart |
| C | Purchase | UC-007, UC-011, UC-012 | Stock consistency under contention, multi-Shop checkout, idempotent simulated payment |
| D | Fulfillment | UC-013, UC-014, UC-015 | Order/payment/shipment state separation, ordered logistics events, cancellation races |
| E | After-sales | UC-016, UC-017 | Completion vs dispute, refund caps, physical return restock |

Each slice is a vertical, demonstrable increment; slice readiness is checked with the [handbook](../PRODUCT_DEVELOPMENT_PROCESS.md) using the named slice's own requirements and decisions.

### Included simplifications (conditional on owner confirmation)

- One simulated Payment Provider with success, failure and expiry outcomes; one simulated Logistics Partner with one whole Shipment per Shop order.
- Flat shipping quote per Shop order (a fixed configured fee) instead of a calculation engine; the quote still follows the snapshot and validity rules.
- Checkout without vouchers (P1 `UC-019/020`); the checkout specification keeps a stable extension point and the full scope is not removed.
- Single currency (VND, whole units) per [OQ-002](../02-requirements/OPEN_DECISIONS.md#oq-002).

### Deferred to later increments (still in the product baseline)

| Capability | Use cases | Reason it can follow Release 1 |
| :--- | :--- | :--- |
| Reviews and ratings | UC-018 | Needs completed purchases; discovery shows "no rating" until then |
| Shop vouchers / platform promotions | UC-019, UC-020 | Rule engine needs OQ-011; affects checkout totals and refund allocation |
| Violations and sanctions | UC-021 | Core flows carry only the state guards in SM-SHOP-001/SM-ACCOUNT-001 |
| Exceptional order intervention | UC-022 | Release 1 uses dispute adjudication and reconciliation disposition |
| Shop reporting | UC-023 | Needs settled revenue/refund treatment |

## Open decisions Release 1 depends on

Recommended confirmation order (each has prepared options in the [decision register](../02-requirements/OPEN_DECISIONS.md)):

1. OQ-014 (this proposal), then OQ-007/OQ-008 (checkout grouping, stock timing), because slices C–E depend on them.
2. OQ-009/OQ-010/OQ-015 (closure, after-sales, partner events).
3. OQ-005/OQ-006/OQ-002/OQ-020/OQ-021 (permissions, SKU identity, validation limits, accounts, categories), because slices A–B depend on them.
4. OQ-012 (quality targets) before test strategy in Phase 4.

Authorized Draft work and provisional exploration continue while these remain Open.

## Quality targets and measurement plan

All targets are Proposed learning-demo goals, not production commitments. Measurements follow the [NFRs](../02-requirements/NON_FUNCTIONAL_REQUIREMENTS.md); this table adds the Release 1 evidence each one needs.

| NFR | Target (Proposed) | Release 1 evidence | Earliest verification |
| :--- | :--- | :--- | :--- |
| NFR-001 Authorization/isolation | Zero unauthorized successes in the [role matrix](../02-requirements/BUSINESS_RULES.md#br-access-001) across two Shops and four internal roles | Allow/deny test table generated from the matrix | Slice A, then every slice |
| NFR-002 Partner outcome safety | 10 replays of one logical event, including concurrent, give one effective transition; invalid signature changes nothing | Payment and logistics replay scenarios | Slice C/D |
| NFR-003 Inventory consistency | Two simultaneous buyers of one unit: at most one hold; ledger equals expected totals after fail/expire/confirm/return | Contention and lifecycle replay | Slice C/D/E |
| NFR-004 Audit completeness | 100% of the [audit inventory](../02-requirements/BUSINESS_RULES.md#br-audit-001) has a record | Inventory-to-record checklist | Per slice |
| NFR-005 Response time | Discovery/detail p95 ≤ 2 s at 100 Shops, 10,000 SKUs, 20 concurrent users | Repeatable load run, environment recorded | Slice B |
| NFR-006 Recovery | Interrupted accepted outcome recovered within 60 s without duplicate effects | Interrupt/restart scenarios | Slice C/D |

### Owner confirmation checklist for OQ-012

- Accept or change the workload (100 Shops, 10,000 SKUs, 20 users) and the 2 s p95 target.
- Accept or change the 60 s recovery target and the 10-replay count.
- Choose audit retention (suggested: the lifetime of the demo environment) and failed-attempt breadth.
- Confirm that documentation checks never count as behavioral evidence.

## Demo acceptance walkthrough (Proposed)

Release 1 is demonstrable when one scripted run completes, with audit records and no manual data repair: (1) register and verify two Users; (2) Seller registers a Shop and gets approval; (3) Admin category with a required attribute exists; (4) Seller publishes a SKU with stock and Moderator approves; (5) Buyer carts items from two Shops and checks out once; (6) payment succeeds through a duplicated callback with one effect; (7) each Shop confirms, ships and is delivered through ordered events; (8) one Order completes and one opens a dispute resolved by partial refund; (9) stock, payment and refund totals reconcile.

## Review guidance

Choose one: **accept** the Release 1 boundary and deferrals; **accept with changes** (for example pull UC-018 forward or push UC-017 to the next increment); or **ask for another option**. Record the choice and date in OQ-014/OQ-012 before Phase 2 exit; until then this is a Proposal.

# Scheduling notice

The broad release proposal below is deferred. [Ordered learning plan](LEARNING_PLAN.md) is authoritative for current work; no all-P0 implementation commitment exists.
