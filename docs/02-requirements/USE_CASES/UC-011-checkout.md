# UC-011 — Checkout a multi-shop cart

## Metadata

- Status: Draft; grouping, atomicity, snapshots, purchase ordering, and payment/reservation policy are Proposed under OQ-007/OQ-008. Accepted BR-PROD-002 blocks new purchases of review-hidden Products.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-005/SC-012, supporting SC-004/SC-006/SC-022 in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md); continuation of WI-002, authorized on 2026-10-07.

## Goal

Let Buyer agree to the current purchase facts and accept a selected multi-shop purchase with protected stock, separate Shop orders, and an unambiguous payable amount.

## Primary actor

Buyer acting through the permitted User account. Account verification and guest purchasing remain OQ-005.

## Supporting actors

Seller / Shop Operator maintains offers and stock through UC-004/007; Moderator changes approval through UC-006; simulated Payment Provider participates through UC-012. These actors do not acquire ownership of the Buyer's cart or purchase group.

## Trigger

Buyer selects intended cart entries and requests a checkout quote, then explicitly accepts the presented purchase facts.

## Preconditions

- Buyer may access the source cart, delivery address, and intended purchase. Exact account prerequisites remain OQ-005.
- The selection is nonempty and identifies exact Shop/Product/SKU quantities. Aggregate repeated selections for the same exact Shop/SKU before checking stock; never substitute a different SKU.
- For a new acceptance, current offers satisfy Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) and Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004). Recovery of an already-accepted operation returns its existing outcome after ownership/identity checks; it does not create another purchase. Positive quantity, units, currency, precision, and SKU identity remain OQ-002/OQ-006/OQ-008.
- Shipping and any selected voucher effects must be determinable under the applicable rules. The draft can be walked through with an explicitly identified valid simulated shipping fixture and no voucher; this does not remove shipping/voucher capabilities or settle OQ-007/OQ-011.

## Main flow

1. Buyer selects cart entries, a permitted delivery address, and supported shipping choices for the participating Shops; optionally selects applicable vouchers.
2. The system checks Buyer ownership and, for an acceptance/recovery request, resolves its logical operation identity and immutable instructions first. If the same accepted operation already has an outcome, return it without creating a new purchase or requiring current cart/catalog eligibility; changed instructions under that identity are refused. For a genuinely new/uncommitted operation, resolve exact references, aggregate quantities by Shop/SKU, and evaluate current Product/Shop/SKU eligibility and available stock.
3. The system calculates a quote containing per-Shop item quantities and unit prices, item subtotal, shipping charge, attributable discounts, Shop payable amount, group total, and currency under Proposed [BR-ORDER-001](../BUSINESS_RULES.md#br-order-001). Any unresolved or invalid selected charge/discount prevents quote acceptance; no invented zero fee or discount silently fills a gap.
4. Buyer sees affected items and changes from the cart estimate, reviews the address/shipping choices and total, and explicitly accepts those facts. Quoting alone creates no Order, payment request, or reservation.
5. At checkout acceptance the system rechecks ownership, all purchasing guards, quantities, quote facts, and stock as one indivisible business outcome. A relevant fact changed since presentation requires a refreshed quote and renewed Buyer acceptance; an earlier cart/quote result is not sufficient.
6. If every selected item passes, accept one purchase group, create one `PAYMENT_PENDING` Shop order per participating Shop, preserve the accepted snapshot, and reserve all required quantities as `ACTIVE_PAYMENT`. No selected subset succeeds on its own. Assign stable identities and the common payment deadline; record the outcome and stock changes.
7. Buyer receives the group identity, each Shop order, accepted totals, and expiry. [UC-012](UC-012-process-payment.md) initiates one simulated Payment Request for the exact group amount/currency. Lost responses can be recovered using the original operation identity; retrying acceptance must not create another group or reservation.

## Alternative and error flows

- **A1 — Unauthorized request:** refuse access or mutation of another Buyer's cart/address/purchase. Seller rights to a Shop do not grant Buyer purchase access.
- **A2 — Invalid/unavailable selection:** identify affected selected entries and reasons. Reject the whole selected checkout without new Orders, holds, or payment requests; retain cart intentions so Buyer can explicitly change the selection. Unselected invalid entries do not join the checkout automatically.
- **A3 — Quote changed:** changes to price, shipping, discounts, address, or selected facts require refreshed presentation and Buyer acceptance before committing. Insufficient stock or hidden content fails the current attempt. No silent SKU/quantity substitution, higher price, or partial checkout.
- **A4 — Checkout races with risky content saving:** Accepted BR-PROD-002 prohibits acceptance after successful risky saving starts hiding. Proposed OQ-007 gives checkout acceptance and risky-save acceptance an authoritative order: hiding first rejects checkout; checkout first preserves the accepted approved snapshot and existing holds. A later risky edit prevents further purchases but does not automatically rewrite/cancel an accepted order. This ordering is a new proposal, independent of moderation-only OQ-003.
- **A5 — Stock contention or Seller adjustment:** reserve only if all current quantities can be held without breaking BR-INV-001. A competing purchase/adjustment must see a coherent accepted outcome; at most one checkout can reserve the last unit. If any selected hold cannot be obtained, leave no successful selected subset.
- **A6 — Interrupted or unknown acceptance:** recover the original outcome before creating another checkout. An accepted group has all its Shop orders/snapshots/holds; a refused group has none. Design must later prove recovery from an interruption without exposing half an accepted checkout.
- **A7 — Repeat or conflicting operation:** same Buyer operation identity and same selected/accepted facts returns the original outcome. Reuse with changed facts is refused; a deliberate new purchase requires a distinct identity and fresh guards. A replay does not extend the deadline or duplicate audit effects.
- **A8 — Payment initiation unavailable:** the accepted group remains identifiable with its holds until its deadline; recover/retry the same Payment Request under UC-012. Do not claim paid or confirmed, silently initiate a second payment, or create another checkout group. Definitive failure/expiry follows UC-012.
- **A9 — Voucher/shipping unavailable:** show the invalid or unresolved selected condition and require an explicit valid alternative or changed selection. Voucher eligibility, quota, allocation, cancellation, and refund handling remain OQ-011; discount-bearing checkout cannot be treated as ready from this draft.
- **A10 — Cart cleanup:** Proposed OQ-007 keeps cart intentions during checkout/payment; no automatic removal or quantity decrement is implied. A later cart change does not change the accepted snapshot, and a further checkout is a new purchase with fresh checks. Final cart cleanup behavior can be reviewed separately without affecting reservation ownership.

## Business rules

- Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) and Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004): publication and exact-SKU guards.
- Proposed [BR-ORDER-001](../BUSINESS_RULES.md#br-order-001): all-or-nothing group acceptance, per-Shop Orders, totals/snapshots, repeat identity, and checkout-versus-edit ordering.
- Proposed [BR-INV-001](../BUSINESS_RULES.md#br-inv-001): reservation invariants; no physical deduction at checkout.
- Proposed [BR-PAY-001](../BUSINESS_RULES.md#br-pay-001): one group payment, verified outcomes, failure/expiry and reconciliation.
- [NFR-001/003/004](../NON_FUNCTIONAL_REQUIREMENTS.md): Buyer/Shop isolation, stock contention, attributable accepted changes. Audit coverage remains OQ-012.

## State/data changes

Checkout quote has no reservation effect. Successful acceptance creates the group and per-Shop Orders in [SM-ORDER-001](../STATE_MACHINES.md#sm-order-001) and payment holds in [SM-RESERVATION-001](../STATE_MACHINES.md#sm-reservation-001). [SM-INVENTORY-001](../STATE_MACHINES.md#sm-inventory-001) changes reserved/available stock while physical stock is unchanged. Payment state is separate; Seller confirmation and physical deduction belong to UC-013, after payment under Proposed OQ-008.

## Postconditions

- Success: all selected Shop orders, accepted snapshots, and holds exist together with stable ownership/correlation, a common expiry, and a payable group amount awaiting simulated payment.
- Refusal: no new selected Shop order, reservation, or payment request; source cart remains recoverable.
- Payment and Seller confirmation remain distinct from checkout acceptance. Failure of a later payment does not mean checkout was never accepted. [UC-013](UC-013-fulfill-shop-order.md)/[UC-015](UC-015-track-cancel-orders.md) now propose a recorded per-Shop Seller deadline and pre-confirmation closure under [BR-CANCEL-001](../BUSINESS_RULES.md#br-cancel-001). Unpaid cancellation covers the original whole group; paid closure preserves its accepted group amount and sibling allocations while creating a separate compensation obligation. Concrete shipping/voucher/money rules remain open.

## Acceptance criteria

- **AC-UC-011-01 — Per-Shop split (Baseline split; Proposed group):** given eligible selections from two Shops, successful acceptance creates exactly two correctly attributed Shop orders in one purchase group, covering every selected quantity and no unselected entry.
- **AC-UC-011-02 — Private purchase access (Proposed):** another Buyer or an actor with only Shop operating rights cannot read/accept the target Buyer's checkout or use their private address; refusal leaves purchase and stock unchanged.
- **AC-UC-011-03 — Hidden before acceptance (Accepted purchase block):** given successful risky saving before checkout acceptance, checkout cannot reserve or accept a new purchase of that Product, including a previously carted selection. Whole-selection refusal is Proposed OQ-007.
- **AC-UC-011-04 — Aggregate exact SKU (Proposed):** selected repeated entries for one Shop/SKU are checked using their total quantity. If that total exceeds availability, no subset is accepted; no other SKU is substituted.
- **AC-UC-011-05 — Quote versus commit (Proposed):** quoting creates no Order, hold, or payment. A material purchase-fact change before acceptance requires refreshed quote and renewed acceptance; the original quote cannot silently accept changed terms.
- **AC-UC-011-06 — Attributable totals (Proposed):** each accepted Shop total equals its item subtotal plus shipping minus attributed discounts, and the single group amount equals the sum of Shop totals in one agreed currency. Currency/precision and concrete shipping/discount fixtures remain OQ-002/007/011.
- **AC-UC-011-07 — Stable snapshot (Proposed):** successful acceptance records exact identities, approved descriptions/variant choices, quantities/prices, item and Shop totals, charge/discount sources, currency, address, shipping choice, and deadline. Later catalog, price, cart, or address edits do not silently rewrite that snapshot.
- **AC-UC-011-08 — All or none (Proposed):** if one selected Shop's item fails validation or reservation, no selected Shop order, hold, or payment request is created; Buyer sees the affected selection and can explicitly resubmit a changed selection.
- **AC-UC-011-09 — Last unit race (Proposed):** two checkouts compete for one available unit; at most one reserves it. The loser has no partial sibling-Shop acceptance and the inventory invariant remains true.
- **AC-UC-011-10 — Hide race ordering (Proposed):** checkout acceptance before successful risky saving keeps its approved snapshot and existing holds; hiding before acceptance blocks the new purchase. No outcome accepts unapproved revised content. OQ-003 is not the authority for this ordering.
- **AC-UC-011-11 — Recover acceptance (Proposed):** repeat the same acceptance identity/facts after a lost response, including after subsequent risky hiding, cart edits, or expiry; return the original group/Shop orders/holds/deadline and current lifecycle status without another purchase or reapplying stock effects. Current offer eligibility does not invalidate retrieval of the existing outcome. Reusing that identity with different instructions is refused.
- **AC-UC-011-12 — Checkout is not stock deduction (Baseline timing; Proposed counters):** reserving quantity q decreases available by q while physical stock stays unchanged. Payment and Seller confirmation are separate observable outcomes.
- **AC-UC-011-13 — Initiation recovery (Proposed):** unavailable or unknown simulated payment initiation retains the identifiable pending group until its deadline; recovery uses its original Payment Request identity, without a second request or a paid/confirmed claim.
- **AC-UC-011-14 — No fabricated benefit/fee (Draft dependency):** invalid or unresolved selected shipping/voucher conditions cannot produce a supposedly valid final quote by silently assigning zero charge or discount. Discount fixtures/eligibility/quota behavior remain pending OQ-011.
- **AC-UC-011-15 — Auditable acceptance (Proposed):** successful acceptance links Buyer, operation/group/order/reservation identities, accepted facts/amounts, stock before/after, action, and time; replay adds no second acceptance or stock-change business audit record. Sensitive-data access/redaction remains OQ-005/012.
- **AC-UC-011-16 — Cart remains an intention (Proposed):** accepted checkout does not automatically remove source cart entries; editing those entries cannot change the accepted purchase. A new checkout uses a new identity and fresh checks.

## Traceability

SC-005/SC-012 → UC-011 → BR-PROD-002/004, BR-ORDER-001, BR-INV-001, BR-PAY-001; SM-PRODUCT/INVENTORY/RESERVATION/ORDER/PAYMENT-001; NFR-001/003/004 → AC-UC-011-01 through AC-UC-011-16. See [Traceability](../TRACEABILITY.md). Domain/design, implementation, and executed tests are pending. Criterion status does not establish execution evidence.

## Open questions

[OQ-007](../OPEN_DECISIONS.md#oq-007) grouping, snapshots, quotes, cart retention, shipping rules, and acceptance ordering; [OQ-008](../OPEN_DECISIONS.md#oq-008) reservation lifetime/confirmation; [OQ-015](../OPEN_DECISIONS.md#oq-015) payment identity/recovery; [OQ-002](../OPEN_DECISIONS.md#oq-002) quantity/currency/validation; [OQ-004](../OPEN_DECISIONS.md#oq-004) Shop/account interventions; [OQ-005](../OPEN_DECISIONS.md#oq-005) permissions; [OQ-006](../OPEN_DECISIONS.md#oq-006) SKU identity; [OQ-009](../OPEN_DECISIONS.md#oq-009) accepted-order cancellation/rejection; [OQ-011](../OPEN_DECISIONS.md#oq-011) vouchers; [OQ-012](../OPEN_DECISIONS.md#oq-012) quality/audit; [OQ-019](../OPEN_DECISIONS.md#oq-019) public/cart eligibility. None is resolved by creating this specification.
