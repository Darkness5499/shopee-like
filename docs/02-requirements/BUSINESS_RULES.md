# Business Rules

- Status: Draft register — BR-PROD-001 Baseline; BR-PROD-002/003 contain Accepted policy; BR-PROD-004 and inventory/purchase/fulfillment/cancellation/shipment rules are Proposed; staff intervention and after-sales details remain open.
- Owner: Project owner.
- Source: existing moderation decision and [Functional Scope](../01-product/FUNCTIONAL_SCOPE.md), SC-010/SC-011/SC-016/SC-017/SC-023.
- Identifier convention: `BR-{DOMAIN}-###`; the existing `BR-PROD-001` is retained.

<a id="br-prod-001"></a>
## BR-PROD-001 — Product Moderation

### Status and applicability

- Status: Baseline — previously documented policy retained; separate owner confirmation provenance is unavailable.
- Applies to: Seller Product submissions, Moderator decisions, and edits of active Product content.
- Rationale: combine deterministic data checks with human content review.
- Related use cases: [UC-004](USE_CASES/UC-004-create-update-product.md), [UC-005](USE_CASES/UC-005-submit-product.md), [UC-006](USE_CASES/UC-006-review-product.md).
- Related lifecycle: [SM-PRODUCT-001](STATE_MACHINES.md#sm-product-001).

### Decision

Product moderation uses a hybrid model: automatic validation plus manual review by Internal Staff.

### Responsibilities

- **Seller:** creates, updates, and submits products for review.
- **System:** validates deterministic business rules automatically.
- **Moderator:** performs manual product review and approves or rejects submissions.
- **Admin:** manages categories, policies, validation rules, and exceptional interventions; Admin is not the default day-to-day reviewer.

### Automatic validation

The system must reject submission before manual review when required data or deterministic rules are invalid, including:

- Missing required fields.
- Invalid price.
- Negative inventory.
- Invalid category structure.
- Unsupported image format.
- Basic forbidden-word validation in product title or description.

### Manual review

A Moderator reviews products that pass automatic validation.

The following recorded outcomes are specified for first publication in UC-006. Review-related visibility is defined in Accepted BR-PROD-002; withdrawal/edit/resubmit, competing actions, and repeated requests follow Accepted BR-PROD-003/OQ-003.

- Approve: the Product becomes `ACTIVE`.
- Reject: the Product becomes `REJECTED` and must include a rejection reason.

### Re-review policy for active products

Changes to high-risk content require manual re-review:

- Category.
- Product name.
- Description.
- Images or video.
- Category-specific attributes.
- Adding or changing product variants.

Operational changes take effect immediately and do not require re-review:

- SKU price.
- Inventory quantity.
- SKU activation or deactivation.
- Seller's internal SKU code.

Draft clarification: authorization and validation of changed operational data should still apply; exemption from manual review is not permission to apply invalid data. This is a proposed refinement of the submission-validation rule, not a requirement to revalidate every unchanged listing field on each price or inventory edit. Exact boundaries remain open under OQ-002/OQ-005.

### Exceptions and unresolved details

- Admin exceptional intervention is part of the baseline responsibility, but its actions/guards are not specified; it is not an automatic bypass of validation or the default Moderator workflow.
- Review-related hiding and recovery follow Accepted [BR-PROD-002](#br-prod-002), [OQ-001](OPEN_DECISIONS.md#oq-001), [OQ-016](OPEN_DECISIONS.md#oq-016), and [OQ-017](OPEN_DECISIONS.md#oq-017).
- Required fields, allowed prices/currency/precision, inventory units, accepted formats, category checks, and forbidden-word policy await [OQ-002](OPEN_DECISIONS.md#oq-002).
- Pending-submission withdrawal/edit/resubmit and competing/repeated actions follow Accepted [BR-PROD-003](#br-prod-003), [OQ-018](OPEN_DECISIONS.md#oq-018), and [OQ-003](OPEN_DECISIONS.md#oq-003).
- Staff revision requests, staff-imposed hiding/locking, and intervention recovery await [OQ-004](OPEN_DECISIONS.md#oq-004). Review-related hiding/rejection recovery follows BR-PROD-002.
- Variant/SKU identity rules await [OQ-006](OPEN_DECISIONS.md#oq-006).

### Acceptance coverage

See `AC-UC-004-01` through `AC-UC-004-11`, `AC-UC-005-01` through `AC-UC-005-09`, and `AC-UC-006-01` through `AC-UC-006-10` in the linked specifications. Boundary values and conditional proposal criteria remain draft; acceptance criteria are not test execution evidence.

<a id="br-prod-002"></a>
## BR-PROD-002 — Product visibility during re-review and rejection

- Status: Accepted for visibility from successful risky edit saving until manual approval; withdrawal/edit/concurrency behavior is covered by Accepted BR-PROD-003; staff intervention details remain Open.
- Owner: Project owner.
- Source: [OQ-001](OPEN_DECISIONS.md#oq-001), owner selected option 2; [OQ-016](OPEN_DECISIONS.md#oq-016), owner selected option 1; [OQ-017](OPEN_DECISIONS.md#oq-017), owner explicitly selected hiding on saving risky changes. Confirmed on 2026-10-07.
- Scope: SC-010/SC-011/SC-016; Buyer discovery/detail/cart/checkout SC-002/SC-003/SC-004/SC-005.
- Applies to: a previously active Product from successful saving of actual high-risk changes requiring re-review through correction, submission, manual review, and any rejection until manual approval under BR-PROD-001.

### Rule

Immediately on successful saving of actual high-risk changes requiring re-review, hide the entire Product from Buyer-facing sale and stop new purchases, including through previously selected cart items. The previous approved content does not remain on sale. Submission is not required to start hiding; opening the editor or a failed save alone does not trigger it. Operational-only changes keep their review exemption.

Keep the Product hidden while revised content is unfinished or not submitted, after automatic-validation failure, and during manual review. Successful submission or automatic checks alone do not restore sale.

If the revised content is rejected, record the reason and keep the entire Product hidden. Previously approved content is not automatically restored. The Seller must correct and resubmit the content, pass automatic validation, and obtain manual approval; editing/saving or successful automatic validation alone does not restore sale. This restriction continues through correction, failed validation, and renewed pending review.

On manual approval, revised content becomes eligible for publication under the ordinary Shop/SKU/stock purchasing guards.

Operational price, inventory, SKU activation, and internal-code updates do not bypass this Product-level sales restriction or replace the required content approval.

### Rationale and boundary

This records the owner's chosen visibility policy. It does not assign a new `HIDDEN` state code or staff-sanction reason; temporary review hiding and exceptional staff hiding/locking are distinct business causes.

Initial hide timing is Accepted under [OQ-017](OPEN_DECISIONS.md#oq-017); draft-save validity remains OQ-002. Pending risky-content withdrawal/edit/resubmit and competing/repeated actions follow Accepted [BR-PROD-003](#br-prod-003). After-rejection recovery is Accepted under [OQ-016](OPEN_DECISIONS.md#oq-016). Existing-order handling and staff interventions remain under OQ-004/OQ-007/OQ-009; no order cancellation, refund, stock release, or rollback is implied by these visibility decisions alone.

### Related use cases, states, and acceptance criteria

- [UC-004](USE_CASES/UC-004-create-update-product.md): AC-UC-004-07, operational updates cannot bypass review-related hiding; AC-UC-004-08, successful risky saving hides before submission.
- [UC-005](USE_CASES/UC-005-submit-product.md): AC-UC-005-07, re-review submission keeps the already-hidden Product unavailable for sale; validation failure does not restore it.
- [UC-006](USE_CASES/UC-006-review-product.md): AC-UC-006-08, approval makes revised content eligible for publication; AC-UC-006-09, rejection keeps it hidden through correction/resubmission until approval.
- UC-008/009/010: Draft discovery/detail/cart specifications apply Accepted hiding; their new presentation policies remain Proposed. [UC-011](USE_CASES/UC-011-checkout.md), AC-UC-011-03, covers the final purchase block; checkout-versus-edit ordering in AC-UC-011-10 remains Proposed under OQ-007.
- [SM-PRODUCT-001](STATE_MACHINES.md#sm-product-001): accepted pending/rejected/correction visibility and recovery behavior; code mappings remain Proposed.

<a id="br-prod-003"></a>
## BR-PROD-003 — Withdraw a pending Product review submission

- Status: Accepted for Seller withdrawal/edit/resubmit, competing actions, and repeated-request behavior.
- Owner: Project owner.
- Source: [OQ-018](OPEN_DECISIONS.md#oq-018) for withdrawal/edit/resubmit and [OQ-003](OPEN_DECISIONS.md#oq-003) for competing/repeated actions, confirmed 2026-10-07.
- Scope: SC-010/SC-011/SC-016; UC-004/005/006.
- Applies to: high-risk Product content already submitted and still pending manual review.

### Rule

An authorized Seller may withdraw a pending review submission before editing its submitted high-risk content. After withdrawal succeeds, that submission is no longer eligible for approval or rejection. Seller may save further changes and submit the revised content again; automatic validation and manual review run again for the new submission.

The entire Product remains hidden throughout withdrawal, editing, failed or successful validation, resubmission, and renewed review under BR-PROD-002. Withdrawal, editing, resubmission, and automatic-validation success do not restore sale. Operational-only changes retain their manual-review exemption but cannot bypass the existing hiding restriction.

### Boundary and open details

Each pending submission represents one exact submitted content set. Withdrawal is allowed only while that submission remains pending. Among withdrawal, approval, and rejection competing for the same submission, the first action accepted by the system wins:

- Accepted withdrawal makes later approval/rejection invalid.
- Accepted approval/rejection makes later withdrawal and a competing opposite decision invalid.
- Repeating the same action returns the existing outcome without another transition, business side effect, notification, publication action, or audit effect.
- Repeating unchanged submission while it is already pending returns the existing pending submission rather than creating duplicate moderation work.
- Revised content after withdrawal must be validated into a new eligible submission; it cannot inherit validation or a decision from the withdrawn submission.

This rule requires stable submission identity and indivisible acceptance of a winning business action without prescribing database locking, API design, or service topology.

### Related use cases and acceptance criteria

- UC-004: AC-UC-004-09 permits editing only after successful withdrawal and preserves hiding.
- UC-005: AC-UC-005-05 and AC-UC-005-08 cover repeated submission and a new eligible resubmission.
- UC-006: AC-UC-006-07 and AC-UC-006-10 cover withdrawn submissions and competing/repeated decisions.
- SM-PRODUCT-001: withdrawal/correction/resubmission behavior; state-code mapping remains Proposed.

<a id="br-prod-004"></a>
## BR-PROD-004 — Public visibility and SKU purchase eligibility

- Status: Proposed; Accepted BR-PROD-002 takes precedence for review-related hiding and blocked new purchases.
- Owner: Project owner.
- Source: SC-002/SC-003/SC-004/SC-005/SC-010/SC-011/SC-012, BR-PROD-001/002, and [OQ-019](OPEN_DECISIONS.md#oq-019).
- Applies to: public discovery/detail, cart evaluation, and new-purchase eligibility in UC-008/009/010/011.

### Proposed rule

A Product is eligible for a public sale listing when its current high-risk content has been approved, its Shop is permitted to sell, no applicable hiding/restriction blocks sale, and it has at least one enabled SKU with valid operational data. Shop restrictions and exact SKU validity remain under OQ-004/OQ-005 and OQ-002/OQ-006; this rule does not define new account/Shop state codes.

Approved high-risk content is combined with current permitted operational price, stock, and SKU activation values. Operational values do not need a new Moderator approval merely because they changed under BR-PROD-001. Disabled or invalid SKUs must not supply a purchase offer or the listing's displayed/filterable price. A Product price range uses its enabled valid SKU offers, including offers temporarily at zero available stock.

Visibility and purchase eligibility are separate. An enabled valid SKU at zero available stock may remain visible with an out-of-stock/unavailable result. If every SKU is disabled or invalid, the Product is absent from public sale listings. Neither condition creates a manual-review requirement. Re-enabling a valid SKU or increasing available stock may restore ordinary listing/purchase eligibility only if the other guards, including BR-PROD-002, are satisfied.

A new purchase requires the Product to satisfy the listing guards and the selected SKU to remain enabled and valid with enough quantity available for the requested purchase. Search/detail/cart availability is information at the time evaluated, not a stock hold or a price guarantee. Checkout must evaluate the applicable guards again before accepting a purchase/reservation; exact stock timing and checkout consistency remain OQ-007/OQ-008.

### Accepted hiding boundary and proposed presentation

Accepted BR-PROD-002 hides the entire Product from sale and blocks new purchases from successful actual risky saving until revised content receives manual approval. It applies before submission and through failed validation, review, withdrawal/editing, and rejection. No public-sale evaluation may use the old approval to bypass this restriction.

Proposed presentation under OQ-019: public discovery excludes such Products; direct Product lookup reports unavailable without displaying unapproved high-risk content or enabling a new purchase. An existing cart item may remain identifiable and removable as unavailable. It must not expose unapproved content, contribute as an eligible checkout item, or silently change to another SKU. Previously presented safe identification, if retained, must not imply an active sale listing.

Cart membership and edits create no stock reservation, Order, payment, or fixed-price entitlement. A new addition/increase must pass current Product/SKU eligibility and positive-quantity checks; evaluate sufficient available stock against the resulting total intended quantity for the exact Shop/SKU across the cart, not merely the added increment. An invalid attempt applies no cart change. Re-evaluate current permitted prices and eligibility when the Buyer reads/changes the cart; disclose price changes from the previously presented amount. Existing unavailable entries may be reduced/removed without becoming eligible by that action alone. Checkout rechecks rather than relying on a prior cart result. Final price acceptance, snapshots, sibling-item failure, and checkout-versus-edit ordering remain OQ-007/OQ-008.

### Boundaries and acceptance coverage

Product hiding or operational changes alone do not cancel, refund, or rewrite an existing Order. Existing-order snapshots and subsequent lifecycle actions belong to OQ-007/OQ-009/OQ-010. OQ-003's first-accepted-action-wins policy applies to competing moderation actions, not a newly assumed purchase race policy.

- [UC-008](USE_CASES/UC-008-discover-products.md): discovery eligibility, out-of-stock presentation, current SKU prices, and Accepted review hiding.
- [UC-009](USE_CASES/UC-009-view-product.md): detail eligibility, SKU selection, direct lookup, and current operational values.
- [UC-010](USE_CASES/UC-010-manage-cart.md): cart eligibility, unavailable retained items, current prices, and no reservation from cart membership.
- [UC-011](USE_CASES/UC-011-checkout.md): final purchasing guards, aggregate quantities, and quote revalidation are Draft; grouping/ordering/snapshots remain Proposed under OQ-007/OQ-008.
- [SM-PRODUCT-001](STATE_MACHINES.md#sm-product-001): public visibility and purchasability are guards, not new accepted Product state codes.
- Open decisions: [OQ-019](OPEN_DECISIONS.md#oq-019), OQ-002/004/005/006/007/008.

<a id="br-inv-001"></a>
## BR-INV-001 — Protected SKU inventory and reservations

- Status: Proposed; Baseline SC-012 reserve/release/deduct timing is preserved. Confirmation remains Seller acceptance, with its payment prerequisite Proposed.
- Owner: Project owner.
- Scope/source: SC-012/SC-005/SC-013; [OQ-008](OPEN_DECISIONS.md#oq-008), dependent OQ-002/005/006/007/009/010/015.

### Proposed rule

For each exact Shop/SKU, `reserved` equals the sum of its effective `ACTIVE_PAYMENT` and `ACTIVE_PAID` holds; `available = on_hand - reserved`, with `on_hand >= reserved >= 0`. All quantities use the agreed unit/precision. `on_hand` denotes recorded physical stock, not a real warehouse integration.

An authorized Seller stock-in increases on_hand; a correction sets a validated total based on current stock/holds. Neither action changes holds, and a total below reserved is refused. Preserve concurrent accepted changes or refuse a stale correction for renewed review. A same-total correction is a no-op. Identity-preserving repeats apply no extra movement; a separately identified deliberate later operation is checked afresh.

Checkout reserves exact aggregate Shop/SKU quantities in one accepted group outcome under BR-ORDER-001: on_hand unchanged, reserved increased, available decreased. No reservation comes from cart membership or quote presentation.

Timely verified payment success changes all group holds from `ACTIVE_PAYMENT` to `ACTIVE_PAID`; all quantity totals remain unchanged. Eligibility is checked at business-effect acceptance strictly before the common unpaid deadline. A provider timestamp or callback receipt alone cannot extend a hold. An overdue unpaid hold remains included in reserved until its recorded expiry/release, even though it cannot convert to paid.

Failure/unpaid expiry releases unpaid holds once: reserved decreases, available increases, on_hand unchanged. The original unpaid deadline cannot release paid holds. Seller confirmation of an eligible paid Shop order consumes all its paid holds and deducts on_hand by the same quantities as the reserved decrease, leaving available unchanged. Each Shop order's required lines transition together; sibling Shop orders remain separate after payment.

Released/expired/consumed holds cannot be reused, transferred to a different SKU/purchase, or consumed/released again. Late/conflicting payment follows BR-PAY-001. Cancellation/rejection and paid-hold timeout use the Proposed owning closure in [BR-CANCEL-001](#br-cancel-001); restock and after-sales actions remain OQ-010. Refund or delivery events alone do not restore stock.

### Rationale, coverage, and limits

This protects sold intentions while keeping payment and Seller confirmation distinct, without choosing a database/lock/queue mechanism. Operational inventory edits cannot bypass Accepted Product hiding. Allowed units/bounds/reasons, SKU identity, permissions, paid-hold limit, and cancellation/restock remain open.

- [UC-007](USE_CASES/UC-007-manage-inventory.md): AC-UC-007-01 through 18; [UC-011](USE_CASES/UC-011-checkout.md): AC-UC-011-04/08/09/12; [UC-012](USE_CASES/UC-012-process-payment.md): AC-UC-012-03/06 through 10/15 through 17/23/24.
- [SM-INVENTORY-001](STATE_MACHINES.md#sm-inventory-001), [SM-RESERVATION-001](STATE_MACHINES.md#sm-reservation-001); NFR-003/004. [UC-013](USE_CASES/UC-013-fulfill-shop-order.md) and [UC-015](USE_CASES/UC-015-track-cancel-orders.md) now specify Proposed pre-confirmation closure/paid-hold release. Consumed-hold restoration/restock remains pending UC-016/017/OQ-010.

<a id="br-order-001"></a>
## BR-ORDER-001 — Checkout acceptance, totals, and purchase facts

- Status: Proposed; the Baseline split into separate Orders per Shop is retained.
- Owner: Project owner.
- Scope/source: SC-004/005/006/012/013; [OQ-007](OPEN_DECISIONS.md#oq-007), dependent OQ-002/004/005/006/008/009/011/019.

### Proposed rule

Buyer explicitly selects the intended checkout set. Accept all of that set together or none; an unselected cart entry is outside the attempt. Resolve exact Shop/Product/SKU identity and aggregate quantities for the same Shop/SKU. No silent substitution, dropped failed Shop, partial order, or unaccepted changed price is permitted.

Quotation checks ownership, publication/SKU eligibility, price, quantity/stock, address/shipping and any voucher conditions, but creates no stock entitlement. At acceptance recheck all applicable guards and presented purchase facts as one indivisible business outcome with snapshots, Orders and holds. Material changes require refreshed presentation and Buyer acceptance. Failure leaves no new selected Orders, holds, or payment request.

Successful acceptance creates one purchase group owned by Buyer and one Shop order per selected Shop. Each Shop payable amount equals item subtotal plus shipping minus attributed discounts; group payable amount equals the sum of Shop payable amounts in one agreed currency. Concrete money/rounding and fee/discount rules must be supplied before those dependent paths become implementation-ready. Never invent zero fees or benefits for unresolved policy.

The accepted immutable purchase snapshot contains exact identities, approved product/variant descriptions, quantities/unit prices, item subtotals, shipping/discount amounts and sources, per-Shop/group amounts, currency, address, shipping choice and payment deadline. Later catalog/cart/address edits do not silently rewrite it. Source cart intentions remain until Buyer changes/removes them; new checkout requires a new deliberate identity and fresh guards.

Checkout acceptance must be ordered with relevant successful Product/offer changes. If review-related hiding is accepted first, Accepted BR-PROD-002 blocks new checkout. If checkout is accepted first, its approved snapshot and existing holds remain; later risky saving blocks further purchases without automatically cancelling/refunding that purchase. This purchase ordering is Proposed OQ-007; Accepted moderation concurrency OQ-003 does not decide it. Existing-order consequences of Shop/staff restriction or SKU reidentification remain open.

After ownership checks, resolve the stable Buyer operation identity and immutable instructions before new-action eligibility checks. An accepted repeat returns its original facts and current lifecycle status, even if the cart/catalog later changed, without another group, hold, payment, deadline extension or effective audit change. Reuse with different instructions is refused. New/uncommitted acceptance still checks all current guards. An interrupted/unknown acceptance must be recovered to all accepted facts or none before claiming success or creating another purchase.

### Rationale, coverage, and limits

One group gives Buyer a clear initial checkout/payment outcome while preserving per-Shop fulfillment. Payment grouping does not give a Shop access to sibling-Shop private records or the entire Buyer's purchase.

- [UC-011](USE_CASES/UC-011-checkout.md): AC-UC-011-01 through 16; [UC-012](USE_CASES/UC-012-process-payment.md): AC-UC-012-01/13/19/24.
- [SM-ORDER-001](STATE_MACHINES.md#sm-order-001); BR-INV-001/PAY-001; NFR-001/003/004. [BR-ORDER-002](#br-order-002) and [BR-CANCEL-001](#br-cancel-001) extend this proposal to Seller fulfillment and cancellation without recalculating the group amount. Concrete shipping/voucher rules, restriction consequences, completion and refund execution remain pending.

<a id="br-pay-001"></a>
## BR-PAY-001 — Verified simulated payment and exception recovery

- Status: Proposed; Baseline simulated signature verification, idempotency, retry/reconciliation and success/failure/expiry capability is retained.
- Owner: Project owner.
- Scope/source: SC-006/026, related SC-005/012/022; [OQ-015](OPEN_DECISIONS.md#oq-015), OQ-007/OQ-008 and dependent OQ-002/005/009/010/012.

### Proposed rule

One accepted purchase group has one stable logical Payment Request identity for its snapshotted group amount/currency. Identity is available for recovery even before simulated provider creation succeeds. Unknown creation/transport outcome is not verified failure; recover/query/retry that same logical request without creating another payable request or extending holds.

After Buyer ownership checks, an already-completed request returns its established outcome/current Order progress before pending-only initiation guards; it never initiates again. Before callback effects, verify authenticity and required structure. Authenticated evidence under an already-known provider event identity is compared with that original record first: exact repeats return the prior result, changed immutable facts require reconciliation of the original association without applying the altered payload or leaking other purchases. New authenticated event identities must correlate the expected request, amount/currency and purchase facts; mismatches are rejected. Unverified/malformed claims cannot mark a conflict solely by copying an identity. Buyer return-page claims cannot establish verified payment.

A stable provider-scoped logical event identity refers to immutable business evidence across delivery retries. Exact repeats return the recorded outcome without repeated effective transitions, quantity changes, notification intent, or business audit effect. A new authenticated event identity reporting the same already-applied correlated terminal outcome also returns that outcome/current Order progress before pending-only guards, retaining evidence without another effect. Reused identity with contradictory facts or authentic opposite terminal evidence requires reconciliation; preserve established effects and history. New transport attempts do not imply new business outcomes.

Timely success may apply only while the request is pending, all group Orders await payment, and all exact unpaid holds are active at a time strictly before their common deadline. Apply payment success, paid/awaiting-confirmation Shop orders, and paid protection of all group holds together or none. Payment success neither confirms a Shop order nor deducts physical stock. Each Seller later confirms their paid Shop order under BR-INV-001 and UC-013.

Verified failure or applicable unpaid expiry closes every payment-pending Shop order and releases every unpaid group hold once without deduction. No automatic retry/new purchase follows. Success is ineligible at/after the authoritative deadline even if an expiry worker has not run. Received/verified evidence without a committed business outcome does not freeze expiry; recovery rechecks current guards.

Buyer cancellation of an unpaid group is an additional owning transition under [BR-CANCEL-001](#br-cancel-001): close the pending Payment Request as `CANCELLED`, cancel every pending Shop order and release unpaid holds together. It cannot cancel only an unpaid sibling or masquerade as payment failure. Paid Shop cancellation/rejection/timeout leaves the original group application `SUCCEEDED` and amount unchanged; compensation is tracked separately.

Financial evidence and application to the purchase are distinct. Authentic failure/expiry after an unpaid Buyer cancellation can corroborate non-success without changing CANCELLED or repeating release; it is not an opposite local cancellation decision. A late success for failed/expired/cancelled Orders is retained with reconciliation required; it cannot resurrect Orders/holds, deduct newly unavailable stock, or claim a refund already happened. A later failure/expiry cannot undo successfully paid or subsequently Seller-confirmed Orders. If inconsistent/missing holds prevent application before expiry, require reconciliation while preserving established effects; remaining unpaid holds still follow their applicable expiry guards.

Reconciliation correlates provider facts, request application, Orders/holds and audit changes. Recovery may apply a valid missing effect once with current guards or return its completed outcome. Conflicts and late financial success need an authorized explicit disposition; no automatic blind overwrite, fresh charge/purchase, refund/reversal or inventory restoration is implied. Numeric retry stopping/retention and reconciliation authority/closure remain OQ-012/OQ-015 and OQ-005/009/010.

### Rationale, coverage, and limits

These business outcomes make asynchronous uncertainty visible without selecting callback fields, signature algorithms, storage or transport. All financial behavior uses the simulated provider.

- [UC-012](USE_CASES/UC-012-process-payment.md): AC-UC-012-01 through 24; [UC-007](USE_CASES/UC-007-manage-inventory.md): AC-UC-007-09 through 16; [UC-011](USE_CASES/UC-011-checkout.md): AC-UC-011-13.
- [SM-PAYMENT-001](STATE_MACHINES.md#sm-payment-001), [SM-ORDER-001](STATE_MACHINES.md#sm-order-001), [SM-RESERVATION-001](STATE_MACHINES.md#sm-reservation-001); NFR-001/002/003/004/006. Logistics event ordering is Proposed in [BR-SHIP-001](#br-ship-001); refund execution remains pending UC-017/OQ-010.

<a id="br-order-002"></a>
## BR-ORDER-002 — Seller confirmation, packing, and handover

- Status: Proposed; Baseline Seller confirmation/deduction and packing/handover capabilities are retained.
- Owner: Project owner.
- Scope/source: SC-013/012/007/022; [OQ-008](OPEN_DECISIONS.md#oq-008), [OQ-009](OPEN_DECISIONS.md#oq-009), OQ-004/005/006/010/015.

### Proposed rule

Only an authorized Shop Operator may confirm a `PAID_AWAITING_CONFIRMATION` Shop order with applied group payment `SUCCEEDED`, no blocking payment reconciliation, and all its exact matching `ACTIVE_PAID` holds. New confirmation must be accepted strictly before its recorded Seller-response deadline. The working candidate is **24 hours from the authoritative time group payment success was applied**, recorded per Shop order with its policy version; callback receipt, provider occurrence time, retry and later reads cannot start or extend it. The duration is unconfirmed under OQ-009.

Confirmation accepts the whole Shop order together: set `CONFIRMED`, consume all its paid holds, and decrease on_hand and reserved by the same quantities once. Available stock is unchanged; sibling Orders, holds, payment amount and fulfillment are unchanged. Missing/conflicting identities, quantities or holds require an observable consistency issue, without partial confirmation, speculative stock correction or SKU substitution. Payment success remains distinct from Seller confirmation.

An authorized Operator may pack all confirmed lines, using the immutable accepted purchase facts, moving `CONFIRMED` to `PACKED` once. Packing adds no stock deduction. The working shipment boundary is one whole Shipment per Shop order, without partial packing, multiple parcels or replacement Shipments; those alternatives require another policy review. Shipment creation belongs to BR-SHIP-001. Seller handover records an attributable intent/action on the associated awaiting-pickup Shipment; only permitted authenticated partner pickup evidence marks the Order `SHIPPED`. A handover repeat cannot create another Shipment or fabricate pickup.

Applied payment reconciliation `REQUIRED` blocks new confirmation/packing/creation/handover commands pending disposition. It does not erase already observed physical partner facts, reverse stock or automatically cancel an Order. Proposed closure under BR-CANCEL-001 may still relinquish paid holds with an explicit financial exception association. Later catalog/price/address changes do not rewrite the accepted snapshot; Accepted Product hiding still blocks new purchases. Existing-Order staff restrictions and SKU removal/reidentification remain OQ-004/OQ-006; do not infer an intervention bypass.

After authorization, resolve a known operation's original instructions/outcome before new-action state/time guards. An exact accepted repeat returns its original result and current progress without another deduction, transition, handover, notification intent or effective audit change. Conflicting reuse is refused. Unknown/interrupted operations recover to one complete outcome or none before reporting completion. Winning competing actions follow the current Order/deadline/hold guards; Accepted moderation OQ-003 is not authority for this new purchase policy.

### Coverage and limits

[UC-013](USE_CASES/UC-013-fulfill-shop-order.md), [UC-014](USE_CASES/UC-014-simulate-shipment.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md); SM-ORDER-001/RESERVATION-001/SHIPMENT-001; NFR-001/003/004/006. New fulfillment and closure criteria are Proposed; executable evidence, packing/handover service deadlines, detailed permissions and after-sales completion remain pending.

<a id="br-cancel-001"></a>
## BR-CANCEL-001 — Unpaid group cancellation and paid Shop closure

- Status: Proposed; no cancellation/refund acceptance is inferred from the scope or drafting request.
- Owner: Project owner.
- Scope/source: SC-007/012/013/020/026; [OQ-009](OPEN_DECISIONS.md#oq-009), OQ-007/008/010/011/015.

### Proposed eligibility and effects

1. **Unpaid Buyer cancellation:** the owning Buyer explicitly cancels the entire group while payment is `PENDING`, every Shop order is `PAYMENT_PENDING`, and all exact unpaid holds are active strictly before the unpaid deadline. Together mark payment application `CANCELLED`, all group Shop orders `CANCELLED` with unpaid-Buyer cause, and release every unpaid hold once (reserved decreases, available increases, on_hand unchanged). No refund obligation is inferred from unapplied/unknown payment; later verified financial success is retained for reconciliation, without restoring the purchase. Do not cancel one unpaid sibling independently. At/after unpaid deadline use UC-012 expiry rather than relabelling expiry as cancellation.
2. **Paid Buyer cancellation / Seller rejection:** while one Shop order is `PAID_AWAITING_CONFIRMATION`, applied group payment is `SUCCEEDED`, all its exact paid holds match, and authoritative acceptance is strictly before the Seller deadline, the owning Buyer may cancel that named Shop order or its authorized Seller may reject it with a reason. An optional Buyer reason is recorded if supplied; rejection requires a reason. One indivisible outcome records `CANCELLED` or `SELLER_REJECTED` with cause, releases all that Shop order's paid holds once, and establishes its complete compensation obligation below. No physical stock was deducted, so release does not increase on_hand. Siblings continue independently.
3. **Seller timeout:** at/after the recorded Seller deadline, an unconfirmed paid Shop order with matching paid holds is eligible only for the system's `SELLER_TIMED_OUT` closure, with the same hold/compensation effects. A delayed timer does not allow late confirmation, rejection or Buyer cancellation. Do not relabel a deadline-due close; return/normalize the due timeout through its guarded owning transition. Exact accepted repeats still return their prior outcomes. Original unpaid expiry has no effect on paid holds.
4. **After confirmation:** ordinary cancellation/rejection is refused for `CONFIRMED`, `PACKED`, `SHIPPED`, `DELIVERED` and delivery-exception Orders. Do not release consumed holds, deduct again or restore stock. A subsequent return/refund/dispute or staff intervention requires UC-016/017/022 policy under OQ-005/OQ-010. Tracking and views cannot confer such eligibility.

New confirmation, rejection, paid cancellation and timeout compete using current state, authoritative acceptance time and the full hold set. Before deadline a single eligible accepted action wins; conflicting later new actions are refused. At the deadline timeout alone is eligible. For an unpaid cancel versus success race, unpaid cancel first closes/releases with late-success reconciliation; success first makes the unpaid intent ineligible, requiring a new explicit paid-Shop cancellation intent. Never silently reinterpret intent or expose partial sibling outcomes.

### Compensation boundary

For each paid Shop closure, record one stable obligation associated with Order, closure, original paid request and immutable currency/Shop payable amount: item subtotal plus attributed shipping minus attributed discounts. The Proposed choice returns that whole snapshotted Shop payable, including its shipping, without repricing remaining siblings or redistributing discounts. A concrete positive-payable, no-voucher fixture can demonstrate this; zero-payable, currency/rounding and voucher-benefit/quota restoration remain OQ-002/007/011 and cannot be filled with fabricated amounts.

The obligation is `REQUIRED`, observable as awaiting resolution/execution in UC-017. It is neither a new charge nor proof of a completed Refund, and it does not turn the group Payment Request from `SUCCEEDED` into `FAILED`/`EXPIRED`/`REFUNDED`. Pending compensation does not keep the cancelled stock reserved in this Proposed choice. If the closure, complete obligation and all hold changes cannot be established together, none closes/releases; preserve the paid holds and expose recovery/consistency work. A payment reconciliation annotation alone does not forbid relinquishing a paid unconfirmed Order whose applied success and hold facts are intact: retain its exception alongside the obligation for authorized disposition, without blind financial execution.

Obligations for distinct cancelled Shop orders remain separate and cannot exceed the original attributed amounts/group paid total. Repeats and competing closure reasons never add a second entitlement for the same Shop order. Later UC-017 must correlate all obligations, adjustments and already executed compensation to cap total execution against verified paid funds, recover unknown results, and record explicit resolution evidence. Until those requirements exist, no executable refund or automatic `RESOLVED` transition is specified. Inventory receipt/restock is separately required for consumed stock and cannot follow from refund completion alone.

### Coverage and limits

[UC-013](USE_CASES/UC-013-fulfill-shop-order.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), linked [UC-007](USE_CASES/UC-007-manage-inventory.md)/[UC-012](USE_CASES/UC-012-process-payment.md); SM-ORDER-001/RESERVATION-001/PAYMENT-001; NFR-001/002/003/004/006. Detailed refund execution, exceptional disposition, permissions, voucher restoration and post-confirmation policy remain pending; this is a proposed closure contract only.

<a id="br-ship-001"></a>
## BR-SHIP-001 — Simulated Shipment identity and ordered progress

- Status: Proposed; Baseline simulated Shipment creation and duplicate/late/out-of-order tracking are retained.
- Owner: Project owner.
- Scope/source: SC-024/025/007/013/022; [OQ-015](OPEN_DECISIONS.md#oq-015), [OQ-010](OPEN_DECISIONS.md#oq-010), OQ-005/007/009/012.

### Proposed rule

An authorized Shop Operator creates one logical Shipment for one whole paid, confirmed and packed Shop order, using its accepted item/address/shipping facts. Resolve stable creation identity before contacting the simulated Partner. Unknown creation or a lost acknowledgement is recoverable under the same identity; it is not rejection, pickup or permission to create another Shipment. Authenticated correlated acknowledgement establishes `AWAITING_PICKUP` and the authoritative tracking-sequence baseline. The exact sequence representation/starting number, authentication scheme and partner fields remain later design/OQ-015. No inbound event can invent a local Shipment/Order or silently substitute its association.

Each partner event has stable provider-scoped identity and immutable Shipment/Order association, sequence and progress evidence across retries. Verify source and structure first. Compare an authenticated known identity to its original evidence before new-event correlation: exact repeats return the established processing result/current progress, changed evidence requires reconciliation of its original association without applying the altered payload or leaking another Order. New authenticated identities must correlate the established Shipment/Order and sequence baseline before effects. Unauthenticated claims cannot manufacture conflict cases by copying an ID.

Apply new events only in contiguous authoritative sequence and through the permitted SM-SHIPMENT-001 graph. Provider occurrence timestamps inform history; sorting arrival/occurrence timestamps or comparing progress ranks is not the acceptance policy. A sequence gap or missing prerequisite is retained/deferred without a false progress update. Recover through missing-event replay or an authorized authoritative query that establishes the complete missing permitted path/evidence, never an unsupported jump. Missing sequence/correlation information remains an observable exception for recovery, not guessed success. Late earlier facts may join history if consistent; same-sequence contradictions, invalid transitions and terminal conflicts require reconciliation without regression/overwrite. Distinct authenticated equivalent evidence may be retained without another business effect. If equivalent current-state evidence has the next contiguous authoritative sequence, record that sequence as processed without a physical/Order transition or another effective audit/notification effect, so subsequent legitimate events do not acquire a false gap. An old equivalent fact does not advance the cursor, and a replay never advances it twice. A real failed-delivery retry follows the graph rather than being suppressed because IN_TRANSIT appeared earlier.

Accepted pickup moves Shipment to `PICKED_UP` and the packed Order to `SHIPPED` together. In-transit and failed-delivery/retry/returning progress preserve Order `SHIPPED`, with the current Shipment exception visible. Delivered evidence moves Shipment/Order to `DELIVERED`; returned evidence moves Shipment to `RETURNED` and Order to `DELIVERY_EXCEPTION`. Delivered is distinct from completed; failed delivery or returned goods do not imply approved after-sales Return, cancellation, completed Refund or stock receipt. Ordinary callbacks cannot overwrite terminal `DELIVERED`/`RETURNED`; corrections require explicit authorized disposition under OQ-010/OQ-015.

Shipment, Order synchronization and effective audit/notification intent are one permitted logical outcome. Recovery resolves committed progress first; receipt alone is not applied progress. Repeats or new event identities representing an already-applied fact do not repeat effects. If Order/Shipment association or progress disagrees, expose reconciliation without partially publishing a new outcome. Payment exceptions block new Seller commands under BR-ORDER-002 but do not erase physical facts for an existing Shipment; valid logistics progress may still be recorded with its linked financial exception. No callback changes payment amount, deducts stock again or executes compensation.

### Coverage and limits

[UC-014](USE_CASES/UC-014-simulate-shipment.md), [UC-013](USE_CASES/UC-013-fulfill-shop-order.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md); SM-SHIPMENT-001/ORDER-001; NFR-001/002/004/006. Retry/query authority, gap-recovery stopping, retention, numeric recovery targets, cancellation before pickup after confirmation, replacement/multiple parcels and terminal corrections remain unconfirmed. This proposal excludes unsafe automatic shortcuts rather than deciding those future capabilities.

<a id="br-aftersales-001"></a>
## BR-AFTERSALES-001 — Delivery completion, dispute intake, and Seller response

- Status: Proposed; after-sales windows, grounds, and auto-completion timing are reviewable proposals under OQ-010.
- Owner: Project owner.
- Scope/source: SC-007/008/020; [OQ-010](OPEN_DECISIONS.md#oq-010), OQ-005/007/009/015.

### Proposed rules

1. **Delivery vs. Completion:**
   - Physical arrival of goods establishes `DELIVERED` status via authenticated Logistics Partner evidence ([BR-SHIP-001](#br-ship-001)).
   - An Order transitions from `DELIVERED` to `COMPLETED` either by:
     a) **Explicit Buyer confirmation:** Buyer acknowledges receipt while in `DELIVERED` state with no active dispute.
     b) **System auto-completion timeout:** If Buyer takes no action for **7 days** (candidate auto-completion window under OQ-010) following recorded `DELIVERED` timestamp, and no active dispute exists, system automatically marks the Order `COMPLETED`.
   - Once an Order reaches `COMPLETED`, the regular after-sales return window closes; post-completion recourse requires exceptional Internal Staff intervention under SC-020/OQ-004.

2. **Dispute intake eligibility and windows:**
   - Buyer may open an after-sales dispute case only for an eligible Shop order owned by that Buyer.
   - Eligibility grounds:
     - `RETURN_AND_REFUND`: Order is `DELIVERED` and within the 7-day after-sales window (e.g., damaged item, defective, wrong product, incomplete parcel). Physical return of items is expected.
     - `REFUND_ONLY`: Permitted when goods were not received (Order is `DELIVERY_EXCEPTION` / `RETURNED` courier failure, or `SHIPPED` past carrier lost-parcel threshold), or item is unusable/hazardous and return shipping is unfeasible.
   - Concurrency guard: Exactly **one active dispute case** is permitted per Shop order. A second dispute cannot be opened while an active case is pending, under review, or awaiting return receipt.
   - Maximum claim amount: Requested refund amount cannot exceed the snapshotted Shop payable amount (item subtotal + attributed shipping - attributed discounts).

3. **Dispute freeze on order completion:**
   - Filing an eligible dispute immediately **suspends the auto-completion timer** on that Shop order. The Order cannot transition to `COMPLETED` while a dispute is open.
   - If Buyer withdraws the dispute, the remaining auto-completion window resumes or a minimum grace period (candidate 24 hours) applies.

4. **Seller response deadline:**
   - Once a dispute is submitted, Seller is given **48 hours** (candidate response window under OQ-010) to respond:
     a) **Accept:** Seller agrees to Buyer's request (`REFUND_ONLY` or accepts item return).
     b) **Reject / Dispute:** Seller disagrees (e.g. asserts item sent was intact, evidence inadequate). The case is escalated to Internal Staff (`Support`) for binding adjudication.
     c) **Timeout:** If Seller does not respond within 48 hours, system auto-accepts Buyer's request on Seller's behalf.

### Coverage

[UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-016](USE_CASES/UC-016-request-return-refund.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); SM-ORDER-001/DISPUTE-001; NFR-001/004.

<a id="br-refund-001"></a>
## BR-REFUND-001 — Financial refund caps, allocation, and simulated execution

- Status: Proposed; financial caps, compensation execution, and exception reconciliation are reviewable proposals under OQ-009/010/015.
- Owner: Project owner.
- Scope/source: SC-006/007/020/026; [OQ-009](OPEN_DECISIONS.md#oq-009), [OQ-010](OPEN_DECISIONS.md#oq-010), [OQ-015](OPEN_DECISIONS.md#oq-015).

### Proposed rules

1. **Cumulative refund cap by verified paid funds:**
   - The total sum of all executed refunds (pre-confirmation cancellation compensation, after-sales dispute refunds, and payment exception resolutions) across all Shop orders for a purchase group **can never exceed the verified successful paid amount received from the Payment Provider** for that group.
   - If payment application was `CANCELLED` or `EXPIRED` but late funds arrived with reconciliation `REQUIRED`, refund of those excess funds cannot exceed the authentic late paid amount.

2. **Per-Shop allocation and liability:**
   - Refunds and compensation obligations are accounted strictly per Shop order based on the accepted purchase snapshot.
   - A full refund of a Shop order returns its exact accepted payable amount (item subtotal + attributed shipping - attributed discounts).
   - In a partial refund (e.g. agreed damage allowance or partial item return), the refund amount is deducted from the Shop's eligible payable amount; sibling Shop orders retain their allocations intact.
   - Platform promotions/vouchers are not recomputed dynamically; refund covers only Buyer's actual out-of-pocket payable amount unless explicit platform voucher restoration policy applies under OQ-011.

3. **Idempotent simulated refund execution:**
   - Every refund execution request must have a stable, unique logical refund identity (`REFUND-###`) tied to the underlying dispute case or compensation obligation.
   - The simulated Payment Provider must handle repeated requests with the same refund ID idempotently, returning the established result without duplicate payouts.
   - A refund is terminal once provider confirms `REFUND_SUCCEEDED`. On `REFUND_FAILED`, the obligation remains pending for retry or Support intervention.

4. **Payment exception and reconciliation closure:**
   - When payment reconciliation is `REQUIRED` (e.g. late payment arrived after order cancelled or expired), authorized Support staff may execute a simulated refund of the unapplied funds.
   - Successful refund execution transitions the payment reconciliation status from `REQUIRED` → `RESOLVED`, leaving audit evidence of the resolution.

### Coverage

[UC-012](USE_CASES/UC-012-process-payment.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); SM-PAYMENT-001/REFUND-001; NFR-002/004/006.

<a id="br-stock-receipt-001"></a>
## BR-STOCK-RECEIPT-001 — Physical return inspection and inventory restock

- Status: Proposed; restock condition and physical receipt evidence are reviewable proposals under OQ-010.
- Owner: Project owner.
- Scope/source: SC-012/020; [OQ-010](OPEN_DECISIONS.md#oq-010), [BR-INV-001](#br-inv-001).

### Proposed rules

1. **No automatic restock upon refund approval or delivery return:**
   - Consumed physical inventory is **never restocked automatically** upon dispute submission, refund approval, or carrier return event (`RETURNED` in BR-SHIP-001).
   - Once inventory is consumed (`on_hand` deducted on Seller confirmation), replenishment requires explicit physical receipt verification.

2. **Seller inspection and condition verification:**
   - For `RETURN_AND_REFUND` disputes, goods returned to Seller must undergo manual inspection upon physical delivery.
   - **Usable / Resellable condition:** If Seller (or Support staff) verifies that the returned items are intact, undamaged, and resale-eligible, Seller records an authorized stock-return movement. This increases `on_hand` and `available` by the verified intact quantity.
   - **Damaged / Defective / Unusable condition:** If goods are broken, expired, or counterfeit, they are scrapped/written off. No inventory adjustment occurs (`on_hand` remains unchanged).
   - Incomplete parcel: If Buyer returns fewer items than claimed, restock is permitted only for the exact quantity received intact.

3. **Failed-delivery return inspection:**
   - When a courier returns an undelivered parcel to Seller (`DELIVERY_EXCEPTION`), Seller inspects the parcel before restocking. Intact goods are restocked via authorized stock-in; damaged goods during transit are flagged for carrier insurance/dispute without inflating available stock.

### Coverage

[UC-007](USE_CASES/UC-007-manage-inventory.md), [UC-014](USE_CASES/UC-014-simulate-shipment.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); SM-INVENTORY-001; NFR-003/004.

## Rules not yet specified

Concrete shipping fee calculations, detailed multi-level voucher subsidy redistribution (OQ-011), and staff disciplinary sanctions against abusive buyers or sellers remain incomplete. Proposals above establish baseline after-sales and financial closure contracts.

