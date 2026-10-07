# Open Decisions and Decision Provenance

## Scheduling resolution — 2026-10-07

Owner explicitly requested a few complex core learning features, closing unfinished features for current work and deferring them to later ordered parts. [Learning plan](../01-product/LEARNING_PLAN.md) records this Accepted scheduling direction and supersedes earlier broad MVP/sprint scheduling. This does not revoke Accepted business policies or assert implementation. Deferred-feature questions remain dormant until their part is selected; only material Part 1 validation/permission/SKU issues are relevant to current design.

- Status: Draft register.
- Owner: Project owner.
- Updated: 2026-10-07.

The existing [moderation rule](BUSINESS_RULES.md#br-prod-001) is a **Baseline**: automatic validation precedes Moderator review; approval activates a Product; rejection requires a reason; risky content requires re-review; operational SKU changes do not. Its original sign-off provenance is not present. Normalization preserves that content without inventing approval history.

**Accepted** entries require recorded explicit owner confirmation or an applicable explicit delegation of decision authority, with the chosen policy and affected artifacts identified. General authorization to prepare documentation is not decision acceptance. Proposed defaults below are discussion options, not executable product policy. OQ-001, OQ-002, OQ-003, OQ-004, OQ-005, OQ-006, OQ-007, OQ-008, OQ-009, OQ-010, OQ-012, OQ-014, OQ-015, OQ-016, OQ-017, OQ-018, OQ-019, OQ-020, and OQ-021 are Resolved (Accepted for the MVP baseline). OQ-011 and OQ-013 remain Open (deferred to subsequent P1 increments per OQ-014).

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

- Status: Accepted; decision state: Resolved.
- Question: which fields/attributes are required per category; what prices, precision/currency, inventory units, media formats/sizes, category validity, and forbidden-word rules are allowed? Also define manual moderation criteria and actionable rejection-reason policy.
- Existing facts: invalid price, negative inventory, invalid category, unsupported image format, missing fields, and forbidden words block submission.
- Resolution:
  - Draft saving may retain incomplete listing content, including missing category, description, media, or category-required attributes. Supplied values must satisfy their applicable format, reference, and range rules; a missing value and an invalid supplied value are distinct findings. Incomplete listing content never becomes eligible for manual review or sale merely by saving.
  - An invalid supplied value rejects the attempted save without applying any of that attempt's content changes. A failed save does not trigger new review-related hiding; a successful actual risky change still triggers Accepted BR-PROD-002, even if the draft is incomplete.
  - Submission requires a nonempty Product name and description, a valid category permitted for listing, at least one supported Product image, all category-required attributes, and at least one enabled SKU with a valid price and non-negative inventory. Zero inventory is permitted for review but does not permit purchase without stock. SKU structure remains OQ-006; stock adjustments remain UC-007/OQ-008.
  - Submission rechecks all applicable rules against the exact content being admitted for review. Validation findings identify the field or content item, the unmet rule, and the correction required. Invalid content does not enter manual review; passing checks does not constitute approval.
  - Operational changes validate the changed values and relevant invariants before taking effect; they do not require every unfinished risky-content field to be complete. Such changes cannot restore sale while BR-PROD-002 applies.
  - Adopted concrete limits: single currency VND with whole-unit integer amounts; SKU price 1,000–500,000,000 VND; name 10–120 characters; description 20–3,000 characters; 1–9 images (JPG/PNG/WebP, up to 2 MB each); stock quantities are whole units, a single adjustment 1–99,999 with a reason code; at most 50 SKUs per Product; forbidden-word list maintained by Admin with case-insensitive whole-word matching; manual review criteria: prohibited goods, misleading content, rights/copyright concerns, with reason codes selected by the Moderator. Category schemas follow [BR-CATEGORY-001](BUSINESS_RULES.md#br-category-001).
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-003/004/005/006/007/008/009/010/011/012, BR-PROD-001/004 and BR-INV-001/ORDER-001/PAY-001.

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

- Status: Accepted; decision state: Resolved.
- Question: how do revision-requested, hidden, locked, restricted, and suspended outcomes differ for Products, Shops, and Users; who applies/reverses them; what happens to existing orders?
- Resolution: Shop registration and operating state uses [SM-SHOP-001](STATE_MACHINES.md#sm-shop-001) and Accounts use [SM-ACCOUNT-001](STATE_MACHINES.md#sm-account-001). `RESTRICTED`/`SUSPENDED` block new listings, new Orders, and new fulfillment; existing accepted Orders are never silently cancelled and continue under their own rules until an explicit Operations/Support decision. Sanctions, appeals, and discretionary staff recovery remain P1 capabilities under UC-021/022.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-001/002/006/021/022, SC-016/SC-018, SM-SHOP-001, SM-ACCOUNT-001.

<a id="oq-005"></a>
## OQ-005 — Shop and staff permissions

- Status: Accepted; decision state: Resolved.
- Question: define Shop registration approval, account verification, staff account type, ownership versus operator permissions, and the Admin/Moderator/Support/Operations action matrix.
- Resolution: Adopt the permission model and allow/deny matrix specified in [BR-ACCESS-001](BUSINESS_RULES.md#br-access-001):
  - Six Shop roles: `OWNER`, `PRODUCT_EDITOR`, `INVENTORY_CLERK`, `FULFILLMENT_OPERATOR`, `AFTERSALES_AGENT`, `VIEWER`. Only `OWNER` manages operators. A role is strictly scoped to one Shop and gives zero rights in another Shop.
  - Four Internal roles: `ADMIN` (categories, accounts, roles, forbidden words), `MODERATOR` (Product and Shop registration review), `SUPPORT` (disputes, refunds, payment reconciliation exceptions), `OPERATIONS` (violations, order interventions).
  - Internal roles never imply Buyer or Shop rights; internal actions are attributed to the internal role.
  - Buyer-owned resources (cart, addresses, Orders, payments, disputes) are private to the owning Buyer. Shop operators see only their own Shop's Order data and delivery address, never sibling-Shop or group data.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-001/002/003/004/005/006/007/008/009/010/011/012/013/014/015/016/017/024, BR-ACCESS-001, NFR-001.

<a id="oq-006"></a>
## OQ-006 — Variant and SKU identity

- Status: Accepted; decision state: Resolved.
- Question: define variant combinations, SKU cardinality/uniqueness, non-variant Products, and whether changing SKU attributes creates a new identity.
- Resolution:
  - A Product has 0–2 variant dimensions (e.g. color, size) and at most 50 SKUs. A Product without variants has one default SKU.
  - One SKU equals one unique combination of option values within its Product.
  - Each SKU has a stable, immutable platform identifier; its internal code is unique within the Shop and may change without changing identity.
  - Renaming an option label retains the SKU identity (and is a risky edit under BR-PROD-001); changing a combination's meaning is done by disabling the SKU and creating a new one.
  - A SKU referenced by a hold, Order, cart entry, or snapshot is disabled, never deleted; a hold/Order never migrates to another SKU. Disabling a SKU blocks new purchases only.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-004/007/008/009/010/011/012/013, glossary Product/Variant/SKU terms, purchase snapshot and reservation models.

<a id="oq-007"></a>
## OQ-007 — Multi-shop checkout and payment grouping

- Status: Accepted; decision state: Resolved.
- Question: one Payment Request for the checkout or one per Shop order; all-or-nothing or partial checkout; what totals and buyer-visible results follow partial failure?
- Also define the price/product/address/shipping snapshot captured for a purchase and its capture time.
- Resolution: Option 1 adopted in [BR-ORDER-001](BUSINESS_RULES.md#br-order-001) and [UC-011](USE_CASES/UC-011-checkout.md):
  - Accept all selected items together into one purchase group with one group Payment Request and separate Shop orders. A selected-item failure accepts none; Buyer can adjust selection and retry.
  - Atomically with checkout acceptance and stock reservation, capture the full snapshot: Shop/Product/SKU identities, variant choices, quantities, unit prices, subtotals, attributable shipping/discounts, per-Shop payables, group total (sum of Shop payables), delivery address, shipping choice, and payment deadline.
  - Later Product/SKU edits do not alter the snapshot. If risky editing/hiding happens first, checkout is refused under BR-PROD-002; if checkout is accepted first, existing Orders retain their snapshot and holds.
  - Idempotent repeats return the existing outcome; conflicting reuse is refused.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-007/010/011/012/013/015/017, BR-ORDER-001/INV-001/PAY-001, SM-ORDER-001/PAYMENT-001/RESERVATION-001.

<a id="oq-008"></a>
## OQ-008 — Inventory timing and reservation invariants

- Status: Accepted; decision state: Resolved.
- Question: what exactly constitutes order confirmation; may it occur before payment; what reservation lifetime, stock adjustment bounds, and release/restock rules apply; what happens to success received after expiry?
- Resolution: Option 1 adopted in [BR-INV-001](BUSINESS_RULES.md#br-inv-001) and [UC-007](USE_CASES/UC-007-manage-inventory.md):
  - Invariant: `on_hand >= reserved >= 0` and `available = on_hand - reserved`. Seller adjustments cannot reduce `on_hand` below `reserved`.
  - Timing: Hold stock at checkout against an unpaid deadline of **15 minutes**. Timely verified payment success converts unpaid holds to protected paid holds (`ACTIVE_PAID`) without deducting stock.
  - Seller confirmation of a Shop order consumes its paid holds and deducts stock (`on_hand` and `reserved` decrease together). Payment success alone is not confirmation.
  - Payment failure or unpaid expiry releases unpaid holds once without deduction. Late success after expiry requires financial reconciliation without resurrecting expired holds.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-007/008/009/010/011/012/013/015/017, BR-INV-001/ORDER-001/PAY-001, SM-INVENTORY-001/RESERVATION-001/ORDER-001/PAYMENT-001.

<a id="oq-009"></a>
## OQ-009 — Cancellation, Seller response, and paid Shop closure

- Status: Accepted; decision state: Resolved.
- Question: which actors may cancel/reject at which states; what Seller/paid-hold deadline applies; when may paid stock be released; what happens to paid amounts, shipping/discount attribution and sibling Shop orders?
- Resolution: Option 1 adopted in [BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001), [BR-ORDER-002](BUSINESS_RULES.md#br-order-002), [UC-013](USE_CASES/UC-013-fulfill-shop-order.md), and [UC-015](USE_CASES/UC-015-track-cancel-orders.md):
  - Buyer may cancel the entire unpaid group before expiry; all holds release and payment application is `CANCELLED`.
  - After payment success, each Shop order is fulfilled independently. Buyer cancellation or Seller rejection is allowed only before Seller confirmation and within the **24-hour Seller response deadline** (from applied success).
  - At or after deadline, unanswered paid Shop orders close automatically as `SELLER_TIMED_OUT`.
  - Closing a paid Shop order releases only that Shop's active holds without changing `on_hand`, preserves group payment `SUCCEEDED` and sibling Shop states, and records one stable `REQUIRED` compensation obligation for that Shop's snapshotted payable (items + attributed shipping − discounts).
  - Ordinary post-confirmation cancellation is refused (recourse follows dispute/after-sales UC-016/017).
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-007/011/012/013/015/017/022, BR-INV-001/ORDER-001/ORDER-002/PAY-001/CANCEL-001, SM-ORDER-001/RESERVATION-001/INVENTORY-001/PAYMENT-001.

<a id="oq-010"></a>
## OQ-010 — Delivery completion, after-sales, and financial disposition

- Status: Accepted; decision state: Resolved.
- Question: distinguish delivered from completed; define failed delivery, returns, refund/dispute eligibility and windows, evidence, staff authority, partial/full refunds, inventory receipt, closure, and appeal.
- Resolution: Options 1 adopted across all areas in [BR-AFTERSALES-001](BUSINESS_RULES.md#br-aftersales-001), [BR-REFUND-001](BUSINESS_RULES.md#br-refund-001), [BR-STOCK-RECEIPT-001](BUSINESS_RULES.md#br-stock-receipt-001), [SM-ORDER-001](STATE_MACHINES.md#sm-order-001), [SM-DISPUTE-001](STATE_MACHINES.md#sm-dispute-001), [SM-REFUND-001](STATE_MACHINES.md#sm-refund-001), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-016](USE_CASES/UC-016-request-return-refund.md), and [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md):
  - Completion: Delivery evidence sets `DELIVERED`. An Order moves to `COMPLETED` on explicit Buyer confirmation or when the **7-day auto-completion window** expires with no active dispute. Opening a dispute freezes auto-completion.
  - Dispute intake: Buyer may request `RETURN_AND_REFUND` (within 7 days of delivery) or `REFUND_ONLY` (non-delivery or unusable goods), capped at the Shop payable snapshot. Seller has **48 hours** to respond; timeout auto-accepts. Seller rejection escalates to Support staff for binding adjudication (Full, Partial, or Reject).
  - Physical restock control: Consumed inventory is **never restocked automatically**. Returned goods require Seller physical inspection: verified intact goods are restocked under [BR-STOCK-RECEIPT-001](BUSINESS_RULES.md#br-stock-receipt-001); damaged goods leave inventory unchanged.
  - Financial refund caps: Cumulative refunds across a purchase group are **strictly capped by verified successful paid funds** ([BR-REFUND-001](BUSINESS_RULES.md#br-refund-001)) using stable logical refund IDs (`REFUND-###`). Late payment unapplied funds with reconciliation `REQUIRED` are resolved by authorized Support refund, setting reconciliation to `RESOLVED`.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-007/012/013/014/015/016/017/022/023, BR-ORDER-002/CANCEL-001/SHIP-001/AFTERSALES-001/REFUND-001/STOCK-RECEIPT-001, SM-ORDER-001/SHIPMENT-001/DISPUTE-001/REFUND-001/INVENTORY-001.

<a id="oq-011"></a>
## OQ-011 — Voucher and promotion semantics

- Status: Draft; decision state: Open.
- Question: define funding/stacking, eligibility, minimum spend, quota reservation/consumption, validity, shipping discounts, and cancellation/refund treatment across Shop orders.
- Affects: UC-009/011/019/020 and glossary Voucher/Promotion definitions.

<a id="oq-012"></a>
## OQ-012 — Demo quality targets

- Status: Accepted; decision state: Resolved.
- Question: confirm the demo workload, latency threshold, recovery/retry targets, audit scope/retention, and isolation/security verification expectations.
- Resolution: Adopt the learning-demo quality targets and measurement plan specified in [NFRs](NON_FUNCTIONAL_REQUIREMENTS.md) and [MVP_RELEASE_PROPOSAL.md](../01-product/MVP_RELEASE_PROPOSAL.md#quality-targets-and-measurement-plan):
  - NFR-001: Zero unauthorized successes in the defined permission matrix across two Shops and four internal roles.
  - NFR-002: 10 replays of one logical partner event (including concurrent) yield exactly one effective transition; invalid signature produces zero state change.
  - NFR-003: Non-negative inventory with contention guard (at most one hold succeeds for a single unit under simultaneous checkouts); ledger matches expected business totals across failure, expiry, confirmation, and return.
  - NFR-004: 100% of actions in the defined audit inventory ([BR-AUDIT-001](BUSINESS_RULES.md#br-audit-001)) produce an attributable audit record.
  - NFR-005: Discovery and product detail respond within a p95 response time of <= 2 seconds under the learning-demo workload (100 Shops, 10,000 SKUs, 20 concurrent users).
  - NFR-006: Interrupted accepted partner processing is recovered within 60 seconds without duplicate side effects.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed demo quality targets: p95 response time <= 2s, observable recovery <= 60s, 10-replay idempotency verification, and 100% audit log coverage.
- Affects: NFR-001 through NFR-006, test strategy, and release verification. These are learning-demo targets, not production service commitments.
- Boundary: documentation checks do not count as behavioral evidence. Actual verification requires executed tests when implementation exists in Phase 5/6.

<a id="oq-013"></a>
## OQ-013 — Review eligibility and ratings

- Status: Draft; decision state: Open.
- Question: when may a Buyer review, which purchase/item is eligible, what editing/moderation is allowed, and how are Product/Shop ratings calculated?
- Affects: UC-008/009/018.

<a id="oq-014"></a>
## OQ-014 — Baseline and MVP confirmation

- Status: Accepted; decision state: Resolved.
- Question: confirm the Phase 1 baseline and the P0/P1 priorities in the catalog before the Phase 2 exit gate is passed.
- Resolution: Baseline scope confirmed. Adopt the MVP boundary proposal in [MVP release boundary and quality proposal](../01-product/MVP_RELEASE_PROPOSAL.md): Release 1 implements the 18 P0 Use Cases (UC-001 through UC-017, UC-024) across five vertical slices (Slice A: Foundation & Setup; Slice B: Product & Inventory; Slice C: Discovery & Cart; Slice D: Checkout, Payment & Logistics; Slice E: Order Management & After-Sales). The 6 P1 Use Cases (UC-018 Reviews & Ratings, UC-019 Shop Vouchers, UC-020 Platform Promotions, UC-021 Violations & Sanctions, UC-022 Staff Appeals & Interventions, UC-023 Compensation & Ledger) and their decisions (OQ-011, OQ-013) are explicitly deferred to post-MVP release increments.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices and MVP baseline in discussion ("đồng ý đề xuất chính sách").
- Affects: entire catalog, USE_CASE_CATALOG.md, and Phase 2 exit gate certification.

<a id="oq-015"></a>
## OQ-015 — Partner event identity and reconciliation

- Status: Accepted; decision state: Resolved.
- Question: identify the same logical partner event across retries; distinguish duplicates from conflicting outcomes; define permitted late/out-of-order updates, correlation, retry stopping, and reconciliation behavior.
- Resolution: Adopt Option 1 for both Payment and Logistics partner events:
  - **Payment Identity & Processing**: stable provider-scoped logical event identity across retries plus Payment Request/group correlation. Verify signature/source, amount, and currency before business effects. Identical repeats return prior result without new side effects; conflicting reuse or opposite terminal evidence is recorded for reconciliation without overwriting completed effects. Authoritative effect time is recorded by the platform upon acceptance, not external provider timestamp. A late success cannot resurrect expired/failed Orders or holds; retain verified financial evidence separately from fulfillment. Unknown results recover using original request/event identities before any retry. Interruption after receipt rechecks expiry and holds before applying effects. Contradictory evidence remains unresolved until authorized Support disposition.
  - **Logistics Identity & Sequencing**: provider-scoped immutable logical event identity, exact Shipment/Order correlation, and authoritative sequential transition graph per Shipment established by creation acknowledgement. Verify source/structure before processing; exact/equivalent repeats return prior processing without duplicate side effects. Apply contiguous sequence through the permitted graph; retain gaps/missing prerequisites for replay or authorized authoritative query. Late consistent evidence joins tracking history without regression; contradictions/invalid terminal transitions require reconciliation. Never create a Shipment from an unknown callback, guess sequence baselines, fast-forward missing paths, or overwrite ordinary terminal states (`DELIVERED`, `RETURNED`).
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-007/011/012/013/014/015/017, BR-PAY-001, BR-SHIP-001, SM-PAYMENT-001/ORDER-001/RESERVATION-001/SHIPMENT-001, NFR-002/NFR-006.

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

- Status: Accepted; decision state: Resolved.
- Question: how do Product listing visibility, SKU purchase eligibility, and existing cart items react to Seller price/stock/activation changes and review-related hiding?
- Resolution: Adopt the public listing eligibility and cart revalidation rules defined in [BR-PROD-004](BUSINESS_RULES.md#br-prod-004) and [UC-010](USE_CASES/UC-010-manage-cart.md):
  - Display only approved high-risk Product content combined with current permitted operational SKU values.
  - Retain enabled zero-stock offers as visible but unavailable for purchase.
  - Remove Products with no enabled valid SKU from public search/category listings. Direct lookup of review-hidden content returns an unavailable outcome without exposing unapproved draft content.
  - Cart membership creates no reservation, Order, or price guarantee. Adding or increasing items validates Product/SKU eligibility, positive quantity, and resulting total intended quantity for that Shop/SKU across the cart; invalid attempts are refused without cart changes.
  - Existing unavailable cart items remain identifiable and removable, without exposing unapproved content or permitting purchase. Show current permitted prices and report changes from previously presented amounts; cart checks are advisory and checkout revalidates eligibility, price, and stock before acceptance.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: SC-002/SC-003/SC-004/SC-005/SC-010/SC-011/SC-012, UC-004/007/008/009/010/011, BR-PROD-002/004, SM-PRODUCT-001.

<a id="oq-020"></a>
## OQ-020 — Account, verification, credential, and address policy

- Status: Accepted; decision state: Resolved.
- Question: how are accounts identified and verified, what protects sign-in, and what address limits apply?
- Resolution: Adopt Option 1 specified in [BR-ACCESS-001](BUSINESS_RULES.md#br-access-001), [SM-ACCOUNT-001](STATE_MACHINES.md#sm-account-001), and [UC-001](USE_CASES/UC-001-manage-account-addresses.md):
  - Email is the unique account identifier.
  - Verification uses a simulated one-time code (15-minute lifetime, single-use, newest code invalidates prior, maximum 5 resends per hour).
  - Unverified accounts may browse products but cannot checkout, make payments, or register a Shop.
  - Authentication security: five consecutive failed sign-in attempts lock the account for 15 minutes.
  - Address policy: each account may store up to 10 shipping addresses, with exactly one designated as default.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-001/002/011/012/016, SC-001, BR-ACCESS-001, SM-ACCOUNT-001, NFR-001/004.

<a id="oq-021"></a>
## OQ-021 — Category hierarchy and attribute-schema change policy

- Status: Accepted; decision state: Resolved.
- Question: how deep is the hierarchy, which attribute types exist, and what happens to existing Products when a schema changes or a category is deactivated?
- Resolution: Adopt Option 1 specified in [BR-CATEGORY-001](BUSINESS_RULES.md#br-category-001) and [UC-003](USE_CASES/UC-003-maintain-categories.md):
  - Category hierarchy has a fixed maximum depth of 3 levels (Root -> Subcategory -> Leaf). Products may only be listed in leaf categories.
  - Category attribute types are strictly typed: `TEXT`, `NUMBER`, `ENUM`, `BOOLEAN`.
  - Category schemas are versioned. Schema updates do not disrupt active products; existing Products remain untouched until their next risky edit or resubmission.
  - Deactivating a category prevents new product listings and resubmissions under that category, but does not silently delete or hide existing approved active products.
- Confirmation: 2026-10-07, Project owner explicitly approved the proposed policy choices in discussion ("đồng ý đề xuất chính sách").
- Affects: UC-003/004/005/008, SC-017, BR-CATEGORY-001.

## Resolution procedure

For each resolution, record the exact chosen policy, owner confirmation or applicable delegation source/date, status Accepted, decision state Resolved, and affected artifact updates. Use delegated authority only within its recorded scope. A proposal's age or lack of response does not resolve it.
