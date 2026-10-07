# Open Decisions and Decision Provenance

- Status: Draft register.
- Owner: Project owner.
- Updated: 2026-10-07.

The existing [moderation rule](BUSINESS_RULES.md#br-prod-001) is a **Baseline**: automatic validation precedes Moderator review; approval activates a Product; rejection requires a reason; risky content requires re-review; operational SKU changes do not. Its original sign-off provenance is not present. Normalization preserves that content without inventing approval history.

**Accepted** entries require recorded explicit owner confirmation or an applicable explicit delegation of decision authority, with the chosen policy and affected artifacts identified. General authorization to prepare documentation is not decision acceptance. Proposed defaults below are discussion options, not executable product policy. OQ-001, OQ-003, OQ-016, OQ-017, and OQ-018 are Resolved; the remaining entries are Open.

<a id="oq-001"></a>
## OQ-001 — Active Product visibility during re-review

- Status: Accepted; decision state: Resolved.
- Question: can the previously approved content remain purchasable while risky edits await review, or must the entire Product be hidden?
- Resolution: hide the entire Product from Buyer-facing sale while revised high-risk content awaits manual review. The previous approved content does not remain on sale during that review. Approval makes the revised content eligible for publication, subject to the normal purchasing guards.
- Confirmation: 2026-10-07, Project owner replied `2` to the discussion choices: (1) keep approved content on sale, (2) hide the Product during re-review.
- Affects: UC-004/005/006/008/009/011, [BR-PROD-002](BUSINESS_RULES.md#br-prod-002), SM-PRODUCT-001, glossary, and traceability.
- Boundary: this confirmation covers pending-review visibility. Rejection/recovery was separately accepted under [OQ-016](#oq-016). Initial hide timing was separately accepted under [OQ-017](#oq-017): hide on successful saving of risky changes. Other moderation policy, priorities, and state-code proposals are not accepted by this answer.

<a id="oq-002"></a>
## OQ-002 — Deterministic validation boundaries

- Status: Proposed for the draft/submission boundary below; exact field limits and manual-review criteria remain Draft; decision state: Open.
- Question: which fields/attributes are required per category; what prices, precision/currency, inventory units, media formats/sizes, category validity, and forbidden-word rules are allowed? Also define manual moderation criteria and actionable rejection-reason policy.
- Existing facts: invalid price, negative inventory, invalid category, unsupported image format, missing fields, and forbidden words block submission.
- Proposed next discussion — draft versus submission:
  - Draft saving may retain incomplete listing content, including missing category, description, media, or category-required attributes. Supplied values must satisfy their applicable format, reference, and range rules; a missing value and an invalid supplied value are distinct findings. Incomplete listing content never becomes eligible for manual review or sale merely by saving.
  - An invalid supplied value rejects the attempted save without applying any of that attempt's content changes. A failed save does not trigger new review-related hiding; a successful actual risky change still triggers Accepted BR-PROD-002, even if the draft is incomplete.
  - Submission requires a nonempty Product name and description, a valid category permitted for listing, at least one supported Product image, all category-required attributes, and at least one enabled SKU with a valid price and non-negative inventory. Zero inventory is permitted for review but does not permit purchase without stock. SKU structure remains OQ-006; stock adjustments remain UC-007/OQ-008.
  - Submission rechecks all applicable rules against the exact content being admitted for review. Validation findings identify the field or content item, the unmet rule, and the correction required. Invalid content does not enter manual review; passing checks does not constitute approval.
  - Operational changes validate the changed values and relevant invariants before taking effect; they do not require every unfinished risky-content field to be complete. Such changes cannot restore sale while BR-PROD-002 applies.
- Remaining boundaries: exact required category schemas, currency/precision and price limits, text/media limits, inventory units, category eligibility, forbidden-word matching, and manual-review criteria/reason categories. The proposal above does not resolve these or mark the whole decision Accepted.
- Affects: UC-003/004/005/006/007/008/009/010/011/012, BR-PROD-001/004 and BR-INV-001/ORDER-001/PAY-001. Concrete quantity/currency boundary data and manual-review scenarios cannot be finalized before these definitions are settled.

<a id="oq-003"></a>
## OQ-003 — Editing, withdrawal, and concurrent moderation

- Status: Accepted; decision state: Resolved.
- Question: how are repeated requests and competing Moderator decisions handled, including a withdrawal racing with a review decision? Seller withdrawal/edit/resubmit behavior is Accepted under [OQ-018](#oq-018).
- Resolution: each review submission identifies one exact, immutable set of submitted content while pending. Among withdrawal, approval, and rejection competing for the same pending submission, the first action accepted by the system wins and makes later conflicting actions invalid. A successful withdrawal makes the submission ineligible; a successful approval/rejection makes it no longer withdrawable. Repeating the same action returns the existing outcome without another state change, review result, notification, publication, or audit effect. Repeated submission of unchanged already-pending content returns the existing pending submission rather than creating another review item. A revised resubmission after withdrawal is a new eligible submission and cannot inherit the prior submission's validation or decision.
- Confirmation: 2026-10-07, Project owner delegated planning and decision authority for the Seller use case, asking for a logical and reasonable decision. The chosen policy is first accepted action wins with idempotent repeats.
- Rationale: deterministic and neutral under concurrency; prevents stale approval, duplicate moderation work, and repeated side effects without favoring Seller or Moderator.
- Affects: UC-004/005/006, SM-PRODUCT-001 and audit acceptance criteria.

<a id="oq-004"></a>
## OQ-004 — Moderation interventions and recovery

- Status: Draft; decision state: Open.
- Question: how do revision-requested, hidden, locked, restricted, and suspended outcomes differ for Products, Shops, and Users; who applies/reverses them; what happens to existing orders?
- Affects: UC-002/006/021/022, SC-016/SC-018, future Shop/Product/account lifecycle models.
- Do not infer Shop states from the Product approval rule.

<a id="oq-005"></a>
## OQ-005 — Shop and staff permissions

- Status: Draft; decision state: Open.
- Question: define Shop registration approval, account verification, staff account type, ownership versus operator permissions, and the Admin/Moderator/Support/Operations action matrix.
- Existing facts: a User may buy and operate Shops; granular staff permissions are required; Moderator is the default Product reviewer, not Admin.
- Purchase dependency: AC-UC-007-04, AC-UC-011-02 and AC-UC-012-02 define Proposed wrong-Shop/wrong-Buyer denial scenarios. Define Buyer cart/address/group/payment access, Shop inventory/confirmation/packing/handover rights, Buyer tracking/cancellation, simulator/query authority and each Shop's permitted Order data without sibling-Shop/private group access. Negative criteria do not settle the allow matrix or staff reconciliation authority.
- Affects: UC-001/002/003/004/005/006/007/008/009/010/011/012/013/017/020/021/022, NFR-001/004 and purchase audit/reconciliation access.

<a id="oq-006"></a>
## OQ-006 — Variant and SKU identity

- Status: Draft; decision state: Open.
- Question: define variant combinations, SKU cardinality/uniqueness, non-variant Products, and whether changing SKU attributes creates a new identity.
- Existing facts: price and inventory are at SKU level; adding/changing variants requires re-review.
- Purchase dependency: accepted snapshots/holds must retain the exact identity or report an explicit conflict; a variant/internal-code change cannot silently migrate a hold/Order to another SKU. Permitted SKU removal/reidentification with active holds or existing Orders remains undecided.
- Affects: UC-004/007/008/009/010/011/012/013, glossary Product/Variant/SKU terms and Proposed purchase snapshot/reservation models.

<a id="oq-007"></a>
## OQ-007 — Multi-shop checkout and payment grouping

- Status: Proposed; decision state: Open; owner: Project owner. Options prepared on 2026-10-07 for WI-002; confirmation pending.
- Question: one Payment Request for the checkout or one per Shop order; all-or-nothing or partial checkout; what totals and buyer-visible results follow partial failure?
- Also define the price/product/address/shipping snapshot captured for a purchase and its capture time.
- Existing fact: checkout produces separate orders per Shop.
- Options:
  1. **Recommended for this draft:** accept all selected items together, one purchase group and one group Payment Request, with separate Shop orders. A selected-item failure accepts none; Buyer can explicitly change the selection and retry. This provides one clear payment result while preserving per-Shop fulfillment.
  2. Accept each Shop independently with separate payment requests. Buyer can proceed with valid Shops, but needs explicit per-Shop failures, payment retries, and multiple payment outcomes.
  3. Accept the entire selected set together but use separate Shop payments. This exposes mixed paid/unpaid states and requires a compensation/cancellation policy before implementation.
- Proposed working choice: option 1 in [BR-ORDER-001](BUSINESS_RULES.md#br-order-001) and [UC-011](USE_CASES/UC-011-checkout.md). Selection is explicit; unselected cart entries do not join the attempt. Quote presentation creates no hold. Changes to accepted quantities, prices, address, shipping, or discount facts before commit require refreshed presentation and Buyer acceptance.
- Proposed snapshot boundary: atomically with checkout acceptance and stock reservation, capture exact Shop/Product/SKU identities, approved descriptions/variant choices, quantities/unit prices, item subtotals, attributable shipping/discounts, per-Shop payable amounts, group total/currency, delivery address, shipping choice, and payment deadline. Later edits do not silently rewrite these facts. The group amount equals the sum of Shop payable amounts; concrete currency/precision, shipping methods/fees/quote validity and discounts remain dependent on OQ-002/OQ-011 and the remaining choices below.
- Proposed checkout-versus-edit ordering: if successful risky saving/hiding occurs first, new checkout cannot be accepted under Accepted BR-PROD-002. If checkout is accepted first, its approved snapshot and holds remain associated with those existing Orders; later review-related hiding blocks further purchases without automatically cancelling/refunding the accepted Orders. This is a new Proposed purchase ordering, not an extension of moderation-only OQ-003. Staff restrictions remain OQ-004.
- Proposed repeat/cart boundary: one Buyer operation identity refers to one accepted fact set; identical repeats return the existing outcome without additional Orders, holds, payment, or deadline extension; conflicting reuse is refused. Keep source cart intentions during checkout/payment; later cart edits do not change the purchase. A deliberate new purchase uses a distinct identity and fresh checks.
- Remaining choices: confirm option/atomicity and snapshot/ordering policy; define supported shipping choices, destination/service eligibility, fee calculation and quote validity; settle currency/rounding under OQ-002, zero-payable purchase handling, and voucher allocation/quota under OQ-011. A no-voucher simulated-shipping walkthrough is a conditional subset, not full SC-005 readiness.
- Affects: UC-007/010/011/012/013/015/017; BR-ORDER-001/INV-001/PAY-001; SM-ORDER-001/PAYMENT-001/RESERVATION-001; AC-UC-011-01 through 16 and dependent inventory/payment ACs. Domain/design/tests remain pending.

<a id="oq-008"></a>
## OQ-008 — Inventory timing and reservation invariants

- Status: Proposed; decision state: Open; owner: Project owner. Options prepared on 2026-10-07 for WI-002; confirmation pending.
- Question: what exactly constitutes order confirmation; may it occur before payment; what reservation lifetime, stock adjustment bounds, and release/restock rules apply; what happens to success received after expiry?
- Existing facts: reserve at checkout, release on payment failure/expiry, deduct when an order is confirmed. Do not replace confirmation with payment success without a recorded decision.
- Baseline terminology: confirmation means Seller acceptance for fulfillment (SC-013 and glossary). Payment success alone is not that action.
- Options:
  1. **Recommended for this draft:** Seller may confirm only after verified payment success. Until then hold stock against a common unpaid deadline. Timely success protects the holds as paid reservations; Seller confirmation of each Shop order subsequently consumes its holds and deducts its stock.
  2. Allow Seller confirmation before payment. This deducts stock before a final payment result and needs explicit restoration and expiry policy for confirmed/unpaid Orders.
  3. Redefine confirmation as automatic system acceptance at payment success, with a later separate Seller action. This changes baseline terminology and SC-013, requiring an explicit scope/requirements revision.
- Proposed working choice: option 1; [BR-INV-001](BUSINESS_RULES.md#br-inv-001) defines `on_hand >= reserved >= 0` and `available = on_hand - reserved`, where reserved includes all effective unpaid and paid holds. Seller stock-in/adjustments cannot reduce on_hand below reserved or mutate holds. Exact units, permitted bounds/reasons, and SKU identity remain OQ-002/OQ-005/OQ-006.
- Proposed unpaid lifetime: a common system deadline for group payment/holds, initially **15 minutes from checkout acceptance** as a reviewable candidate, not an accepted target. Timely verified success must take effect strictly before that deadline; reaching the deadline prevents payment-to-paid-hold conversion even if expiry processing has not yet run. Hold quantities remain reserved until an explicit release/consume transition, avoiding time-based double release.
- Proposed effects: failure/expiry releases unpaid holds once, without physical deduction. Timely success changes all group holds to paid protection, without deduction. Seller confirmation consumes only that Shop order's paid holds, reducing on_hand and reserved by the same quantity. Original unpaid expiry cannot release a paid hold. Later success after expiry is reconciliation-required and cannot restore Orders/holds or sell newly unavailable stock.
- Remaining choices: confirm invariant/timing and the unpaid duration; define Seller response deadline, maximum paid-hold lifetime, abandonment/rejection/cancellation, refunds and possible restock under OQ-009/OQ-010. WI-004 now prepares a Proposed paid-hold timeout with explicit Order closure and stable compensation obligation in [OQ-009](#oq-009)/BR-CANCEL-001, rather than an independent quantity timer. These choices remain unconfirmed and block commitment to the full paid-to-fulfillment path, not Draft preparation.
- Affects: UC-007/008/009/010/011/012/013/015/017; BR-INV-001/ORDER-001/PAY-001; SM-INVENTORY-001/RESERVATION-001/ORDER-001/PAYMENT-001; NFR-003 and inventory/checkout/payment ACs.

<a id="oq-009"></a>
## OQ-009 — Cancellation, Seller response, and paid Shop closure

- Status: Proposed; decision state: Open; owner: Project owner. WI-004 review package prepared 2026-10-07; confirmation pending.
- Question: which actors may cancel/reject at which states; what Seller/paid-hold deadline applies; when may paid stock be released; what happens to paid amounts, shipping/discount attribution and sibling Shop orders?
- Existing facts: payment success is distinct from Seller confirmation; Seller confirmation deducts stock. OQ-007/OQ-008 propose group payment and protected paid holds; neither accepts cancellation/refund policy.
- Options:
  1. **Recommended for this draft:** Buyer may explicitly cancel a whole unpaid group before expiry. After paid success, each Shop is independent: Buyer cancellation or reasoned Seller rejection is allowed only before Seller confirmation and its response deadline. At/after deadline the system closes an unanswered paid Shop order as Seller timeout. Each paid close atomically releases that Shop's active holds and establishes its compensation obligation; refund execution may follow separately in UC-017.
  2. Permit Buyer cancellation until partner pickup, including confirmed/packed Orders. This requires explicit restoration of already deducted stock, packing/Shipment cancellation and pickup-race evidence before acceptance; it cannot simply release consumed holds.
  3. Treat paid cancellation as a request pending Seller/staff approval and keep the paid holds until disposition. This requires request states, decision authority, appeal/timeout and an upper hold limit, rather than automatic closure on request.
- Proposed working choice: option 1 in [BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001), [BR-ORDER-002](BUSINESS_RULES.md#br-order-002), [UC-013](USE_CASES/UC-013-fulfill-shop-order.md) and [UC-015](USE_CASES/UC-015-track-cancel-orders.md). Ordinary post-confirmation cancellation/rejection is refused; further recourse requires UC-016/017/022 policy, not silent stock restoration.
- Proposed response candidate: **24 hours after payment success was applied**, recorded per Shop order; not from callback receipt/provider time, and never extended by retry/view. New confirmation/rejection/paid cancellation requires acceptance strictly before this boundary. At/after it only owning Seller-timeout closure is eligible even when its worker runs late. Exact accepted repeats still return the original outcome/current progress before new-action guards. Duration, configuration/version and any packing/handover deadline need confirmation; 24 hours is not an accepted target.
- Proposed unpaid/payment-race consequence: one pending group closes entirely as payment application CANCELLED, all unpaid Shop orders CANCELLED and all holds released together. Success-first makes that unpaid-cancel intent ineligible and requires a fresh explicit paid-Shop cancellation choice; cancel-first makes authentic late success a financial reconciliation case without resurrecting Orders. At unpaid expiry use expiry, not cancellation relabelling.
- Proposed paid accounting consequence: one Shop close sets CANCELLED, SELLER_REJECTED or SELLER_TIMED_OUT, releases all its ACTIVE_PAID holds without changing on_hand, and records one stable REQUIRED compensation obligation for its original Shop payable/currency (items + attributed shipping − attributed discounts). Return the whole snapshotted Shop payable, including its shipping; no sibling discount reallocation, repricing or group payment reversal is implied. Group payment stays SUCCEEDED and siblings retain their own states/holds. Repeats/competing reasons add no second entitlement. If complete obligation/Order/hold effects cannot be established together, none closes/releases. Pending compensation does not retain stock in option 1.
- Proposed financial exception boundary: reconciliation REQUIRED blocks new fulfillment but does not erase a coherent applied payment/hold fact or prohibit an eligible pre-confirmation close. Preserve/correlate the obligation and exception for authorized UC-017 disposition. Do not claim completed refund, automatically execute disputed compensation or cap against unverified amounts. UC-017 must later prevent double or excessive total execution across obligations and prior compensation, capped by verified paid funds.
- Remaining choices: confirm actor/state/cutoff/race policy, duration and stock-versus-compensation timing; confirm shipping refund allocation; define closure reasons/permissions, compensation execution/authority/recovery and exception disposition under OQ-005/010/015; settle voucher/quota restoration OQ-011 and zero-payable/money precision OQ-002/007. No blanket Seller authority from moderation-only OQ-003 is applied to this journey.
- Affects: UC-007/011/012/013/015/017/022; BR-INV-001/ORDER-001/ORDER-002/PAY-001/CANCEL-001; SM-INVENTORY-001/RESERVATION-001/ORDER-001/PAYMENT-001; NFR-001/002/003/004/006 and linked acceptance criteria. Refund execution and consumed-stock restock remain pending, not assumed complete.

<a id="oq-010"></a>
## OQ-010 — Delivery completion, after-sales, and financial disposition

- Status: Proposed; completion timing, dispute intake, return restock, and refund execution are reviewable proposals prepared on 2026-10-07 for WI-005; confirmation pending.
- Owner: Project owner.
- Question: distinguish delivered from completed; define failed delivery, returns, refund/dispute eligibility and windows, evidence, staff authority, partial/full refunds, inventory receipt, closure, and appeal.
- Shipment and completion options:
  1. **Recommended for this draft:** one whole Shipment per paid confirmed/packed Shop order. Authenticated delivery evidence sets `DELIVERED`. An Order moves to `COMPLETED` on explicit Buyer confirmation or when the **7-day auto-completion window** elapses with no active dispute. Opening an eligible dispute freezes auto-completion.
  2. Immediate automatic completion on delivery callback. This disallows a standard buyer confirmation window and forces all disputes into post-completion exceptions.
  3. Indefinite pending completion until Buyer explicitly clicks. This leaves Seller escrow/settlement unfinalized if Buyer abandons the order.
- Dispute and resolution options:
  1. **Recommended for this draft:** Buyer may request `RETURN_AND_REFUND` (within 7 days of delivery) or `REFUND_ONLY` (non-delivery or unusable goods), capped at the Shop payable snapshot. Seller has **48 hours** to respond; timeout auto-accepts. Seller rejection escalates to Support staff for binding adjudication (Full, Partial, or Reject).
  2. Direct platform staff adjudication for all disputes without seller involvement.
- Restock options:
  1. **Recommended for this draft:** consumed inventory is **never restocked automatically**. For returned goods, Seller physical inspection is required: verified intact items are restocked to `on_hand` and `available`; damaged/scrap items leave inventory unchanged ([BR-STOCK-RECEIPT-001](BUSINESS_RULES.md#br-stock-receipt-001)).
  2. Automatic restock upon carrier return scan. This risks inventory inflation from damaged, defective, or empty returned parcels.
- Financial refund caps and exception options:
  1. **Recommended for this draft:** cumulative refunds across all Shop orders and exceptions for a purchase group are **strictly capped by verified successful paid funds** ([BR-REFUND-001](BUSINESS_RULES.md#br-refund-001)). Each refund has a stable logical ID (`REFUND-###`). Unapplied late payments with reconciliation `REQUIRED` are resolved by authorized Support refund, transitioning reconciliation to `RESOLVED`.
- Proposed working choices: options 1 across all four areas in [BR-AFTERSALES-001](BUSINESS_RULES.md#br-aftersales-001), [BR-REFUND-001](BUSINESS_RULES.md#br-refund-001), [BR-STOCK-RECEIPT-001](BUSINESS_RULES.md#br-stock-receipt-001), [SM-ORDER-001](STATE_MACHINES.md#sm-order-001), [SM-DISPUTE-001](STATE_MACHINES.md#sm-dispute-001), [SM-REFUND-001](STATE_MACHINES.md#sm-refund-001), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-016](USE_CASES/UC-016-request-return-refund.md), and [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md).
- Remaining choices: confirm window durations (7-day completion, 48-hour seller response, 5-day return shipping); staff hierarchy and appeal levels; return shipping label fee attribution; voucher quota restoration on partial refunds (OQ-011).
- Affects: UC-007/012/013/014/015/016/017/022/023; BR-ORDER-002/CANCEL-001/SHIP-001/AFTERSALES-001/REFUND-001/STOCK-RECEIPT-001; SM-ORDER-001/SHIPMENT-001/DISPUTE-001/REFUND-001/INVENTORY-001.

<a id="oq-011"></a>
## OQ-011 — Voucher and promotion semantics

- Status: Draft; decision state: Open.
- Question: define funding/stacking, eligibility, minimum spend, quota reservation/consumption, validity, shipping discounts, and cancellation/refund treatment across Shop orders.
- Affects: UC-009/011/019/020 and glossary Voucher/Promotion definitions.

<a id="oq-012"></a>
## OQ-012 — Demo quality targets

- Status: Proposed; decision state: Open.
- Question: confirm the demo workload, latency threshold, recovery/retry targets, audit scope/retention, and isolation/security verification expectations.
- Proposed initial workload/latency and retry/recovery experiments are in [NFRs](NON_FUNCTIONAL_REQUIREMENTS.md).
- Affects: NFR-001 through NFR-006 and subsequent test strategy. These are learning-demo targets, not production service commitments.

<a id="oq-013"></a>
## OQ-013 — Review eligibility and ratings

- Status: Draft; decision state: Open.
- Question: when may a Buyer review, which purchase/item is eligible, what editing/moderation is allowed, and how are Product/Shop ratings calculated?
- Affects: UC-008/009/018.

<a id="oq-014"></a>
## OQ-014 — Baseline and MVP confirmation

- Status: Proposed; decision state: Open.
- Question: confirm the Phase 1 baseline and the P0/P1 priorities in the catalog before the Phase 2 exit gate is passed.
- Initial owner instruction authorizes documentation review and starting Phase 2; it is not evidence that newly drafted policies or MVP priorities have been accepted.
- Affects: entire catalog and Phase 2 completion status. Preparing drafts can continue while this remains open.

<a id="oq-015"></a>
## OQ-015 — Partner event identity and reconciliation

- Status: Proposed for payment and the bounded logistics policy below; decision state: Open; owner: Project owner. Options prepared on 2026-10-07 for WI-002/WI-004; confirmation pending.
- Question: identify the same logical partner event across retries; distinguish duplicates from conflicting outcomes; define permitted late/out-of-order updates, correlation, retry stopping, and reconciliation behavior.
- Existing facts: verified payment callbacks require idempotency/retry/reconciliation; logistics must tolerate duplicate, late, and out-of-order tracking events.
- Payment identity options:
  1. **Recommended for this draft:** a stable provider-scoped logical event identity across retries plus Payment Request/group correlation. Compare verified outcome, amount/currency, and relevant fact content; transport-attempt identifiers do not define a new payment result.
  2. Treat Payment Request identity plus final status as the logical event. Simpler, but less precise when the provider has several distinct events, corrections, or conflicting content.
- Proposed working choice: option 1 in [BR-PAY-001](BUSINESS_RULES.md#br-pay-001) and [UC-012](USE_CASES/UC-012-process-payment.md). Verify signature/source and correlate the expected request, amount, and currency before business effects. Identical repeats return the prior result; conflicting reuse or opposite terminal evidence is recorded for reconciliation without overwriting completed effects.
- Proposed timing/recovery: use the authoritative time when the system accepts the business effect, not a provider timestamp or merely received/verified callback. A late success cannot resurrect expired/failed Orders or holds; retain verified financial evidence separately from the fulfillment/payment-application outcome. Unknown initiation/processing results recover using the original request/event identities before any new request. Interruption after receiving a verified event rechecks expiry and holds before applying effects; receipt does not extend reservations.
- Proposed reconciliation boundary: inspect correlated provider evidence, Payment Request outcome, Shop orders, stock effects, and audit history. Consistent evidence may replay an originally valid missing effect once with all current guards satisfied. A late or contradictory payment remains explicitly unresolved until an authorized recorded disposition; no automatic new purchase, blind terminal overwrite, stock restoration, or refund is implied. Refund/reversal and staff decision authority remain OQ-005/OQ-009/OQ-010.
- Logistics ordering options:
  1. **Recommended for this draft:** provider-scoped immutable logical event identity, exact Shipment/Order correlation and authoritative sequence per Shipment established by creation acknowledgement. Apply contiguous sequence through the permitted graph; retain gaps/missing prerequisites for replay or authorized authoritative query with the complete missing permitted path. Late consistent evidence joins history without regression; contradictions/invalid terminal changes require reconciliation.
  2. Use only allowed progress transitions and occurrence times without authoritative sequence. Simpler simulator protocol, but retries/failed-delivery loops/corrections require additional causality evidence; chronological arrival/occurrence sorting cannot establish a safe outcome alone.
- Proposed logistics working choice: option 1 in [BR-SHIP-001](BUSINESS_RULES.md#br-ship-001)/[UC-014](USE_CASES/UC-014-simulate-shipment.md). Verify source/structure before conflict classification; compare authenticated known IDs to original evidence before new-event correlation. Exact/equivalent repeats return prior processing/current progress without another effect. Never create a Shipment from unknown callback, guess the sequence baseline, fast-forward a missing path or overwrite ordinary DELIVERED/RETURNED. Receipt-only interruptions recover current progress; payment exceptions do not erase established physical partner facts.
- Remaining choices: confirm logical identity/conflict/late-event policy; define permitted authoritative provider queries, retry stopping, idempotency retention, reconciliation triggers/authority and terminal exception closure. Numeric retry/recovery targets stay OQ-012. Logistics sequencing/query authority, gap stopping/retention and terminal corrections also remain unconfirmed; the bounded WI-004 proposal does not decide correction/refund authority or protocol fields/signature algorithms.
- Affects: UC-007/011/012/013/014/015/017, BR-PAY-001, SM-PAYMENT-001/ORDER-001/RESERVATION-001, NFR-002/NFR-006 and glossary integration terms.

<a id="oq-016"></a>
## OQ-016 — Product recovery after re-review rejection

- Status: Accepted; decision state: Resolved.
- Question: after revised content of a previously active Product is rejected, does the Product remain hidden until corrected content is approved, or is its previously approved content restored to sale?
- Resolution: the entire Product remains hidden and unavailable for new purchases. Seller must correct and resubmit the content, pass validation, and obtain manual approval before revised content becomes eligible for publication. Previously approved content is not automatically restored to sale.
- Confirmation: 2026-10-07, Project owner replied `1` to the choices: (1) remain hidden until corrected content is approved, (2) restore previously approved content.
- Affects: UC-004/005/006/008/009/011, [BR-PROD-002](BUSINESS_RULES.md#br-prod-002), SM-PRODUCT-001, and recovery acceptance criteria.
- Boundary: editing, saving corrections, or passing automatic checks alone does not restore sale. Initial hide timing is Accepted under OQ-017; withdrawal/edit/resubmit is Accepted under OQ-018; withdrawal/review concurrency follows the first-accepted-action-wins rule in Accepted OQ-003; state-code mappings remain Proposed.

<a id="oq-017"></a>
## OQ-017 — Initial hide timing for risky edits

- Status: Accepted; decision state: Resolved.
- Question: for a currently active Product, does hiding begin when risky changes are successfully saved, or only when a valid revised submission enters manual review?
- Resolution: hide the entire Product and stop new purchases immediately when an authorized Seller successfully saves an actual high-risk change requiring re-review, even if the Seller has not submitted it for review. Keep it hidden while completing/correcting content, after submission validation failures, through pending review, and after rejection until manual approval.
- Confirmation: 2026-10-07, Project owner explicitly chose hiding immediately on saving changes requiring re-review, even before submission.
- Affects: UC-004/005/006/008/009/011, BR-PROD-002, SM-PRODUCT-001, and AC-UC-004-08.
- Boundary: opening an editor or a failed save does not satisfy the successful-change trigger. Operational-only edits retain their BR-PROD-001 exemption. Draft-save validity stays OQ-002; withdrawal/edit/resubmit is Accepted under OQ-018; withdrawal/review concurrency follows Accepted OQ-003; state-code mappings remain Proposed.

<a id="oq-018"></a>
## OQ-018 — Further risky edits while manual review is pending

- Status: Accepted; decision state: Resolved.
- Question: when a Seller wants to further edit high-risk content already submitted for manual review, must the Seller wait for the review decision, or may the Seller withdraw that submission, edit, and submit again?
- Resolution: Seller may explicitly withdraw a high-risk content submission while it is still pending review, edit the content, and submit it again. A successfully withdrawn submission is no longer eligible for Moderator approval or rejection. The entire Product remains hidden throughout withdrawal, editing, validation, resubmission, and renewed review until manual approval.
- Confirmation: 2026-10-07, Project owner explicitly selected withdrawal of the pending submission before editing and resubmitting; the withdrawn submission cannot be reviewed and the Product remains hidden.
- Affects: UC-004/005/006, [BR-PROD-003](BUSINESS_RULES.md#br-prod-003), BR-PROD-002, SM-PRODUCT-001, and submission identity/stale-decision acceptance criteria.
- Boundary: operational changes retain their manual-review exemption but cannot restore sale. A withdrawal is permitted only while that submission remains pending. Accepted OQ-003 makes the first accepted withdrawal/approval/rejection final for that submission; later conflicts are refused and repeated requests return the existing outcome without additional effects. State-code mappings remain Proposed.

<a id="oq-019"></a>
## OQ-019 — Public listing eligibility and cart revalidation

- Status: Proposed; decision state: Open.
- Owner: Project owner.
- Question: how do Product listing visibility, SKU purchase eligibility, and existing cart items react to Seller price/stock/activation changes and review-related hiding?
- Proposal: use [BR-PROD-004](BUSINESS_RULES.md#br-prod-004) as the common eligibility rule for UC-008/009/010/011. Display only approved high-risk Product content combined with current permitted operational SKU values; retain enabled zero-stock offers as visible but unavailable, and remove Products with no enabled valid SKU from public sale listings. A selected SKU requires sufficient available quantity before a new purchase can be accepted. Public lookup of review-hidden content returns an unavailable outcome without exposing unapproved content.
- Proposed cart boundary: cart membership creates no reservation, Order, or price guarantee. Validate each new addition/increase against Product/SKU eligibility, positive quantity rules, and the resulting total intended quantity for the exact Shop/SKU across the cart, not just the added increment; refuse an invalid attempt without applying its cart change. Keep existing unavailable cart items identifiable and removable, without exposing unapproved content or permitting purchase. Show current permitted prices and report changes from the previously presented amount; cart checks are advisory and checkout revalidates eligibility, price, and stock before acceptance. Do not silently substitute a different SKU or variant.
- Basis: Accepted BR-PROD-002 already hides and blocks new purchases immediately on successful risky saving; BR-PROD-001 exempts permitted operational SKU changes from manual re-review. The public out-of-stock, direct-lookup, and cart presentation policies above are new proposals, not additional Accepted decisions.
- Remaining dependencies: exact SKU validity/identity (OQ-002/OQ-006), Shop/account restrictions (OQ-004/OQ-005), available-stock definition and inventory timing (OQ-008), checkout price/snapshot confirmation and partial failure (OQ-007), and review/voucher policies (OQ-011/OQ-013). This proposal does not settle checkout-versus-edit ordering or apply OQ-003 beyond moderation.
- Affects: SC-002/SC-003/SC-004/SC-005/SC-010/SC-011/SC-012; UC-004/007/008/009/010/011, BR-PROD-002/004, SM-PRODUCT-001, glossary, and traceability.

## Resolution procedure

For each resolution, record the exact chosen policy, owner confirmation or applicable delegation source/date, status Accepted, decision state Resolved, and affected artifact updates. Use delegated authority only within its recorded scope. A proposal's age or lack of response does not resolve it.
