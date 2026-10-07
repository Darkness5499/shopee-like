# UC-010 — Manage a multi-shop cart

## Metadata

- Status: Draft; cart presentation, quantity validation, and price-change handling are Proposed under OQ-019. Review-related hiding and blocked new purchases follow Accepted BR-PROD-002.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-004; dependencies SC-003/SC-005/SC-012 in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Let a Buyer maintain intended SKU purchases across Shops and understand current eligibility before checkout, without treating cart contents as reserved stock or confirmed prices.

## Primary actor

Buyer acting through a User account. Guest-cart behavior and exact account prerequisites remain under OQ-005.

## Supporting actors

Seller / Shop Operator changes Product content, SKU price/activation, and stock through UC-004/007. Moderator approval in UC-006 affects public eligibility. These actors do not gain access to the Buyer's private cart by operating a Shop.

## Trigger

Buyer requests to add a selected SKU from [UC-009](UC-009-view-product.md), inspect the cart, change an intended quantity, remove an entry, or proceed to checkout.

## Preconditions

- The target cart belongs to the acting Buyer; access to another Buyer's cart is refused. Exact account verification/authorization prerequisites await OQ-005.
- For an addition or increase, the Product and exact selected SKU are identifiable and eligible under Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004). Requested quantity must satisfy the agreed unit/range rules; those boundaries remain OQ-002/OQ-008.
- A saved cart entry is an intention to purchase, not an Order, stock reservation, or completed payment.

## Main flow

1. Buyer identifies the intended Product/SKU and desired quantity, or an existing cart entry to inspect/change/remove.
2. The system checks cart ownership and resolves the exact Product, SKU, and Shop references. It does not silently substitute another variant or SKU.
3. For an addition or quantity increase, the system checks Product/SKU eligibility, a valid positive requested quantity, the current permitted price, and sufficient currently available stock for the resulting total intended quantity of the exact Shop/SKU across the cart, not just the added increment. Such a check is advisory; it does not hold stock.
4. The system records the permitted cart change. An explicit removal deletes that Buyer's selected cart entry without requiring its Product to remain publicly available.
5. When reporting the cart, the system evaluates each entry's current Product/SKU eligibility and permitted price, and checks quantity availability against the total intended quantity for each exact Shop/SKU. It identifies affected entries and any insufficient aggregate quantity, and reports changed prices against the previously presented amount, following Proposed OQ-019. Individually sufficient entries must not imply that their combined quantity is available.
6. Buyer receives the intended items grouped by Shop, the current per-entry eligibility/price/quantity findings, and an estimated eligible-item amount. Shipping, vouchers, and final payable amounts are evaluated in UC-011, not promised by the cart estimate.
7. Buyer may change/remove entries or request checkout for intended entries through UC-011. Checkout rechecks all applicable guards before accepting a purchase or reservation; it does not rely on an earlier cart result.

## Alternative and error flows

- **A1 — Cart access denied:** another Buyer or unauthorized actor cannot read/change the target cart. Leave its contents unchanged.
- **A2 — Invalid quantity:** report the unmet quantity rule and leave the attempted cart change unapplied. Positive quantities and exact units/limits remain Proposed/Open under OQ-002/OQ-008. Removal is a separate permitted operation.
- **A3 — Product hidden after risky saving:** Accepted BR-PROD-002 makes the Product unavailable for new purchases even if it was previously selected. Proposed OQ-019 keeps the existing entry identifiable and removable as unavailable, without displaying unapproved content or representing it as an active offer. It cannot be treated as an eligible checkout item.
- **A4 — SKU disabled, missing, or insufficient stock:** report the affected entry and unavailable selection/quantity. Refuse a new addition or increase that fails the current checks; existing entries may remain unavailable so the Buyer can reduce/remove them. Do not substitute another SKU. Exact SKU identity recovery remains OQ-006.
- **A5 — Operational price change:** report the current permitted price and the change from the previously presented amount. An earlier cart price is not binding. Final price/snapshot confirmation at checkout remains OQ-007.
- **A6 — Multiple Shops with mixed eligibility:** report eligibility per entry and preserve Shop attribution. Whether a checkout attempt containing invalid entries is entirely refused or can partially succeed belongs to OQ-007/UC-011; this use case does not decide it.
- **A7 — Availability changes after a successful cart check:** a later stock/price/visibility change is considered at the next evaluation and at checkout. Cart membership does not defeat Accepted Product hiding or grant priority over other purchasers.
- **A8 — Remove an unavailable entry:** permit the cart owner to remove an entry even if its Product has been hidden or its SKU disabled. Removing it does not release stock because cart membership did not reserve any stock.

## Business rules

- Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002): review-related hiding blocks new purchases, including previously selected cart items.
- Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004): common listing/purchase guards, current operational values, unavailable cart entries, and advisory checks.
- SC-004/SC-012 and the [Cart definition](../GLOSSARY.md): multi-shop cart membership does not establish an Order or reservation; stock reservation begins in checkout under UC-011/OQ-008.
- Authorization follows [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001); cart-specific access expectations are Proposed pending OQ-005. Audit scope for cart changes remains OQ-012.

## State/data changes

Permitted changes affect only the acting Buyer's cart references and intended quantities. Viewing/revalidating the cart does not edit Product/SKU data or approve content. No inventory reservation/release/deduction, Order creation, payment, or Product moderation transition occurs here. Per-entry available/unavailable findings describe the current evaluation, not additional Product state codes. Separate inventory/reservation/order/payment models are Proposed under OQ-007/OQ-008/OQ-015; their policies remain unconfirmed.

## Postconditions

- Success: permitted cart intentions are recorded and Buyer receives current per-entry eligibility, price, and quantity findings with Shop attribution.
- Failure: an unauthorized or invalid attempted change does not change protected cart data.
- Checkout remains responsible for final purchasing guards, reservation, amounts, and Orders. A successful cart action does not guarantee a later purchase.

## Acceptance criteria

- **AC-UC-010-01 — Multi-shop intentions (Draft):** given a Buyer and eligible selected SKUs from two Shops, when permitted items are saved in the cart, each retains its correct Shop/Product/SKU identity and intended quantity; no Order is created.
- **AC-UC-010-02 — Cart ownership (Proposed):** given another Buyer's cart, when an actor without permission requests to read/change it, no private cart contents are disclosed and no change is applied.
- **AC-UC-010-03 — No stock hold from cart membership (Baseline boundary):** when Buyer adds, changes, reads, or removes a cart entry, no inventory reservation, stock deduction, or stock release is performed merely because of that cart operation.
- **AC-UC-010-04 — Previously carted Product hidden (Accepted policy; Proposed presentation):** given an existing cart entry and a subsequent successful risky Product save, any subsequent purchase eligibility evaluation treats the Product as unavailable under BR-PROD-002. Under Proposed OQ-019 the entry remains removable, exposes no unapproved content, and cannot be represented as an eligible purchase.
- **AC-UC-010-05 — Invalid selection/quantity (Proposed):** given a hidden Product, disabled/missing SKU, invalid requested quantity, or insufficient currently available quantity for the resulting total intended quantity of the exact Shop/SKU, a new addition/increase reports the affected item and unmet rule without applying the attempted cart change or substituting a different SKU. The stock check covers the total across that SKU's cart entries rather than only the increment. Exact quantity fixtures await OQ-002/OQ-008.
- **AC-UC-010-06 — Current price and change disclosure (Proposed):** given a cart entry previously presented at one price and a permitted Seller price update, when Buyer next evaluates the cart, the current price and its change are reported; the earlier amount is not promised as the checkout price.
- **AC-UC-010-07 — Mixed eligibility (Proposed):** given eligible and unavailable entries across Shops, cart evaluation reports eligibility independently per entry without removing Shop attribution or treating unavailable entries as eligible. The outcome of a mixed checkout remains OQ-007.
- **AC-UC-010-08 — Remove unavailable entry (Proposed):** given a cart owner's entry whose Product is hidden or SKU disabled, when Buyer removes it, that entry is removed without altering Product visibility, releasing stock, or changing any existing Order.
- **AC-UC-010-09 — Checkout must recheck (Draft dependency):** given a successful cart evaluation followed by a change in Product visibility, SKU activation, price, or available quantity, a purchase attempt must be evaluated against the current applicable guards in UC-011; the previous cart result does not grant purchase eligibility. Final checkout acceptance/partial-failure tests remain pending OQ-007/OQ-008.

## Traceability

- SC-004 → UC-010 → BR-PROD-002/004, [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), NFR-001 → AC-UC-010-01 through AC-UC-010-09.
- Product detail: [UC-009](UC-009-view-product.md); stock availability: [UC-007](UC-007-manage-inventory.md); final purchase/reservation: [UC-011](UC-011-checkout.md). These now have Draft specifications; quote/snapshot/grouping/hold policies remain Proposed, with analysis/design and executed verification pending.

## Open questions

[OQ-019](../OPEN_DECISIONS.md#oq-019) public/cart eligibility and price-change presentation; [OQ-002](../OPEN_DECISIONS.md#oq-002) valid operational values and quantity boundaries; [OQ-004](../OPEN_DECISIONS.md#oq-004) restrictions; [OQ-005](../OPEN_DECISIONS.md#oq-005) account/cart authorization; [OQ-006](../OPEN_DECISIONS.md#oq-006) SKU identity; [OQ-007](../OPEN_DECISIONS.md#oq-007) checkout price/snapshots and partial failures; [OQ-008](../OPEN_DECISIONS.md#oq-008) availability and reservations; [OQ-012](../OPEN_DECISIONS.md#oq-012) audit coverage. Accepted review hiding remains OQ-001/OQ-016/OQ-017/OQ-018.
