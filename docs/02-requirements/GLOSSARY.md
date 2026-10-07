# Glossary

- Status: Draft; term-level Baseline/Draft/Proposed labels identify the source and unresolved policy.
- Owner: Project owner.
- Source: [Functional Scope](../01-product/FUNCTIONAL_SCOPE.md) and [actors](../01-product/STAKEHOLDERS_AND_ACTORS.md).

## Purpose and status

This glossary establishes shared vocabulary for Phase 2 requirements. It clarifies the language in [Functional Scope](../01-product/FUNCTIONAL_SCOPE.md) and [Stakeholders and Actors](../01-product/STAKEHOLDERS_AND_ACTORS.md); it does not approve new product policies or prescribe an implementation.

- **Baseline:** the meaning follows the existing scope or recorded decision. Existing documents do not provide independent evidence of owner sign-off.
- **Draft:** a working definition for writing consistent requirements. Any policy or relationship it implies still requires review.
- **Proposed:** a candidate concept or behavior for owner decision. It is not part of the agreed scope by virtue of appearing here.
- **Accepted:** reserved for decisions explicitly confirmed by the Project Owner; no Draft or Proposed definition is silently promoted to Accepted.

## Vocabulary conventions

- Use **User account** for the account, **Buyer** and **Seller** for the roles acting through it, and **Shop** for the business entity. Avoid `User / Buyer` or `Seller / Shop` when a requirement needs to distinguish them.
- Use **Internal Staff** for platform personnel. Use **Shop staff** for people operating a Shop; these terms do not imply the same permissions.
- Use **Product**, **Variant**, and **SKU** separately. Requirements that change price, inventory, or operational activation should identify the affected SKU.
- Use **Shop order** where the per-Shop boundary matters. Unqualified **Order** means a Shop order in Phase 2 documents unless a document explicitly introduces a different concept.
- Use **Payment Provider** and **Logistics Partner** for the simulated external actors. Use **Partner callback** for the inbound notification and **Business event** for the fact it reports; they are distinct concepts.
- State labels such as `ACTIVE` and `REJECTED` retain their recorded spelling. A descriptive phrase in this glossary is not a new state label.
- Use stable requirement references `UC-###`, `BR-{DOMAIN}-###`, and `NFR-###`. Scope capabilities use `SC-###`, and open decisions use `OQ-###`. A glossary term does not replace an observable requirement or acceptance criterion.
- Keep open questions explicit. A Draft definition is vocabulary for discussion, not acceptance of a policy.

## Participants and access

### User account — Baseline

A marketplace account used to access the product. One User account can act as a Buyer and can also own or operate one or more Shops. Account verification is in scope; its method and effect on allowed actions remain unspecified.

### Buyer — Baseline

The role of a User account when discovering products, managing a Cart, checking out, paying, tracking purchases, reviewing products or Shops, or requesting a refund or dispute. Buyer is not a separate account type.

### Seller / Shop Operator — Baseline

The role of a User account permitted to own or operate a Shop and perform the Shop activities allowed by its permissions. A Seller is an actor; a Shop is the entity on whose behalf the actor operates.

### Shop — Baseline

A marketplace entity whose authorized operators manage its products, SKUs, inventory, fulfillment, vouchers, and promotions. Checkout separates selected items into Orders per Shop. One User account may own or operate more than one Shop.

### Shop staff — Baseline

People authorized to operate a Shop with distinct permissions. The account representation, membership rules, and exact permission set require specification.

### Internal Staff — Baseline

Platform personnel who perform administration, moderation, support, and operations according to granular permissions. Named permission areas are Admin, Moderator, Support, and Operations. The names do not establish a role hierarchy or imply that every Internal Staff member can perform every action.

### Permission — Draft

Authorization to perform a specified action within an applicable scope, such as a particular Shop or platform function. Permissions should be distinguished from business roles. The permission matrix, assignment rules, and exceptional access remain open.

### Project Owner — Baseline

The product stakeholder who defines learning objectives, validates scope, and approves product decisions, including unresolved policies in Phase 2.

## Catalog and inventory

### Category — Baseline

A classification in the marketplace's hierarchical product catalog. A Category can define the attributes required for products classified under it.

### Category-specific attribute — Baseline

A product characteristic whose meaning and validation depend on its Category. Category attribute schemas are managed by Internal Staff. Allowed types, values, and mandatory fields require detailed requirements.

### Product — Draft

A Shop's listing describing an item offered through the marketplace, including category, content, media, and sellable choices. Product content is moderated, while price and inventory are managed at the SKU level. Accepted [BR-PROD-002](BUSINESS_RULES.md#br-prod-002) / [OQ-017](OPEN_DECISIONS.md#oq-017) hides a previously active Product from sale immediately when an actual high-risk content change is successfully saved. The restriction persists before submission, through automatic-validation failure, during review, and after rejection until revised content receives Moderator approval. The old approved content is not automatically restored. Opening the editor, a failed save, and operational-only edits do not trigger this restriction. The exact Product–Variant–SKU relationships remain open.

### Public listing eligibility — Proposed

Whether a Product may appear as a public sale listing. Proposed [BR-PROD-004](BUSINESS_RULES.md#br-prod-004) requires approved high-risk content, applicable Shop eligibility, and at least one enabled valid SKU, independently of positive available stock. Accepted BR-PROD-002 review-related hiding always blocks this eligibility. This is an evaluation condition, not a new Product state code.

### SKU purchase eligibility — Proposed

Whether the exact selected SKU and requested quantity may be accepted for a new purchase. It depends on Product listing eligibility, permitted current SKU activation/price, and sufficient available quantity. A visible out-of-stock listing is not a purchasable selection; discovery/detail/cart checks do not reserve stock. Final stock and checkout semantics remain OQ-007/OQ-008, and the public-display proposal remains [OQ-019](OPEN_DECISIONS.md#oq-019).

### Variant — Draft

A distinguishable sellable choice of a Product, typically described by an attribute combination such as color and size. This term identifies the business choice, without deciding how many SKUs may represent it or how variant changes affect existing orders.

### SKU (Stock Keeping Unit) — Baseline

The unit at which a Seller manages price and inventory for a Product. SKU activation, deactivation, and the Seller's internal SKU code are operational changes. The internal SKU code is not assumed to be the system identifier, nor is its uniqueness scope defined yet.

### Physical stock — Draft

The quantity recorded as on hand for a SKU. It is affected by stock-in, adjustments, and the stock deduction required when an Order is confirmed. It does not imply a warehouse model or real-world inventory integration.

### Reserved stock — Draft

The quantity held for checkout items so that the same quantity is unavailable for another purchase. Reservation at checkout and release on payment failure or expiry are in scope. Proposed [BR-INV-001](BUSINESS_RULES.md#br-inv-001) counts unpaid and paid active holds until an explicit release/consume transition; overdue unpaid holds still count until recorded expiry. Ownership, unpaid duration, paid-hold limit and conversion guards remain [OQ-008](OPEN_DECISIONS.md#oq-008).

### Available stock — Draft

The quantity a SKU can currently offer for a new purchase after applicable holds. Proposed [BR-INV-001](BUSINESS_RULES.md#br-inv-001) calculates `on_hand - reserved`, including every effective unpaid/paid hold; other Shop/Product/SKU restrictions are separate eligibility guards, not invented quantity deductions. Formula/units/timing require owner confirmation under [OQ-008](OPEN_DECISIONS.md#oq-008). Available stock is distinct from Product visibility and SKU activation.

### Stock reservation / Paid hold — Proposed

An identified hold for an exact Shop/SKU quantity belonging to one Shop order and purchase group. [SM-RESERVATION-001](STATE_MACHINES.md#sm-reservation-001) distinguishes unpaid `ACTIVE_PAYMENT`, protected paid `ACTIVE_PAID`, release, expiry and consumption. Timely payment changes protection without deduction; Seller confirmation consumes the paid hold and deducts physical stock. The unpaid deadline cannot expire an already-paid hold. Paid timeout/cancellation remain OQ-008/OQ-009, rather than an accepted unlimited hold.

### Stock-in — Baseline

A Seller action that records an increase in a SKU's inventory. The source, supporting evidence, and allowed correction behavior require detailed requirements.

### Inventory adjustment — Draft

A correction to a SKU's recorded stock outside the ordinary purchase flow. Inventory change history is in scope; allowed reasons, authorization, and interaction with reservations remain open.

## Purchase and fulfillment

### Cart — Baseline

A Buyer's collection of selected items before checkout. A Cart can contain items from multiple Shops. Cart membership alone does not establish an Order or a confirmed stock reservation.

### Checkout — Baseline

The purchase process that validates selected items and inventory, calculates shipping fees, applies vouchers, and splits items into Orders per Shop. [UC-011](USE_CASES/UC-011-checkout.md) proposes all-or-nothing acceptance of the explicitly selected set, with quote revalidation and stock holds; those choices remain OQ-007/OQ-008. Concrete shipping/voucher policy remains incomplete.

### Purchase group — Proposed

The grouping of all selected Shop orders accepted by one Buyer checkout operation, with a common purchase amount/currency, unpaid deadline, and one logical Payment Request under Proposed [OQ-007](OPEN_DECISIONS.md#oq-007). It is a business correlation concept, not a committed aggregate/database design. Shop fulfillment remains per Shop order; access to one Shop order does not grant access to the entire private group.

### Shop order / Order — Baseline

The purchase record produced by checkout for selected items belonging to one Shop. It has its own business lifecycle and is associated with payment and fulfillment activity. A purchase selection containing multiple Shops produces separate Shop orders. The proposed purchase group links them without replacing their per-Shop fulfillment identity.

### Order confirmation — Baseline action; Draft boundary

The Seller's acceptance of an Order for fulfillment. The functional scope requires stock deduction when an Order is confirmed. Proposed [OQ-008](OPEN_DECISIONS.md#oq-008) permits confirmation only after verified payment; it consumes all that Shop order's paid holds and deducts stock once. Payment success alone does not mean Seller confirmation. These prerequisite/consumption guards remain unconfirmed; [UC-013](USE_CASES/UC-013-fulfill-shop-order.md) now provides the Draft confirmation/packing/handover and closure flows.

### Order line — Draft

A purchase entry identifying an ordered SKU and quantity within a Shop order. This is business vocabulary only; it does not prescribe a database structure.

### Seller-response deadline — Proposed

The per-Shop response boundary recorded from the authoritative time group payment success was applied. [OQ-009](OPEN_DECISIONS.md#oq-009) proposes 24 hours, strict-before confirmation/rejection/paid cancellation, and timeout eligibility at/after the boundary; duration and policy await confirmation. It is independent of the original unpaid deadline and cannot free paid holds without the complete owning closure/compensation outcome.

### Compensation obligation — Proposed

The stable required monetary consequence of an eligible paid pre-confirmation Shop closure under [BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001). It refers to the original paid request and snapshotted Shop payable/currency, including attributed shipping/discounts. REQUIRED means awaiting authorized disposition/execution; it is not a completed Refund, new charge or group payment rollback. Actual execution/closure/caps/recovery remain UC-017/OQ-010/OQ-015.

### Delivered versus completed — Proposed distinction

Delivered records admissible Logistics Partner evidence synchronized to the Shop order. Commercial completion and its Buyer/system action, timing and after-sales/review consequences remain OQ-010; no ordinary tracking event establishes them. Failed/returned logistics also does not alone prove approved after-sales Return, goods receipt or Refund.

### Payment — Draft

The business process and recorded outcome of paying for a purchase through the simulated Payment Provider. Payment success, failure, and expiration are in scope. Proposed [BR-PAY-001](BUSINESS_RULES.md#br-pay-001) separates provider financial evidence from successful payment application to Orders/holds, and from reconciliation status. A provider-reported success can therefore require reconciliation while an expired purchase stays closed. Payment remains distinct from Seller confirmation, Shipment and Refund.

### Payment request — Baseline

A request to the simulated Payment Provider to initiate payment. A provider result may arrive asynchronously. Proposed OQ-007 assigns one stable logical request to one purchase group for its snapshotted amount/currency; recovering an unknown creation/result reuses that identity. Grouping/identity remain unconfirmed, and transport details are deferred to design.

### Shipment — Draft

A delivery arrangement associated with Order fulfillment and tracked through the simulated Logistics Partner. Shipment state is distinct from Order and Payment state. Proposed [OQ-010](OPEN_DECISIONS.md#oq-010)/[BR-SHIP-001](BUSINESS_RULES.md#br-ship-001) uses one whole Shipment per paid confirmed/packed Shop order, without splits/replacement. This remains unconfirmed; partial fulfillment and corrections are unresolved.

### Tracking event — Baseline

A Logistics Partner notification reporting delivery progress, such as awaiting pickup, picked up, in transit, delivered, failed delivery, or returned. Duplicate, late, and out-of-order tracking events must be handled. These examples do not themselves establish approved transitions. Proposed [SM-SHIPMENT-001](STATE_MACHINES.md#sm-shipment-001) distinguishes authenticated contiguous progress, deferred gaps, nonterminal failed attempts, terminal delivery/return and separate Order effects.

## Moderation and after-sales service

### Moderation — Baseline

The baseline review and intervention process for Shops and Products combines automatic validation with manual Product review as recorded in [Business Rules](BUSINESS_RULES.md). For first publication, a Moderator approval makes a Product `ACTIVE`; rejection makes it `REJECTED` with a reason. Accepted [BR-PROD-002](BUSINESS_RULES.md#br-prod-002) / [OQ-017](OPEN_DECISIONS.md#oq-017) additionally hides a previously active Product from the successful save of an actual high-risk content change, before submission and through automatic-validation failure and pending re-review. Accepted [OQ-016](OPEN_DECISIONS.md#oq-016) keeps it hidden after rejection until revised content receives Moderator approval, without automatically restoring the old approved content. Admin manages policies and exceptional interventions rather than serving as the default reviewer.

### Automatic validation — Baseline

System checks of deterministic submission rules before manual Product review, including required data, price, non-negative inventory, category structure, supported image format, and basic forbidden-word checks. Exact values and thresholds still require specification.

### Re-review — Baseline

Baseline manual review required after changes to high-risk content of an active Product: category, name, description, media, category-specific attributes, or variants. Operational SKU price, inventory, activation, and internal-code changes retain their manual-review exemption. Accepted [BR-PROD-002](BUSINESS_RULES.md#br-prod-002) defines visibility from successful risky saving until manual approval. Accepted [BR-PROD-003](BUSINESS_RULES.md#br-prod-003) permits withdrawal before further risky edits. For a pending submission, the first accepted withdrawal/approval/rejection wins; later conflicts are refused and repeated identical actions have no additional effect. Revised content after withdrawal creates a new eligible submission.

### Return — Draft

The process in which purchased goods move back after fulfillment or a delivery attempt. A Return is distinct from the monetary Refund and the adjudication of a Dispute. Buyer-initiated return eligibility, physical-return requirements, and the relation to a Logistics Partner's returned tracking status remain open.

### Refund — Draft

A reversal of all or part of a recorded purchase payment, simulated within the demo. Refund requests, decisions, execution, and closure are in scope. Eligibility, partial refunds, approval authority, and the relation to a Return remain open.

### Dispute — Draft

A contested transaction or after-sales issue requiring evidence and a decision. Internal Staff handles dispute intake, evidence collection, decision, and closure within its permissions. A Dispute does not automatically imply a Return or Refund; the allowed outcomes remain open.

### Ticket — Draft

A case used to track refund or dispute intake, evidence, decisions, execution where applicable, and closure. Whether refund and dispute requests share a single case type is undecided.

## Discounts

### Voucher — Baseline capability; Draft definition

A benefit offered by a Shop or the platform that can be applied to an eligible purchase under rules, quota, and a validity window. Eligibility, stacking, usage counting, cancellation restoration, and allocation across Shop orders require specification.

### Promotion — Draft

A Shop or platform program that changes the benefit or terms of an eligible offer or purchase. It is distinct from a Voucher, although a promotion may use vouchers. Automatic application, overlapping promotions, and calculation precedence remain open.

## Partner interactions and consistency

### Simulated partner — Baseline

An external actor represented by demo behavior rather than a real commercial integration. Payment Provider and Logistics Partner are simulated partners. Their business outcomes and asynchronous interactions must still be specified and tested.

### Partner callback / Webhook — Baseline

An asynchronous notification from a simulated partner reporting a result or progress. `Callback` is the general business term; `webhook` names the notification mechanism already present in scope. Payment callbacks require signature verification, retries, idempotency, and reconciliation; Logistics tracking must tolerate duplicate, late, and out-of-order notifications. Detailed policies remain open under [OQ-015](OPEN_DECISIONS.md#oq-015).

### Business event — Draft

A reported fact about a business occurrence, such as a payment result or shipment progress. An event is distinct from the delivery attempt carrying it; multiple callback attempts can report the same occurrence. Event names, identifiers, and ownership belong to later analysis after requirements are agreed.

### Idempotency — Baseline capability; Draft definition

The property that repeated handling of the same logical operation or partner result does not repeat its business effects. For example, a repeated payment-success callback must not apply successful payment processing twice. Logical-operation identity, scope, retention, and treatment of conflicting results remain open.

### Event correlation — Draft

The association of a partner notification with the Payment, Shipment, and Order it concerns, and with prior notifications where needed. Correlation determines what an event refers to; idempotency determines whether its effects have already been applied. Identifier formats and transport fields are deferred to design.

### Reconciliation — Baseline capability; Draft definition

The process of detecting and resolving disagreement between purchase/payment application and simulated provider evidence. Proposed [OQ-015](OPEN_DECISIONS.md#oq-015) preserves established effects and correlates request/event, Orders/holds and audit history; late success/conflict needs an explicit disposition rather than automatic resurrection/refund. Authority, trigger, retry/retention and resolution actions remain open.

### Snapshot — Proposed

A record of accepted business facts so later changes can be distinguished from the agreed purchase. Proposed [BR-ORDER-001](BUSINESS_RULES.md#br-order-001) captures approved descriptions/exact SKU identity, quantities/prices/totals, shipping/discount sources, currency, address, shipping choice and deadline with checkout acceptance/holds. Later edits cannot silently rewrite it. Contents, capture/ordering and correction policy remain OQ-007.

### Audit log — Baseline

A record of actor, action, changed data, and timestamp for auditable activity. Required coverage, access, retention, and sensitive-data treatment remain open.

## Open terminology decisions

The [Open Decisions register](OPEN_DECISIONS.md) is the canonical register for unresolved product policy. The questions below identify terminology implications and must be resolved in the relevant use cases or business rules:

1. Do Shop staff and Internal Staff use the same account concept as marketplace Users, and how are their roles and permission scopes assigned? See `OQ-005` for Shop approval and permissions.
2. What is the exact relationship between Product, Variant, and SKU, including whether a Product without selectable variants has a default SKU? See `OQ-006`.
3. Which stock quantities are recorded, how is available stock calculated, and what constitutes the reservation owner, expiry, and conversion to stock deduction? See `OQ-008`.
4. Is there a named checkout grouping concept, and how are Payment requests associated with multiple Shop orders? See `OQ-007`.
5. What conditions permit Order confirmation, Shipment creation, and multiple or partial Shipments? See `OQ-008` and `OQ-010`.
6. How do Return, Refund, Dispute, and Ticket relate, and does a returned logistics status mean delivery failure, an approved after-sales Return, or either? See `OQ-010`.
7. Which Voucher and Promotion behaviors apply across Shops, and how are overlap and allocation described? See `OQ-011`.
8. What facts are preserved in a purchase Snapshot, and when are they captured? See [OQ-007](OPEN_DECISIONS.md#oq-007).
9. What identifies a logical partner occurrence, and how are duplicate, conflicting, late, or out-of-order notifications distinguished? See [OQ-015](OPEN_DECISIONS.md#oq-015).
