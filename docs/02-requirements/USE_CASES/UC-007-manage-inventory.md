# UC-007 — Adjust and protect SKU inventory

## Metadata

- Status: Draft; quantity accounting, adjustment bounds, reservation states, concurrency, and repeated-operation behavior below are Proposed under OQ-008. Payment grouping and partner-result handling remain Proposed under OQ-007/OQ-015. No new product policy is Accepted by this specification.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope: SC-012; supporting dependencies SC-005/SC-006/SC-013/SC-022/SC-026 in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).
- Source: Baseline SC-012 reserves at checkout, releases on payment failure/expiry, and deducts when Seller confirms the Shop order. This Draft connects those actions with the candidate decisions in [OQ-007](../OPEN_DECISIONS.md#oq-007), [OQ-008](../OPEN_DECISIONS.md#oq-008), and [OQ-015](../OPEN_DECISIONS.md#oq-015); authorization and valid quantities remain OQ-002/OQ-005/OQ-006.

## Goal

Let an authorized Seller record stock-in or a deliberate correction for an exact Shop/SKU while protecting existing purchase reservations and preventing overselling. Connect inventory effects to checkout, simulated payment, and Seller order confirmation without treating them as the same action.

## Primary actor

Seller / Shop Operator with inventory-maintenance permission for the target Shop.

## Supporting actors

- Buyer initiates a purchase through [UC-011](UC-011-checkout.md).
- The system applies eligible reservation/payment outcomes through [UC-012](UC-012-process-payment.md), including verified simulated Payment Provider results and unpaid expiry.
- An authorized Seller confirms an eligible paid Shop order through [UC-013](UC-013-fulfill-shop-order.md); paid closure/timeout and Buyer cancellation follow its Proposed contract and [UC-015](UC-015-track-cancel-orders.md).
- Internal Staff access and intervention are separate permission-dependent capabilities; being Admin, Moderator, Support, or Operations does not automatically authorize an inventory adjustment.

## Trigger

Seller requests stock-in or an inventory adjustment; alternatively, the purchase lifecycle requests an authorized reservation, payment-associated hold change, release, or consumption for identified purchase items.

## Preconditions

- The exact Shop and SKU are identifiable. The SKU belongs to the target Shop; an internal SKU code alone is not assumed to establish immutable identity or uniqueness. Identity/variant changes and inventory units remain OQ-002/OQ-006.
- A Seller operation checks the acting account's inventory permission and Shop scope before exposing protected inventory data or changing it. Account/Shop restrictions and the exact permission matrix remain OQ-004/OQ-005.
- A lifecycle operation identifies its originating purchase group, Shop order, exact SKU, quantity, and existing reservation where applicable. Sellers do not create a payment outcome by supplying a callback or directly altering reservation records.
- Supplied quantities satisfy the applicable unit, precision, and range rules. This Draft does not settle whether all quantities must be whole units or prescribe numeric limits.
- Proposed accounting uses `on_hand` for recorded physical stock, `reserved` for the sum of all active reservation quantities for the SKU, and `available = on_hand - reserved`. Both `ACTIVE_PAYMENT` and `ACTIVE_PAID` holds count toward `reserved`; all three quantities remain non-negative. The canonical candidate contract is [BR-INV-001](../BUSINESS_RULES.md#br-inv-001).

## Main flow — Seller stock-in or adjustment

1. Seller identifies the Shop and exact SKU, inspects the permitted current `on_hand`, `reserved`, and `available` quantities, and chooses stock-in or adjustment.
2. For stock-in, Seller supplies the positive increment; for an adjustment, Seller supplies the intended new `on_hand` total. The operation describes the correction and its business context for audit. Mandatory reason categories or supporting evidence are not defined here.
3. The system verifies Shop ownership/permission, resolves the operation identity, and validates supplied quantities. An unchanged total is a no-op rather than an additional stock movement. The exact operation-identity representation remains a later design choice.
4. The system checks the current effective stock and active holds at the moment the change is accepted. A resulting `on_hand` below `reserved` is refused. Seller cannot release, reduce, transfer, or consume a reservation to make a correction pass.
5. The system applies the one permitted stock movement and reports the resulting `on_hand`, `reserved`, and `available`. An adjustment changes only `on_hand`; stock-in increases it. Other accepted changes that occurred concurrently are preserved or the stale attempt is refused for a fresh review, rather than silently overwritten.
6. The system records the actor, action, changed data, timestamp, and correlation to the exact Shop/SKU and logical operation. The Seller can inspect the permitted result; broader inventory-history/reporting behavior belongs to UC-023.

## Supporting flow — Purchase reservation and consumption

1. UC-011 revalidates the selected purchase's Product/SKU eligibility, quantities, and current stock. Cart membership, discovery, or an earlier availability check has not reserved stock.
2. On acceptance of the Proposed checkout group, the system reserves each exact SKU's aggregate selected quantity and associates each hold with its Shop order and purchase group. The all-or-nothing group boundary is defined in [UC-011](UC-011-checkout.md) and OQ-007. `on_hand` does not decrease; `reserved` increases and `available` decreases by the held quantity.
3. Each hold starts in Proposed `ACTIVE_PAYMENT`, with an authoritative unpaid deadline defined by the purchase/payment contract. Deadline duration remains OQ-008. Crossing that deadline makes the hold ineligible for successful payment processing; a still-active overdue hold remains counted until its one recorded expiry/release transition occurs.
4. If verified simulated payment success is accepted before the authoritative deadline while the purchase's required holds are eligible, UC-012 changes them to Proposed `ACTIVE_PAID`. The Shop orders await Seller confirmation. `on_hand`, `reserved`, and `available` do not change merely because payment succeeded. The original unpaid expiry no longer releases these paid holds.
5. If payment failure or unpaid expiry is accepted while the unpaid purchase remains eligible for that terminal outcome, release its still-active unpaid holds once through UC-012. `reserved` decreases and `available` increases; `on_hand` remains unchanged. The whole purchase-group boundary and conflicting-outcome rules are Proposed under OQ-007/OQ-015.
6. When an authorized Seller confirms an eligible paid Shop order through UC-013, consume its `ACTIVE_PAID` holds and deduct their quantities from `on_hand` in the same accepted business transition. `reserved` decreases by the same quantities; `available` is unchanged by consuming that order's holds. Every required line for that Shop order succeeds together or none is confirmed/consumed/deducted.
7. Proposed [BR-CANCEL-001](../BUSINESS_RULES.md#br-cancel-001) adds whole unpaid-group cancellation and paid pre-confirmation Shop cancellation/rejection/timeout. The owning UC-013/015 outcome releases all its active holds once; paid closure records the complete stable compensation obligation together, leaves on_hand unchanged and preserves sibling paid Orders. Candidate Seller deadline and financial disposition remain unconfirmed OQ-009; consumed-stock restock and refund execution remain OQ-010. Time passage or a refund claim alone cannot release paid/consumed holds.

## Alternative and error flows

- **A1 — Permission denied or wrong Shop:** refuse protected reads/changes without disclosing another Shop's private inventory or changing its stock/holds. A User's Buyer role or permission for another Shop grants no additional inventory rights. Failed-attempt audit coverage remains OQ-005/OQ-012.
- **A2 — Missing, invalid, or changed SKU identity:** report the unresolved selection without moving stock or silently substituting a variant/SKU. SKU removal/reidentification while holds or orders exist is not authorized by this Draft and remains OQ-006.
- **A3 — Invalid quantity or adjustment below reservations:** refuse the entire attempted movement, identify the unmet quantity/invariant rule, and preserve stock and holds. Exact boundary fixtures await OQ-002/OQ-006.
- **A4 — Concurrent checkout contention:** accepting one reservation reduces availability for every competing attempt. The next attempt must use the resulting quantities; no two successful operations may hold the same last available unit.
- **A5 — Adjustment races with reservation/release/consumption:** evaluate both against one consistent sequence of accepted business changes. Refuse or reevaluate a stale correction rather than losing the other change or letting `on_hand < reserved`. A preview or displayed quantity does not authorize an unconditional later overwrite.
- **A6 — Payment success races with failure/unpaid expiry:** only the outcome permitted by UC-012's Proposed transition policy wins. An accepted eligible success preserves paid holds; an accepted failure/expiry releases unpaid holds. A late/conflicting success does not recreate a terminal hold, deduct stock, or confirm a Shop order; it is exposed for reconciliation under OQ-015.
- **A7 — Original unpaid expiry after payment success:** an expiry action targeting the original payment-waiting window cannot release `ACTIVE_PAID` stock. A separate Proposed paid-order timeout/cancellation under UC-013/015 and BR-CANCEL-001 owns any paid release; it is not an unpaid-expiry effect.
- **A8 — Seller confirmation with missing, released, unpaid, or already consumed hold:** refuse a new confirmation/deduction when its guards fail. A repeated already-accepted confirmation returns that existing outcome without another stock effect. This use case does not permit confirmation before verified successful payment under the candidate OQ-008 policy.
- **A9 — Repeated request versus later deliberate movement:** repeating the same logical stock-in/adjustment returns its original outcome with no new quantity or audit effect. A separately identified later deliberate stock-in/adjustment is evaluated afresh even when its entered quantities match an earlier request. Reusing one identity for different instructions is refused; content equality alone must not suppress a legitimate later operation. Identity/retention details are pending design and OQ-008/OQ-012.
- **A10 — Repeated release, paid-hold change, or consumption:** repeats have no additional quantity, reservation, order, or effective audit change. A conflicting terminal action cannot consume a released hold or release a consumed hold. Partner event identity and contradictory outcomes remain OQ-015.
- **A11 — Product hidden or SKU operationally disabled:** permitted inventory maintenance does not approve Product content, reactivate the SKU, or restore purchase eligibility. Accepted BR-PROD-002 still blocks new purchases. Consequences for already-created purchase holds when content/Shop/SKU eligibility later changes remain OQ-004/OQ-007/OQ-008/OQ-019; do not silently cancel them in UC-007.
- **A12 — Cancellation, Seller rejection, return, or refund:** release/restock only through an explicitly allowed owning lifecycle transition. A paid amount being refunded, a shipment being returned, or a Seller declining fulfillment does not independently authorize a stock increment here. Paid pre-confirmation closure is Proposed in BR-CANCEL-001/UC-013/015; after-sales inventory receipt remains unspecified OQ-010.

## Business rules

- Proposed [BR-INV-001](../BUSINESS_RULES.md#br-inv-001): quantity invariants, protected holds, reservation/release/consumption accounting, and repeated/concurrent stock effects.
- Proposed [BR-CANCEL-001](../BUSINESS_RULES.md#br-cancel-001)/[BR-ORDER-002](../BUSINESS_RULES.md#br-order-002): owning cancellation/rejection/timeout release plus complete paid compensation, and exact Seller confirmation cutoff/consumption; criteria reside in UC-013/015.
- Baseline [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001): valid inventory maintenance is an operational change exempt from manual Product re-review.
- Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002): an inventory change cannot override review-related hiding or allow a new purchase of hidden content.
- Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004): common Product/SKU purchase eligibility must also pass at reservation acceptance; positive available stock alone does not establish purchase eligibility.
- [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-003](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-003), and [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004): Shop isolation, contention/replay consistency, and attributable inventory changes. Numeric verification thresholds and failed-attempt coverage remain Proposed/Open.

## State/data changes

- Quantity changes follow Proposed [SM-INVENTORY-001](../STATE_MACHINES.md#sm-inventory-001); reservation eligibility/terminal transitions follow Proposed [SM-RESERVATION-001](../STATE_MACHINES.md#sm-reservation-001). Numeric availability is not a new Product publication state.
- Seller stock-in/adjustment changes `on_hand`, preserving every existing hold. No Product content or approval state changes.
- Checkout creates `ACTIVE_PAYMENT` holds; eligible verified success changes them to `ACTIVE_PAID` without changing quantity totals. Failure/unpaid expiry releases eligible unpaid holds; Seller confirmation consumes paid holds with the associated deduction.
- Each reservation retains its exact Shop/SKU and purchase/order association, original quantity, relevant deadline, and outcome history so another purchase or SKU cannot inherit its entitlement. Representation and storage mechanism are deferred to design.
- Order/payment states and guards are owned by UC-011/012 and their linked models; Seller fulfillment/paid closure continues in Draft UC-013/015; no readiness or policy acceptance is inferred. No payment, order, shipment, and inventory states are merged into one lifecycle.

## Postconditions

- Successful Seller operation: the intended permitted movement is applied once, reported, and attributable; all stock invariants and existing holds remain valid.
- Successful lifecycle operation: its associated reservation/quantity/order effects occur together once according to the owning transition; unrelated Shop/SKU/order data remain unchanged.
- Failure: the refused operation applies no partial movement, reservation edit, purchase confirmation, or content/publication change. An existing valid reservation is not lost merely because a later attempt was denied.
- Paid holds awaiting Seller confirmation remain protected; their maximum lifetime and allowed cancellation/rejection handling are explicit pending decisions, not a passed readiness condition.

## Acceptance criteria

All newly specified inventory policies below are **Proposed** unless a criterion explicitly names a preserved Baseline or Accepted boundary. These are specification criteria; executable verification is pending implementation.

- **AC-UC-007-01 — Stock-in:** given an authorized Shop Operator and a valid positive stock-in increment, when the operation is accepted, `on_hand` and `available` increase by that increment, `reserved` and all existing holds are unchanged, and the result identifies the exact Shop/SKU.
- **AC-UC-007-02 — Protect active holds:** given active unpaid and paid reservations totaling `reserved`, when a Seller adjusts `on_hand`, a proposed total equal to or greater than `reserved` may be accepted if otherwise valid; a total below `reserved` is refused without changing quantities or holds. Paid holds cannot be deleted to permit the lower total.
- **AC-UC-007-03 — Invalid movement:** given an invalid quantity/unit/range or missing/unresolved SKU, when a movement is attempted, the unmet rule/selection is reported and no stock or reservation change is applied. Concrete numeric limits await OQ-002/OQ-006.
- **AC-UC-007-04 — Shop isolation:** given an actor with rights only for Shop A and a SKU in Shop B, when that actor attempts to inspect protected inventory, stock-in, adjust, or directly alter a hold for Shop B, private inventory is not disclosed and Shop B's stock/holds remain unchanged. Apply the same denial to an unauthorised Internal Staff actor; allow cases await OQ-005.
- **AC-UC-007-05 — Operational moderation boundary (Baseline/Accepted):** given permitted inventory maintenance, it does not require Product re-review merely for that stock movement. If the Product is already hidden under BR-PROD-002, a successful increase/adjustment does not unhide it or permit a new purchase.
- **AC-UC-007-06 — Checkout accounting:** given an eligible purchase with a valid requested quantity and sufficient aggregate available stock per exact Shop/SKU, when UC-011 accepts its reservation, `reserved` increases and `available` decreases by the held quantity while `on_hand` remains unchanged. The purchase-group hold set follows UC-011's Proposed all-or-nothing boundary; refusal leaves no partial set.
- **AC-UC-007-07 — Last-unit contention:** given one valid available unit and two simultaneous checkout attempts each requesting it, at most one reservation succeeds. Every resulting `on_hand`, `reserved`, and `available` remains non-negative and `reserved <= on_hand`.
- **AC-UC-007-08 — Adjustment/checkout race:** given two valid available units, a correction from two to one `on_hand`, and a competing reservation for two units, the accepted outcomes reflect one coherent ordering. If the correction succeeds first, the two-unit reservation is refused; if reservation succeeds first, the correction is refused. A stale preview does not permit both to succeed.
- **AC-UC-007-09 — Failure/unpaid expiry release:** given an eligible unpaid purchase with active holds, when payment failure or unpaid expiry is accepted, each affected hold is released once, its quantity is removed from `reserved` and added to `available`, and `on_hand` is unchanged. An overdue still-active hold is not counted as available before its recorded release.
- **AC-UC-007-10 — Paid hold without deduction:** given an unpaid purchase with every required hold eligible and verified payment success accepted before its authoritative deadline, UC-012 moves the holds to `ACTIVE_PAID` and Shop orders await Seller confirmation. `on_hand`, `reserved`, and `available` stay unchanged; no Seller confirmation or stock deduction is fabricated.
- **AC-UC-007-11 — Paid holds survive unpaid expiry:** given an accepted success and `ACTIVE_PAID` holds, when the original unpaid deadline's expiry action runs, it does not release those holds or alter stock totals. The scenario does not establish a paid-hold timeout policy.
- **AC-UC-007-12 — Seller confirmation consumes once:** given an eligible paid Shop order with every required `ACTIVE_PAID` hold, when its authorized Seller confirms through UC-013, all its line holds are consumed and both `on_hand` and `reserved` decrease by the same line quantities in one accepted transition; `available` is unchanged. A missing/ineligible line hold refuses the whole Shop-order transition. Other Shop orders retain their own holds.
- **AC-UC-007-13 — Competing payment outcomes:** given concurrent verified success and failure/unpaid-expiry processing for one eligible unpaid purchase, the UC-012 policy yields either protected paid holds or released unpaid holds, never both effective outcomes. A released hold is not recreated by later success, and no outcome deducts stock before Seller confirmation. Contradictory/late results remain visible for reconciliation.
- **AC-UC-007-14 — Repeat versus deliberate later adjustment:** given one accepted stock-in/adjustment operation, replaying its logical identity, including concurrent delivery, returns the original result without another movement or effective audit entry. Reusing that identity with different instructions is refused. A separately identified later deliberate operation is evaluated afresh, even if its entered quantities equal a prior operation; intervening accepted changes are preserved or reported for fresh review.
- **AC-UC-007-15 — Repeated lifecycle effects:** given an accepted reservation, paid-hold change, release, or consumption, repeated processing has no additional stock/hold/order/effective audit effect. A conflicting request cannot consume a released hold, release a consumed hold, or deduct for an already-confirmed order again. Event/operation identity fixtures remain OQ-008/OQ-015 and later design.
- **AC-UC-007-16 — Attributable inventory changes:** given a successful stock-in, adjustment, reservation, paid-hold change, release, or consumption, its audit evidence identifies actor/system origin, action, changed data, timestamp, exact Shop/SKU, logical operation, and purchase/Shop-order/reservation association where applicable. Quantities before/after and the recorded hold transition explain the change. Failed-attempt logging, access, retention, and redaction remain OQ-005/OQ-012.
- **AC-UC-007-17 — Cart does not hold stock (Baseline boundary):** given Buyer cart addition, update, inspection, or removal under UC-010, the cart operation itself creates no reservation, release, consumption, or `on_hand` deduction. Final reservation eligibility is checked in UC-011.
- **AC-UC-007-18 — After-sales boundary:** given a confirmed order's consumed holds, a later cancellation, rejection, return, shipment update, or simulated refund does not automatically release those terminal holds or increment inventory through this use case. Restocking requires the permitted inventory-receipt/correction transition once OQ-009/OQ-010 defines it.

## Traceability

- SC-012 → UC-007 → [BR-INV-001](../BUSINESS_RULES.md#br-inv-001), BR-PROD-001/002/004, [SM-INVENTORY-001](../STATE_MACHINES.md#sm-inventory-001), [SM-RESERVATION-001](../STATE_MACHINES.md#sm-reservation-001), NFR-001/003/004 → AC-UC-007-01 through AC-UC-007-18; canonical coverage is maintained in [TRACEABILITY.md](../TRACEABILITY.md).
- Product/SKU edits: [UC-004](UC-004-create-update-product.md); cart intentions: [UC-010](UC-010-manage-cart.md); purchase-group reservation: [UC-011](UC-011-checkout.md); payment result/expiry: [UC-012](UC-012-process-payment.md). Seller confirmation/fulfillment UC-013, permitted cancellation UC-015, and refund/inventory receipt UC-016/017 remain pending detailed specifications.
- Related acceptance coverage: AC-UC-004-04/07, AC-UC-010-03/09, and UC-011/012's linked purchase/payment criteria. Domain/design and executable verification are pending; this Draft does not pass a product or release gate.

## Open questions

[OQ-008](../OPEN_DECISIONS.md#oq-008) accounting, unpaid deadline, paid-hold limit, Seller-confirmation timing, and logical inventory operations; [OQ-007](../OPEN_DECISIONS.md#oq-007) purchase grouping/atomicity and existing purchases after eligibility changes; [OQ-015](../OPEN_DECISIONS.md#oq-015) verified, repeated, conflicting, late partner outcomes and reconciliation; [OQ-009](../OPEN_DECISIONS.md#oq-009) permitted cancellation/rejection and paid-hold consequences; [OQ-010](../OPEN_DECISIONS.md#oq-010) returns/refunds/inventory receipt; [OQ-002](../OPEN_DECISIONS.md#oq-002) quantity validation/units; [OQ-006](../OPEN_DECISIONS.md#oq-006) immutable SKU identity and outstanding-purchase compatibility; [OQ-004](../OPEN_DECISIONS.md#oq-004) account/Shop/Product restrictions; [OQ-005](../OPEN_DECISIONS.md#oq-005) Shop inventory permission and protected reads; [OQ-012](../OPEN_DECISIONS.md#oq-012) audit coverage and quality measurements; [OQ-019](../OPEN_DECISIONS.md#oq-019) purchase eligibility and checkout-versus-edit ordering. No unresolved policy is promoted to Accepted here.
