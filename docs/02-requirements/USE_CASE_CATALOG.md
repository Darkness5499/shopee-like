# Use Case Catalog

## Current execution authority

[Ordered learning plan](../01-product/LEARNING_PLAN.md) supersedes the scheduling proposals below. **All 24 entries are Not implemented.** Only UC-004/005/006 are active core features; UC-008/009 are minimal read support and UC-024 provides scoped audit. UC-001/002/003/007/010–023 and advanced read/audit features are **Deferred — closed for current work**. Do not draft or implement them until a later part is explicitly selected.

- Status: Draft catalog; implementation status and active scheduling are separate from recorded policy decisions.
- Owner: Project owner.
- Source: [Functional Scope](../01-product/FUNCTIONAL_SCOPE.md), [Product Vision](../01-product/PRODUCT_VISION.md), and [actors](../01-product/STAKEHOLDERS_AND_ACTORS.md).

## Prioritization proposal

### Active Part 1

UC-004/005/006 are the core learning features. Minimal UC-008/009 reads and scoped UC-024 audit support them. Seeded actor, Shop, category and SKU references replace full administration in this part. The owner attempts the domain/design exercises and core implementation; the assistant reviews and assists bounded work.

### Historical broad release classification

**P0 — Initial end-to-end demo:** authorized shop setup, category/product/SKU/stock maintenance, moderation, buyer discovery and multi-shop checkout, simulated payment, seller fulfillment, shipment tracking, and a minimal refund/dispute path. This preserves the vision's purchase-to-completion-or-dispute outcome.

**P1 — Subsequent demo increments:** reviews, voucher/promotion management, violation handling, exceptional staff order interventions, and reporting. These remain in scope; P1 is not an exclusion. Voucher application in checkout is specified when the voucher rules are ready, not assumed away permanently.

[OQ-014](OPEN_DECISIONS.md#oq-014) records baseline/MVP confirmation. This broad classification is deferred scheduling reference; the Learning Plan controls current work. Items below are a requirements inventory, not an implementation backlog. A broad entry can be split into additional stable use case IDs during specification.

## P0 inventory

### UC-001 — Manage account, profile, and delivery addresses

- Primary actor: User acting as Buyer or Seller.
- Scope: SC-001.
- Goal: establish account access and maintain profile/address information for permitted actions.
- Detail: [Draft specification](USE_CASES/UC-001-manage-account-addresses.md), 14 Proposed criteria for registration, simulated verification, sign-in lock, profile and addresses under OQ-020/OQ-005.

### UC-002 — Register and manage a Shop and its operators

- Primary actor: Seller; supporting actors: shop staff and authorized Internal Staff.
- Scope: SC-009, SC-016, SC-023.
- Goal: register a Shop, assign permitted operators, and complete required Shop moderation.
- Detail: [Draft specification](USE_CASES/UC-002-register-manage-shop.md), 16 Proposed criteria for Shop registration/review, operator roles and cross-Shop isolation (BR-SHOP-001, BR-ACCESS-001, SM-SHOP-001). Product moderation is UC-006, not a reused Shop state machine; sanctions remain UC-021/OQ-004.

### UC-003 — Maintain categories and attribute definitions

- Primary actor: Admin.
- Scope: SC-017; supports SC-002/SC-010.
- Goal: maintain the category hierarchy and category-specific required/optional attributes.
- Detail: [Draft specification](USE_CASES/UC-003-maintain-categories.md), 13 Proposed criteria for hierarchy, versioned attribute schemas, deactivation effects and Admin-only access (BR-CATEGORY-001) under OQ-002/OQ-021.

### UC-004 — Create or update a Product and its SKUs

- Primary actor: Seller / Shop Operator.
- Scope: SC-010, SC-011.
- Goal: maintain product content, variants, SKU prices, activation, and internal codes.
- Detail: [Draft specification](USE_CASES/UC-004-create-update-product.md). Inventory quantity changes use UC-007; submitting content uses UC-005.
- Open decisions: OQ-002/OQ-005/OQ-006.
- Accepted policy: BR-PROD-002 defines hiding until approval. BR-PROD-003/OQ-003/OQ-018 defines withdrawal before further risky edits, first-accepted-action-wins, and no additional effects from repeated actions.

### UC-005 — Submit a Product for review

- Primary actor: Seller / Shop Operator.
- Scope: SC-010, SC-011, SC-016.
- Goal: validate product content and send a valid submission to manual moderation.
- Detail: [Draft specification](USE_CASES/UC-005-submit-product.md).
- Open decisions: OQ-002/OQ-005.
- Accepted policy: BR-PROD-002 defines hiding through validation/review/rejection until approval. BR-PROD-003/OQ-018 requires withdrawal before further risky edits and fresh validation/resubmission afterward.

### UC-006 — Review a Product submission

- Primary actor: Moderator.
- Scope: SC-016, SC-023.
- Goal: approve a validated submission or reject it with a reason.
- Detail: [Draft specification](USE_CASES/UC-006-review-product.md).
- Open decisions: OQ-002/OQ-004/OQ-005.
- Accepted policy: BR-PROD-002 defines visibility until approval. BR-PROD-003/OQ-003/OQ-018 makes the first accepted withdrawal/approval/rejection final for that submission; later conflicts are refused and repeats have no additional effects.

### UC-007 — Adjust and protect SKU inventory

- Primary actor: Seller; supporting flows: checkout, payment, and order confirmation.
- Scope: SC-012.
- Goal: stock-in/adjust stock and correctly reserve, release, or deduct inventory during purchasing.
- Detail: [Draft specification](USE_CASES/UC-007-manage-inventory.md), Proposed protected unpaid/paid holds, stock-in/adjustments, contention, release, and deduction only at Seller confirmation. Timing/invariants and cancellation/restock remain OQ-008/OQ-009/OQ-010; valid units, permissions, and SKU identity remain OQ-002/OQ-005/OQ-006.

### UC-008 — Discover Products

- Primary actor: Buyer.
- Scope: SC-002.
- Goal: browse categories and search/filter by supported category attributes, price, rating, and Shop.
- Detail: [Draft specification](USE_CASES/UC-008-discover-products.md), with Accepted review-related hiding and Proposed public-listing/out-of-stock guards in BR-PROD-004. Attribute definitions depend on UC-003, ratings on OQ-013, and quality targets on OQ-012.
- Open decisions: OQ-002/004/005/006/008/012/013/019.

### UC-009 — View a Product and its purchase conditions

- Primary actor: Buyer.
- Scope: SC-003.
- Goal: inspect product/SKU choices, availability, reviews, vouchers, and Shop policies.
- Detail: [Draft specification](USE_CASES/UC-009-view-product.md), with Accepted hiding from successful risky saving through manual approval and Proposed direct-lookup, SKU-choice, and purchase guards in BR-PROD-004. Review and voucher calculations remain open.
- Open decisions: OQ-002/004/005/006/007/008/009/010/011/012/013/019.

### UC-010 — Manage a multi-shop cart

- Primary actor: Buyer.
- Scope: SC-004.
- Goal: add, update, and remove intended SKU purchases across Shops.
- Detail: [Draft specification](USE_CASES/UC-010-manage-cart.md), with no stock reservation from cart membership, Accepted blocking of previously carted review-hidden Products, and Proposed cart ownership/current-price/unavailable-item presentation under OQ-005/OQ-019. Final revalidation and reservation remain UC-011.
- Open decisions: OQ-002/004/005/006/007/008/012/019.

### UC-011 — Checkout a multi-shop cart

- Primary actor: Buyer.
- Scope: SC-005, SC-012.
- Goal: validate inventory, calculate shipping and voucher effects, reserve stock, and produce orders grouped per Shop.
- Detail: [Draft specification](USE_CASES/UC-011-checkout.md), Proposed all-or-nothing selected checkout, per-Shop Orders/one payment group, final revalidation, snapshots, repeat recovery and checkout-versus-edit ordering under OQ-007/OQ-008. Accepted BR-PROD-002 blocks new purchases after successful risky saving, including previously carted items. Concrete shipping/voucher calculations remain incomplete under OQ-007/OQ-011; the conditional no-voucher walkthrough does not complete SC-005.

### UC-012 — Pay and process simulated payment outcomes

- Primary actor: Buyer; supporting actor: simulated Payment Provider.
- Scope: SC-006, SC-026.
- Goal: create a payment request and reconcile success, failure, or expiry from verified callbacks without repeated effects.
- Detail: [Draft specification](USE_CASES/UC-012-process-payment.md), Proposed one group request, verified complete paid transition, protected paid holds without deduction, failure/expiry, duplicate/conflicting/late evidence and recovery. OQ-007/OQ-008/OQ-015 remain Open; paid cancellation/refund/exception closure and retry limits remain OQ-005/OQ-009/OQ-010/OQ-012.

### UC-013 — Confirm and fulfill a Shop order

- Primary actor: Seller / Shop Operator.
- Scope: SC-013, SC-012.
- Goal: confirm or permissibly reject an order, pack it, and hand it over for delivery.
- Detail: [Draft specification](USE_CASES/UC-013-fulfill-shop-order.md), 22 Proposed criteria for per-Shop paid confirmation/deduction, Seller rejection/response timeout, packing, handover intent, financial exceptions and coherent compensation obligations. Payment success alone is not confirmation. Duration, closure/compensation and permission/SKU policy remain OQ-002/004/005/006/007/008/009/010/011/012/015; execution of refunds/restock is not specified.

### UC-014 — Create and track a shipment

- Primary actor: authorized Shop Operator; supporting actor: simulated Logistics Partner.
- Scope: SC-024, SC-025.
- Goal: request a shipment and apply tracking events safely to shipment/order progress.
- Detail: [Draft specification](USE_CASES/UC-014-simulate-shipment.md), 24 Proposed criteria for one whole Shipment, stable creation/recovery, authenticated sequence/graph, gaps/duplicates/conflicts, physical delivery/return exception and synchronized Order progress. OQ-010/OQ-015 remain Open; corrections/query/permissions/retry and completion/refund/receipt remain pending.

### UC-015 — Track, cancel, and complete a purchase

- Primary actor: Buyer.
- Scope: SC-007.
- Goal: observe lifecycle outcomes, perform allowed cancellation, and confirm receipt or trigger auto-completion.
- Detail: [Draft specification](USE_CASES/UC-015-track-cancel-orders.md), 26 Proposed criteria for owned tracking, whole unpaid-group versus paid-Shop cancellation, deadline/race/recovery, sibling isolation, REQUIRED compensation, explicit Buyer completion, and 7-day auto-completion timeout under BR-AFTERSALES-001.

### UC-016 — Request a return, refund, or dispute

- Primary actor: Buyer.
- Scope: SC-008, SC-007, SC-020.
- Goal: submit an after-sales dispute request (Return & Refund or Refund Only) and supporting evidence for a delivered or delivery-exception order.
- Detail: [Draft specification](USE_CASES/UC-016-request-return-refund.md), 16 Proposed criteria for eligibility windows, reason codes, evidence requirements, amount caps (snapshotted Shop payable), auto-completion suspension, 48-hour Seller response deadline, and Buyer withdrawal under BR-AFTERSALES-001.

### UC-017 — Resolve a refund/dispute and simulate refund execution

- Primary actor: Support; supporting actors: Seller / Shop Operator, Buyer, simulated Payment Provider.
- Scope: SC-020, SC-023, SC-026, SC-007.
- Goal: adjudicate after-sales disputes, verify physical return receipt before restocking, execute simulated refunds for disputes and pre-confirmation cancellations, and resolve payment reconciliation exceptions.
- Detail: [Draft specification](USE_CASES/UC-017-resolve-refund-dispute.md), 22 Proposed criteria for pre-confirmation compensation execution, Seller acceptance/timeout auto-approval, physical return inspection/restock under BR-STOCK-RECEIPT-001, Support adjudication (Full/Partial/Reject), cumulative paid funds caps and idempotency under BR-REFUND-001. No real-money movement is introduced.

## P1 inventory

### UC-018 — Submit product and Shop reviews

- Primary actor: Buyer.
- Scope: SC-008; supplies review data to SC-002/SC-003.
- Goal: submit eligible feedback on a purchase and Shop.
- Detail: Not yet specified; eligibility, edits, and rating calculation depend on OQ-013.

### UC-019 — Manage Shop vouchers and promotions

- Primary actor: Seller / Shop Operator.
- Scope: SC-014; supports SC-003/SC-005.
- Goal: define eligibility, quota, validity, and permitted promotional effects.
- Detail: Not yet specified; OQ-011 governs application during UC-011.

### UC-020 — Manage platform vouchers and promotions

- Primary actor: authorized Internal Staff.
- Scope: SC-021, SC-023; supports SC-003/SC-005.
- Goal: define platform promotional programs and their interaction with Shop offers.
- Detail: Not yet specified; OQ-011 governs funding, stacking, quota, and refund effects.

### UC-021 — Handle User and Shop violations

- Primary actor: authorized Internal Staff.
- Scope: SC-018, SC-016, SC-023.
- Goal: apply permitted warnings, restrictions, suspensions, or locks according to policy.
- Detail: Not yet specified; authority, recovery, and affected purchases depend on OQ-004/OQ-005.

### UC-022 — Perform an authorized order intervention

- Primary actor: authorized Internal Staff.
- Scope: SC-019, SC-023.
- Goal: find an order and perform a permitted exceptional action with an audit record.
- Detail: Not yet specified; authority and state guards depend on OQ-005/OQ-009/OQ-010.

### UC-023 — Inspect Shop performance and inventory history

- Primary actor: Seller / Shop Operator.
- Scope: SC-015.
- Goal: view revenue, orders, top Products, and stock change history.
- Detail: Not yet specified; revenue recognition and refund treatment depend on OQ-010. Reporting calculations need explicit definitions before acceptance criteria.

## Cross-cutting P0 requirement

### UC-024 — Record auditable business actions

- Initiating actors: Buyer, Seller, and Internal Staff through their permitted use cases.
- Scope: SC-022; supports SC-023.
- Goal: preserve actor, action, changed data, and timestamp for auditable actions.
- Detail: [Draft specification](USE_CASES/UC-024-record-audit.md), 12 Proposed criteria for the audit inventory, atomic records, immutability, redaction and authorized inspection (BR-AUDIT-001, [NFR-004](NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004)). Audit coverage is still attached to each detailed use case; this entry coordinates a cross-cutting capability, not a separate user-interface flow.

## Specification order

Work by connected business journey across Seller, Buyer, Internal Staff, and partners. Refine prerequisites alongside the journey that needs them; open decisions remain explicit and block acceptance where relevant.

1. Product publication and shopping: UC-004/005/006 → UC-008/009/010 have Draft specifications. Refine UC-003 category validation, UC-002 Shop eligibility and permission dependencies alongside their affected journeys.
2. Purchase through delivery: UC-007/011/012 now have connected Draft stock/checkout/payment specifications. UC-013/014/015 now add Draft Seller confirmation/packing, simulated delivery and Buyer tracking/cancellation. Next review Proposed OQ-007/OQ-008/OQ-009/OQ-010/OQ-015 and connect UC-016/017 completion/after-sales/financial disposition. Keep order, payment, shipment and reservation lifecycles distinct; settle paid-hold timeout/rejection consequences before committing fulfillment behavior.
3. After-sales resolution: UC-016/017 alongside the relevant UC-015/014/012 outcomes. Specify Buyer requests, Seller evidence/returns, Support decisions, and simulated refunds together.
4. UC-001/002/003 and UC-024 now have Draft account/Shop/category, authorization and audit specifications (55 criteria). Owner review of OQ-020/OQ-021/OQ-005/OQ-004 is needed before the MVP gate.
5. P1 capabilities extend the relevant journeys. Reviews affect discovery/detail; vouchers affect checkout; reporting uses settled order/refund rules. They remain in scope without requiring completion of every Seller feature before Buyer specifications begin.

No P0 use case is accepted yet. Draft coverage and the Phase 2 exit-gate checklist are in [TRACEABILITY.md](TRACEABILITY.md).
