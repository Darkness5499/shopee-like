# UC-009 — View a Product and its purchase conditions

- Implementation status: Not implemented — minimal read-only demonstration support for Part 1; advanced discovery features deferred.
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; public detail and per-SKU purchase-eligibility policy is Proposed under OQ-019. Accepted review-related hiding remains binding under BR-PROD-002.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope: SC-003, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).
- Source: baseline Product/variant/SKU detail capability; [glossary](../GLOSSARY.md); Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002); Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004) / [OQ-019](../OPEN_DECISIONS.md#oq-019).

## Goal

Let a Buyer inspect approved Product information and current SKU choices, prices, availability, reviews, vouchers, and Shop policies before choosing a potential purchase.

## Primary actor

Buyer viewing a Product.

## Supporting actors

Seller / Shop Operator maintains content and operational information through [UC-004](UC-004-create-update-product.md), UC-007, and UC-019. Moderator approves Product content through [UC-006](UC-006-review-product.md). Review and promotion publication rules belong to UC-018/019/020 and remain open; these actors do not participate in each detail request.

## Trigger

Buyer selects a discovery result or requests a Product directly, including from a previously saved reference or Cart item.

## Preconditions

- The requested Product reference can be resolved or identified as unavailable.
- Public-detail eligibility can be checked independently of any earlier discovery result. Exact Shop restrictions/permissions remain OQ-004/OQ-005; SKU validity remains OQ-002/OQ-006.
- No guest-access or account-verification policy is assumed; the applicable access boundary remains OQ-005.

## Main flow

1. Buyer requests the Product detail.
2. The system checks current public-listing eligibility under Proposed BR-PROD-004 and independently applies Accepted BR-PROD-002 review-related hiding. A prior search result or direct reference cannot bypass these checks.
3. For an eligible Product, the system presents approved Product category, descriptive content, media, and category-specific attributes together with Shop identity and applicable public Shop policies.
4. The system presents enabled valid SKU/variant choices and their current valid prices and availability. Under Proposed OQ-019, enabled out-of-stock choices remain visible with an unavailable/out-of-stock indication; disabled or invalid SKUs do not supply a public purchase choice or displayed price.
5. Buyer selects a SKU and intended quantity. The system indicates whether that choice is currently eligible for a new purchase: the Product must remain visible, the selected SKU must be enabled and valid with a valid price, and sufficient available quantity must exist. The available-stock definition remains OQ-008.
6. The system presents published reviews and rating information according to OQ-013, and available voucher/promotion information according to OQ-011. It does not invent reviews/ratings or promise a discount before the applicable eligibility check.
7. Buyer may change the SKU/quantity selection or proceed to UC-010 to manage a Cart. This use case creates no reservation, Order, or payment commitment; UC-011 checks purchase eligibility again at checkout.

## Alternative and error flows

- **A1 — Missing or no longer publicly eligible Product (Proposed response):** report that the Product is unavailable for a new purchase. Do not present protected or unapproved listing content as a substitute. Exact responses for staff-imposed restrictions remain OQ-004/OQ-005.
- **A2 — Review-hidden Product through any entry point (Accepted hiding; Proposed unavailable presentation):** from the successful save of an actual risky change, BR-PROD-002 prevents public sale detail and new purchases, including direct requests and previously selected Cart items. Report an unavailable outcome without exposing old approved sale content, pending/revised content, review notes, or withdrawal/rejection details. Hiding continues before submission, during validation failure, while withdrawn or pending, and after rejection until manual approval.
- **A3 — Out-of-stock SKU (Proposed):** show the enabled valid choice as unavailable or out of stock, retain the eligible Product detail, and do not allow the choice to progress as currently purchasable. Other enabled valid SKUs are evaluated independently. Exact inventory quantity display and calculation remain OQ-008.
- **A4 — Disabled/invalid SKU or all choices removed (Proposed):** a disabled/invalid SKU is not a public purchase choice and does not determine public price information. If no enabled valid SKU remains, the Product no longer meets the public-listing condition and returns the unavailable outcome. SKU structure/identity rules remain OQ-006.
- **A5 — Insufficient or invalid selected quantity:** report the affected choice as ineligible for that quantity and permit correction. Quantity units/limits and exact available-stock checks remain OQ-002/OQ-008. No partial purchase or reservation occurs by selecting a quantity.
- **A6 — Eligibility changes after detail was displayed:** a previous detail view creates no entitlement to buy a hidden Product, disabled SKU, former price, or unavailable stock. Current Cart and checkout actions use their own eligibility checks; UC-011 owns stock reservation and purchase totals.
- **A7 — No reviews or uncertain voucher eligibility:** show the absence of published review data without a fabricated rating; do not guarantee an unverified voucher benefit. Rating and discount rules remain OQ-013/OQ-011, including any final payable price.
- **A8 — Operational edit while review-hidden:** permitted price, SKU activation, or stock updates cannot restore public sale detail under BR-PROD-002. A successful review approval restores only content approval; normal Shop and SKU guards still apply under Proposed BR-PROD-004.
- **A9 — Existing Order references:** public-detail unavailability is not an Order cancellation, refund, stock release, or alteration of agreed purchase facts. Access to historical Order information and consequences of restrictions are specified separately under OQ-004/OQ-007/OQ-009/OQ-010.

## Business rules

- Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) governs hiding from successful risky saving until required manual approval across detail and new purchase entry points.
- Accepted [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003) governs pending withdrawal/edit/resubmit; a withdrawn submission cannot be approved and withdrawal does not restore sale.
- Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004) governs public listing versus per-SKU purchase eligibility, including out-of-stock listing behavior and disabled-SKU price exclusion. [OQ-019](../OPEN_DECISIONS.md#oq-019) tracks confirmation.
- Approved content means the high-risk descriptive content subject to moderation; price, activation, and inventory information use current permitted operational values. A valid operational update retains its manual-review exemption under BR-PROD-001.
- Published review/rating semantics remain OQ-013; voucher eligibility and price effects remain OQ-011. Available stock remains OQ-008. None of these unresolved calculations is silently fixed by this use case.

## State/data changes

Viewing and selecting Product/SKU information do not change [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), review submissions, SKU activation, inventory/reservations, Cart contents, Orders, or Payments. Cart mutation is UC-010; checkout validation/reservation is UC-011. Existing purchase history is a separate access path, not a means to republish unapproved Product content.

## Postconditions

- Success: Buyer sees approved public content, supported choices, and current purchase conditions with unavailable choices clearly distinguished.
- Unavailable Product or selection: no unapproved sale content or new purchase entitlement is supplied.
- Viewing detail has no stock or Order effect. Applicable purchase conditions must be rechecked at later purchase steps.

## Acceptance criteria

- **AC-UC-009-01 — Approved detail:** given a Product meeting current public-listing guards, when Buyer views it, the response contains approved descriptive content/category/media/attributes and the applicable Shop information; it does not include content from a pending, withdrawn, or rejected submission.
- **AC-UC-009-02 — Direct lookup cannot bypass hiding (Accepted policy; Proposed response):** given a Product hidden under BR-PROD-002, when Buyer requests detail directly or through a previously obtained reference, new purchase remains unavailable and no old approved sale listing or revised unapproved content is exposed. The Proposed response is an unavailable outcome.
- **AC-UC-009-03 — Hidden through review recovery (Accepted policy):** given review-related hiding, detail/new-purchase unavailability persists during preparation, validation failure, pending review, withdrawal, and rejection until required manual approval; withdrawal or an operational edit cannot restore sale.
- **AC-UC-009-04 — Visible versus purchasable (Proposed under OQ-019):** given an approved Product at a Shop permitted to sell with an enabled valid SKU at zero available stock, detail remains eligible and shows the SKU as out of stock; no new purchase is eligible for that SKU. Stock scenarios depend on OQ-008.
- **AC-UC-009-05 — Per-SKU eligibility (Proposed under OQ-019):** given a visible Product with multiple enabled valid SKUs, when Buyer selects a SKU/quantity, the selection is eligible only with a valid price and sufficient available stock for that SKU. An available alternative SKU does not make an unavailable selected SKU purchasable.
- **AC-UC-009-06 — Disabled/invalid prices excluded (Proposed under OQ-019):** given enabled valid and disabled/invalid SKUs, disabled/invalid SKUs are not public purchase choices and their prices cannot determine displayed Product price information.
- **AC-UC-009-07 — All SKUs disabled (Proposed under OQ-019):** given otherwise approved content, when all SKUs are disabled, direct public detail returns the unavailable outcome because no enabled valid SKU remains. Re-enabling a valid SKU cannot bypass existing review-related hiding or Shop restrictions.
- **AC-UC-009-08 — Current guards after approval (Proposed under OQ-019):** given manual approval of revised content, public detail returns only when normal listing, Shop, and enabled-valid-SKU guards hold; the selected SKU becomes purchasable only when sufficient quantity is also available. Approval alone cannot make a disabled SKU or insufficient stock purchasable, and zero available stock alone does not prevent an otherwise eligible detail view.
- **AC-UC-009-09 — Stale selection:** given a prior detail view or Cart reference, when a risky change is successfully saved, subsequent new purchase is prohibited under Accepted BR-PROD-002. Other price/activation/stock changes are evaluated at the current purchase step; the prior view does not bypass validation.
- **AC-UC-009-10 — Reviews and benefits:** given no published review data or an unverified voucher benefit, detail shows no fabricated numeric rating and does not present the benefit as guaranteed. Concrete published-rating/voucher examples depend on OQ-013/OQ-011.
- **AC-UC-009-11 — Read-only detail:** given repeated detail and SKU/quantity selections, Product/review state, SKU activation, inventory/reservations, Cart contents, Orders, and Payments remain unchanged.
- **AC-UC-009-12 — Existing purchases remain separate:** when public detail becomes unavailable, that lookup does not itself cancel/refund an existing Order, release/deduct stock, or rewrite historical purchase facts. Historical display and any staff-restriction effects remain OQ-004/OQ-007/OQ-009/OQ-010.

## Traceability

- SC-003 → UC-009 → Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002), [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003); Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004); [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001); Proposed [NFR-005](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-005) → AC-UC-009-01 through AC-UC-009-12.
- Related use cases: [UC-008](UC-008-discover-products.md) discovery and [UC-010](UC-010-manage-cart.md) Cart have Draft specifications; UC-007 stock, UC-011 checkout, UC-015 purchase history, UC-018 reviews, and UC-019/020 promotions remain catalog entries until specified.
- Canonical coverage: [TRACEABILITY.md](../TRACEABILITY.md). Domain/design and acceptance-test artifacts remain pending. NFR-005 workload/latency verification remains Proposed under OQ-012; it does not relax Accepted hiding.

## Open questions

[OQ-002](../OPEN_DECISIONS.md#oq-002) SKU/quantity validity; [OQ-004](../OPEN_DECISIONS.md#oq-004) restrictions and existing Orders; [OQ-005](../OPEN_DECISIONS.md#oq-005) Shop selling/access eligibility; [OQ-006](../OPEN_DECISIONS.md#oq-006) SKU identity; [OQ-007](../OPEN_DECISIONS.md#oq-007) historical purchase facts; [OQ-008](../OPEN_DECISIONS.md#oq-008) available stock and displayed quantities; [OQ-009](../OPEN_DECISIONS.md#oq-009) cancellation/rejection; [OQ-010](../OPEN_DECISIONS.md#oq-010) historical/after-sales lifecycle; [OQ-011](../OPEN_DECISIONS.md#oq-011) voucher terms; [OQ-012](../OPEN_DECISIONS.md#oq-012) quality targets; [OQ-013](../OPEN_DECISIONS.md#oq-013) reviews/ratings; [OQ-019](../OPEN_DECISIONS.md#oq-019) public detail versus purchasability. OQ-001/OQ-003/OQ-016/OQ-017/OQ-018 are resolved and are not reopened here.
