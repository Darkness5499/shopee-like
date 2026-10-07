# State Machines

- Status: Accepted lifecycle models for MVP Release 1 baseline — SM-PRODUCT-001 (retained baseline & accepted review-hiding), SM-INVENTORY-001, SM-RESERVATION-001, SM-ORDER-001, SM-PAYMENT-001, SM-SHIPMENT-001, SM-DISPUTE-001, SM-REFUND-001, SM-ACCOUNT-001, and SM-SHOP-001 are Accepted with recorded owner confirmation provenance.
- Owner: Project owner.
- Updated: 2026-10-07.
- Source: [BR-PROD-001](BUSINESS_RULES.md#br-prod-001), Accepted [BR-PROD-002](BUSINESS_RULES.md#br-prod-002), SC-007/SC-011/SC-012/SC-016/SC-025/SC-026 in [scope](../01-product/FUNCTIONAL_SCOPE.md).

State codes here represent business outcomes, not database fields or API contracts. Proposed codes/guards require confirmation. Product, order, payment, shipment, reservation, and after-sales lifecycles must be distinct even when their transitions cause related effects.

<a id="sm-product-001"></a>
## SM-PRODUCT-001 — Product publication and review

### States

- `DRAFT` — Proposed code: Seller content not yet submitted; not purchasable.
- `PENDING_REVIEW` — Proposed code: first submission passed automatic checks and awaits manual review; not purchasable.
- `ACTIVE` — Baseline code: approved Product. Proposed [BR-PROD-004](BUSINESS_RULES.md#br-prod-004) separates public listing from per-SKU purchase eligibility: zero stock does not alone remove an eligible listing, while a new purchase requires sufficient available quantity. Exact validity, Shop restrictions, and purchasing guards remain open.
- `REJECTED` — Baseline code: first submission rejected with a reason; not purchasable.

`APPROVED` names the moderation outcome, not a fifth Product state. SKU activation is independent of Product approval. These four state definitions describe first publication; the accepted re-review visibility behavior and its proposed code mapping are specified below.

### Allowed transitions, guards, and effects

- Create → `DRAFT`: Proposed; authorized Seller creates content under its Shop (UC-004). No publication occurs.
- `DRAFT` → `PENDING_REVIEW`: Proposed transition; authorized submission passes all deterministic checks (UC-005). Capture the submitted content for review and record the action.
- `DRAFT` → `DRAFT` on failed submission checks: Baseline validation blocks manual review; return validation failures. The code name is Proposed.
- `PENDING_REVIEW` → `ACTIVE`: Baseline approval outcome; Moderator reviews a valid submission (UC-006). Record decision, actor, and time; content becomes approved.
- `PENDING_REVIEW` → `REJECTED`: Baseline rejection outcome; Moderator provides a rejection reason (UC-006). Record decision, actor, time, and reason.
- `REJECTED` → `DRAFT` → `PENDING_REVIEW`: Proposed correction/resubmission path through UC-004/005; rejected content must pass automatic checks again before another review.
- `ACTIVE` → `ACTIVE` for operational edits: Baseline manual-review exemption for price, inventory, SKU activation, or internal SKU code; validate changed data and authorization. Inventory effects belong to UC-007.

Audit content derives from SC-022/[NFR-004](NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004). The mechanism storing reviewed content is a later design decision.

### Re-review boundary

Category, name, description, media, category attributes, or variant changes require re-review under BR-PROD-001. Accepted [BR-PROD-002](BUSINESS_RULES.md#br-prod-002) / [OQ-001](OPEN_DECISIONS.md#oq-001) requires the entire Product to be hidden from Buyer-facing sale and unavailable for new purchases while the revised content awaits manual review. Previously approved content does not continue selling during that review.

- Accepted initial business transition under [OQ-017](OPEN_DECISIONS.md#oq-017): successful saving of actual high-risk edits on an active Product → the entire Product is hidden before submission. Failed saves and opening an editor do not satisfy that trigger; operational-only changes do not require re-review.
- Accepted next step: valid re-review submission → pending manual review with the Product still hidden. Unsubmitted/unfinished content and validation failure do not restore sale. Operational SKU updates cannot bypass the visibility restriction.
- Proposed code mapping: `ACTIVE` → `DRAFT` (hidden) on successful risky saving; `DRAFT` → `PENDING_REVIEW` (still hidden) on valid submission; `PENDING_REVIEW` → `ACTIVE` on manual approval. This reuses draft/review code names for an existing Product and remains Proposed; the successful-save visibility trigger is Accepted independently.
- Approval restores eligibility to publish revised content subject to ordinary Shop/SKU/stock guards.
- Accepted rejection/recovery behavior under [OQ-016](OPEN_DECISIONS.md#oq-016): record a rejection reason and keep the entire Product hidden through correction and resubmission until revised content obtains manual approval. Previously approved content is not automatically restored; saving changes or passing automatic validation does not restore sale.
- Proposed rejection/correction code mapping: `PENDING_REVIEW` → `REJECTED` → `DRAFT` → `PENDING_REVIEW` → `ACTIVE` on eventual approval. This reuses existing code names for the business path, but the mapping is not owner-confirmed; the hidden/not-purchasable invariant is Accepted independently of those names.
- Initial hiding is Accepted under OQ-017: successful risky saving hides before submission, including when later validation fails. Draft-save validity remains OQ-002. Accepted [BR-PROD-003](BUSINESS_RULES.md#br-prod-003) permits withdrawal before further risky edits; the withdrawn submission becomes ineligible and the Product remains hidden. Under Accepted OQ-003, the first accepted withdrawal/approval/rejection wins and later conflicting actions are invalid; repeats have no additional effect. Continued hiding after rejection remains OQ-016.
- Proposed withdrawal code mapping: `PENDING_REVIEW` → `DRAFT` (still hidden) on successful withdrawal; later valid resubmission returns to `PENDING_REVIEW`. These code names remain Proposed even though the withdrawal behavior is Accepted.
- Temporary review hiding is a visibility consequence, not an accepted new `HIDDEN` state or a staff sanction. Staff hiding/locking and its recovery stay under OQ-004.

### Invalid or blocked actions

- Invalid submission cannot enter manual review.
- Rejection without a reason cannot produce a rejection decision.
- An unauthorized actor cannot submit or moderate another Shop's content.
- Seller cannot directly approve content to bypass Moderator review.
- Accepted submission safeguard: a decision applies only to the exact eligible submission. The first accepted withdrawal/approval/rejection wins; later conflicting actions are invalid. Repeated identical actions return the existing outcome without another transition or side effect. Revised resubmission after withdrawal is a new eligible submission.
- Staff revision requests, staff-imposed hiding/locking, deletion, and intervention recovery remain unspecified under [OQ-004](OPEN_DECISIONS.md#oq-004). Seller withdrawal and its competing-action behavior follow Accepted BR-PROD-003/OQ-003/OQ-018. Staff interventions do not override Accepted review-related hiding.

### Acceptance references

UC-004/005/006 acceptance criteria exercise draft creation, submission failures, approval/rejection, operational edits, permissions, withdrawal/edit/resubmit, and deterministic competing/repeated actions. Visibility is covered from successful risky saving through eventual approval. Code mappings and staff interventions remain incomplete.

Draft UC-008/009/010/011 extend Accepted review hiding into discovery, direct lookup, previously selected cart items, and final checkout guards. Proposed BR-PROD-004 defines ordinary listing/SKU/cart eligibility without adding Product state codes. Positive stock, cart membership, or SKU activation cannot override review hiding. UC-007/011/012 specify checkout/reservation consistency as Proposed under OQ-007/OQ-008/OQ-015/OQ-019.

<a id="sm-inventory-001"></a>
## SM-INVENTORY-001 — SKU inventory quantity model

- Status: Proposed quantity/transition model; this is not a list of Product publication states.
- Owner: Project owner.
- Scope/source: SC-012; [BR-INV-001](BUSINESS_RULES.md#br-inv-001), [OQ-008](OPEN_DECISIONS.md#oq-008).

### Quantities, guards, and changes

For each exact Shop/SKU, on_hand is recorded physical stock, reserved is all effective unpaid/paid hold quantities, and available = on_hand - reserved. Require on_hand >= reserved >= 0 and valid units/precision. `AVAILABLE` and `OUT_OF_STOCK` may describe evaluated quantity but are not accepted persisted states.

- Stock-in q: authorized valid positive movement increases on_hand and available by q; reserved unchanged.
- Adjustment to n: authorized correction with current n >= reserved changes on_hand and available by the difference; reserved unchanged. Protect intervening accepted changes or refuse stale input for renewed review.
- Checkout reservation q: require purchasing guards and q <= available; increase reserved by q, decrease available by q, leave on_hand unchanged.
- Unpaid-to-paid hold: change hold eligibility, with all quantity counters unchanged.
- Release/expiry q: require a permitted active hold transition; decrease reserved and increase available by q; on_hand unchanged.
- Seller confirmation consumes q: require eligible paid Order and all its paid holds; decrease on_hand and reserved by q together; available unchanged. All lines of that Shop order consume together.
- Return receipt q: authorized physical inspection verifies q intact/resellable units received from an after-sales return or undelivered courier return under BR-STOCK-RECEIPT-001; increases on_hand and available by q; reserved unchanged. Damaged, defective, or scrap items leave counters unchanged.

At every accepted outcome enforce the invariant against all active holds, including overdue unpaid holds until recorded expiry. Never free quantity both by time arithmetic and by later release. Invalid quantities/authorization, adjustments below reserved, repeat movements or terminal-hold reuse cannot produce another stock effect. Each effective change retains attributable before/after and Shop/SKU/purchase correlation; mechanism deferred to design.

Coverage: [UC-007](USE_CASES/UC-007-manage-inventory.md), AC-UC-007-01 through 18; AC-UC-011-04/08/09/12 and AC-UC-012-03/06 through 10/23/24; [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); NFR-003/004. Proposed pre-confirmation cancellation/rejection/timeout releases the owning holds under BR-CANCEL-001; units/identity/permissions remain OQ-002/005/006. These are unexecuted criteria.

<a id="sm-reservation-001"></a>
## SM-RESERVATION-001 — Exact purchase-item stock hold

- Status: Proposed codes, transitions, grouping and paid-hold protection; Baseline reserve/release/deduct boundary retained.
- Owner: Project owner.
- Scope/source: SC-005/012/013; [BR-INV-001](BUSINESS_RULES.md#br-inv-001), BR-ORDER-001/PAY-001; OQ-007/OQ-008/OQ-009/OQ-015.

### States and identity

- `ACTIVE_PAYMENT`: stock held for one exact Shop/SKU and its purchase-group/Shop-order quantity; payment awaits an authoritative common deadline. Counts in reserved even if overdue until recorded expiry.
- `ACTIVE_PAID`: timely verified payment has secured this hold for Seller confirmation. Counts in reserved; original unpaid deadline cannot release it.
- `RELEASED`: a permitted failure or later owning cancellation/rejection action relinquished the hold; terminal.
- `EXPIRED`: unpaid deadline/valid provider expiry relinquished the hold; terminal.
- `CONSUMED`: Seller confirmed its paid Shop order and the held stock was deducted; terminal.

Quantity and Shop/SKU/purchase associations cannot silently move to a new identity. A new purchase needs new holds. Identity formats/storage remain design work.

### Allowed transitions, guards, and effects

- No hold → `ACTIVE_PAYMENT`: entire selected checkout accepted with current purchasing/stock guards. Apply all group holds and Orders together; no physical deduction.
- `ACTIVE_PAYMENT` → `ACTIVE_PAID`: request/group/amount correlation verified; all group unpaid holds active; effect accepted strictly before deadline. Apply the full payment/Order/hold outcome together; counters unchanged.
- `ACTIVE_PAYMENT` → `RELEASED`: admissible payment failure or explicit whole unpaid-group cancellation strictly before its deadline while the group still awaits payment. Release all group unpaid holds once with the owning payment/Order outcome under BR-PAY-001/BR-CANCEL-001.
- `ACTIVE_PAYMENT` → `EXPIRED`: authoritative unpaid deadline reached or admissible verified provider expiry, before completed success. Release all group unpaid holds once; no physical deduction.
- `ACTIVE_PAID` → `CONSUMED`: authorized Seller confirms the eligible paid Shop order; all its required holds exist and match. Deduct on_hand and reserved once together per Shop order; preserve sibling Orders/holds.
- `ACTIVE_PAID` → `RELEASED`: Proposed eligible pre-confirmation Buyer cancellation, Seller rejection or Seller timeout under [BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001). Together close that Shop order, release all its paid holds and establish its complete compensation obligation; counters change by release only, without physical deduction. Rejection/cancellation accepts strictly before Seller deadline; timeout at/after. If complete facts/effects cannot be established, retain the active paid holds and expose recovery.

No late success converts terminal holds, no unpaid timer expires paid holds, and no refund/delivery event alone restores stock. Repeats preserve outcome without added effects. Paid-hold Seller deadline/closure is Proposed OQ-009; consumed-stock restock and after-sales remain unspecified OQ-010. Mere time passage cannot release paid holds independently of the owning Order/obligation outcome.

Coverage: AC-UC-007-06 through 18, AC-UC-011-08/09/12/13, AC-UC-012-03/05 through 10/15 through 17/23/24. See the owning [inventory](USE_CASES/UC-007-manage-inventory.md), [checkout](USE_CASES/UC-011-checkout.md), and [payment](USE_CASES/UC-012-process-payment.md) specifications.

<a id="sm-order-001"></a>
## SM-ORDER-001 — Shop order purchase, fulfillment, and closure

- Status: Proposed lifecycle through delivery/exception and pre-confirmation closure; Baseline per-Shop orders and Seller confirmation retained. Completion/after-sales pending.
- Owner: Project owner.
- Scope/source: SC-005/007/012/013; BR-ORDER-001/002, BR-INV-001/PAY-001/CANCEL-001/SHIP-001; OQ-007/008/009/010/015.

### States

- `PAYMENT_PENDING`: accepted snapshot/unpaid holds, group payment not applied; Seller confirmation not permitted.
- `PAID_AWAITING_CONFIRMATION`: applied group success and protected paid holds, with a recorded Seller-response deadline; no Seller acceptance/deduction yet.
- `CONFIRMED`: Seller accepted the whole paid Shop order and consumed/deducted all its holds once.
- `PACKED`: Seller packed all confirmed lines; not yet partner pickup.
- `SHIPPED`: authenticated accepted pickup and linked Shipment; in-transit, retry/failure and returning are Shipment details.
- `DELIVERED`: authenticated accepted delivered evidence; awaiting Buyer confirmation or auto-completion timeout.
- `COMPLETED`: Buyer confirmed receipt or auto-completion timer elapsed without active dispute; regular return window closed.
- `DISPUTED`: eligible after-sales dispute active; auto-completion suspended.
- `DELIVERY_EXCEPTION`: linked Shipment returned; resolution awaits UC-016/017.
- `PAYMENT_FAILED` / `PAYMENT_EXPIRED`: unpaid path closed with released unpaid holds; financial exceptions can remain open.
- `CANCELLED`: eligible unpaid-group or paid-Shop Buyer closure, with original cause and payment/compensation distinction retained.
- `SELLER_REJECTED` / `SELLER_TIMED_OUT`: pre-confirmation paid closure with paid holds released and a complete compensation obligation required.
- `REFUNDED`: order closed following complete refund/compensation execution under BR-REFUND-001.

### Transitions, guards, and effects

- Checkout → `PAYMENT_PENDING`: all selected Shop orders/snapshots/holds accepted together under BR-ORDER-001.
- `PAYMENT_PENDING` → `PAID_AWAITING_CONFIRMATION`: verified timely group success and all unpaid holds become paid together. Record applied-success time and each Seller deadline using the Proposed OQ-009 duration; retries do not extend it.
- `PAYMENT_PENDING` → `PAYMENT_FAILED` / `PAYMENT_EXPIRED`: admissible group failure/expiry closes all unpaid Shop orders and relinquishes holds once.
- `PAYMENT_PENDING` → `CANCELLED`: owning Buyer cancels the entire still-pending group before unpaid expiry; payment application CANCELLED and all unpaid releases happen together. No single unpaid Shop cancellation.
- `PAID_AWAITING_CONFIRMATION` → `CONFIRMED`: authorized Seller, applied successful payment without blocking reconciliation, all matching paid holds, acceptance strictly before Seller deadline; consume/deduct only this Shop order together.
- `PAID_AWAITING_CONFIRMATION` → `CANCELLED` / `SELLER_REJECTED`: eligible owning Buyer/Shop Seller before Seller deadline; full paid-hold release plus stable complete compensation obligation and cause together. Seller rejection needs a reason. Group payment stays SUCCEEDED and siblings unchanged.
- `PAID_AWAITING_CONFIRMATION` → `SELLER_TIMED_OUT`: authoritative time at/after Seller deadline; same complete release/obligation outcome, even with a linked financial exception when applied success/holds remain coherent. A delayed timer cannot allow late confirmation/rejection/cancellation.
- `CONFIRMED` → `PACKED`: authorized all-lines packing, paid financial prerequisites under BR-ORDER-002; no stock change.
- `PACKED` → `SHIPPED`: permitted authenticated pickup synchronized with the established Shipment under BR-SHIP-001. Creation acknowledgement or Seller handover alone leaves Order PACKED.
- `SHIPPED` → `DELIVERED`: permitted Shipment delivered outcome and Order update together.
- `SHIPPED` → `DELIVERY_EXCEPTION`: permitted Shipment returned outcome and Order update together; no compensation/restock by implication.
- `DELIVERED` → `COMPLETED`: explicit Buyer receipt acknowledgment or 7-day auto-completion window elapsed strictly with no active dispute ([BR-AFTERSALES-001](BUSINESS_RULES.md#br-aftersales-001)).
- `DELIVERED` / `DELIVERY_EXCEPTION` → `DISPUTED`: authorized Buyer submits eligible dispute case within allowed window; suspends auto-completion timer ([UC-016](USE_CASES/UC-016-request-return-refund.md)).
- `DISPUTED` → `REFUNDED`: dispute resolved with full refund executed under [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md).
- `DISPUTED` → `COMPLETED`: dispute rejected by Staff or withdrawn by Buyer; Order closes as completed.
- Pre-confirmation closed Shop order (`CANCELLED`, `SELLER_REJECTED`, `SELLER_TIMED_OUT`) with executed compensation obligation → `REFUNDED`.

### Invalid actions, recovery, and associated data

Ordinary post-confirmation cancellation/rejection, unpaid confirmation, partial consumption, missing/inconsistent holds, SKU substitution, invalid pickup and terminal reopening are refused. No payment replay resets Seller progress. Applied financial exceptions block new fulfillment commands; already established Shipment facts use the independent logistics guards. Staff restrictions and SKU reidentification need OQ-004/OQ-006 disposition, without a guessed bypass or automatic rollback.

Confirmation/rejection/cancellation/timeout compete on current authoritative state/time and the exact complete hold set under BR-CANCEL-001. After authorization and original identity comparison, an accepted repeat returns its original outcome plus current progress before new-action guards; changed instructions are refused. Recovery reports a complete original outcome or no accepted effect, not partial Order/stock/obligation success. Audit retains actor/system, Order/group/hold/Shipment/obligation identity, before/after, cause and effective time without private sibling data disclosure.

Associated compensation is `REQUIRED` when a paid pre-confirmation closure establishes its obligation. Resolution is handled in UC-017 under BR-REFUND-001. The amount/currency are the immutable Shop payable and refer to the original group payment; independent obligations do not rewrite the group amount. Payment reconciliation remains a separate annotation, not a fulfillment state.

### Coverage and remaining boundary

UC-007/011/012 purchase criteria plus [UC-013](USE_CASES/UC-013-fulfill-shop-order.md), [UC-014](USE_CASES/UC-014-simulate-shipment.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-016](USE_CASES/UC-016-request-return-refund.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); NFR-001/002/003/004/006. Post-completion staff interventions, replacement parcels, and partial voucher restorations remain pending OQ-004/010/011.

<a id="sm-payment-001"></a>
## SM-PAYMENT-001 — Payment Request application and reconciliation

- Status: Proposed; financial provider evidence is separate from payment application to the purchase.
- Owner: Project owner.
- Scope/source: SC-006/026; [BR-PAY-001](BUSINESS_RULES.md#br-pay-001), OQ-007/OQ-008/OQ-015; NFR-002/NFR-006.

### Application outcomes and exception status

- `PENDING`: stable request/group identity with snapshotted amount/currency; creation or provider/processing outcome may still be unknown. An unknown transport result does not equal failure.
- `SUCCEEDED`: verified timely success fully applied to group paid Orders/holds, without Seller confirmation or stock deduction.
- `FAILED`: admissible verified failure closed the pending group and released its unpaid holds.
- `EXPIRED`: authoritative unpaid deadline or admissible verified provider expiry closed the pending group and released its unpaid holds.
- `CANCELLED`: owning Buyer explicitly closed the entire pending group before unpaid expiry under BR-CANCEL-001, cancelling every unpaid Shop order and releasing its holds. This application outcome does not assert that the provider never received funds.

Separately, reconciliation is `CLEAR` (no observed mismatch), `REQUIRED` (late/conflicting evidence or inconsistent application), or `RESOLVED` (authorized disposition recorded; authority/actions still open under OQ-005/009/010/015). Provider success evidence can coexist with application EXPIRED or FAILED and reconciliation REQUIRED. Provider evidence is not discarded or falsely relabelled as no payment.

### Allowed transitions, guards, and effects

- Establish PENDING with the original logical identity; create/recover the simulated provider request under that identity once. Restarts/retries keep the accepted amount/currency/deadline.
- PENDING → SUCCEEDED: authentic matching evidence, all group Orders pending, exact unpaid holds active, acceptance strictly before deadline. Paid Order/hold changes occur together; counters unchanged.
- PENDING → FAILED: admissible authentic failure and no completed success; close all payment-pending Orders and release unpaid holds once.
- PENDING → EXPIRED: deadline reached or admissible verified provider expiry and no completed success; close all payment-pending Orders and expire unpaid holds once.
- PENDING → CANCELLED: permitted owning-Buyer whole-group cancellation strictly before unpaid deadline, all Orders/holds still pending; cancel/release all together under BR-CANCEL-001. A callback or local unknown creation can still arrive afterward; record authentic financial evidence and reconciliation without reinitiating/reopening.
- Malformed/unauthenticated evidence: reject without payment/Order/hold effects or a conflict case solely from that claim. Authenticated known event identity: compare to its original record first; changed immutable facts → reconciliation REQUIRED on the original association, preserve effects and never apply altered financial facts/disclose another purchase. For a new authenticated event identity, failed correlation is rejected; admissible opposite terminal evidence requires reconciliation.
- CANCELLED + authentic failure/expiry: retain corroborating provider evidence without relabelling Buyer cancellation or releasing again. CANCELLED + authentic success requires reconciliation.
- Failed/expired/cancelled outcome + authentic late success: preserve evidence and require reconciliation; do not reopen Orders, create holds, deduct stock or report refund completed. Deadline reached while still PENDING prevents success; expiry still closes/releases once.
- Success + original expiry or repeat: preserve SUCCEEDED and paid/later Seller outcomes; original unpaid timer has no paid-stock effect.
- Receipt-only interruption: recovery checks original identities, prior committed outcome and current time/holds. It can complete eligible effects once, return an existing outcome, or record a late/inconsistent exception. Receipt alone cannot establish SUCCEEDED or stop expiry.

Exact logical repeats have no extra effective audit, notification intent or stock/Order effects. Delivery/diagnostic records can distinguish transport attempts from effective business changes under agreed audit policy. No terminal-to-PENDING retry, blind terminal overwrite, automatic charge/refund, or assumed exception closure is allowed. Duplicate same-outcome evidence under different event identities cannot repeat an already completed effect; correlation still applies.

Coverage: [UC-012](USE_CASES/UC-012-process-payment.md), AC-UC-012-01 through 24; AC-UC-007-09 through 16 and AC-UC-011-13. Unpaid duration is Proposed OQ-008; zero-payable/money precision, retry budgets, retention and reconciliation resolution remain OQ-002/007/009/010/011/012/015. Paid-Shop closure leaves SUCCEEDED/group amount unchanged and records separate REQUIRED compensation under BR-CANCEL-001; full/partial refund execution is pending. Logistics is a separate Proposed SM-SHIPMENT-001 model.

<a id="sm-shipment-001"></a>
## SM-SHIPMENT-001 — Simulated Shipment creation and delivery

- Status: Proposed; single whole Shipment, progress graph, sequence and correction policy unconfirmed.
- Owner: Project owner.
- Scope/source: SC-024/025/007/013; [BR-SHIP-001](BUSINESS_RULES.md#br-ship-001), OQ-010/OQ-015.

### States and identity

- `CREATION_PENDING`: stable local creation identity for the packed Order, possibly unknown simulator outcome.
- `AWAITING_PICKUP`: correlated authenticated creation acknowledgement and authoritative sequence baseline established.
- `PICKED_UP`: permitted partner pickup applied; Order SHIPPED.
- `IN_TRANSIT`: permitted transit progress, including a retry after failed delivery.
- `DELIVERY_FAILED`: failed attempt; nonterminal, reason visible, not completed cancellation/refund.
- `RETURNING`: goods moving back after failed delivery; not proof of received usable inventory.
- `DELIVERED` / `RETURNED`: terminal ordinary logistics progress; completion/after-sales receipt remains separate.

The logical creation identity belongs to one whole Shop order with accepted address/items/shipping facts. Provider Shipment identity is correlated once, never silently replaced. Provider-scoped event ID, immutable evidence and authoritative sequence are separate from transport-attempt ID and occurrence/receipt time. Missing source sequence baseline cannot be guessed.

### Allowed graph and effects

- No Shipment → `CREATION_PENDING`: authorized creation for a paid PACKED Order with coherent completed confirmation effects satisfying BR-ORDER-002 and one-Shipment guard; no pickup/stock effect.
- `CREATION_PENDING` → `AWAITING_PICKUP`: verified matching creation acknowledgement, mapping and baseline; Order stays PACKED. Unknown response recovers the same identity, never another parcel/request.
- `AWAITING_PICKUP` → `PICKED_UP`: contiguous correlated pickup; Order PACKED → SHIPPED together.
- `PICKED_UP` → `IN_TRANSIT`: contiguous permitted progress; Order stays SHIPPED.
- `IN_TRANSIT` → `DELIVERED`: contiguous delivery fact; Order SHIPPED → DELIVERED together.
- `IN_TRANSIT` → `DELIVERY_FAILED`: failed attempt with evidence/reason; Order stays SHIPPED, exception visible.
- `DELIVERY_FAILED` → `IN_TRANSIT`: permitted retry evidence; same Shipment/Order, no replacement or second deduction.
- `DELIVERY_FAILED` → `RETURNING` → `RETURNED`: permitted return path in contiguous sequence; RETURNED synchronizes Order to DELIVERY_EXCEPTION. No refund/restock/approved Buyer Return follows automatically.

### Event guards, invalid actions, and recovery

Apply authenticity/identity/correlation, repeat/conflict classification and contiguous sequence before graph effects under BR-SHIP-001. Sequence gaps/missing prerequisite are deferred and observable until authorized replay/query recovers the complete permitted path; no fast-forward PICKED_UP → DELIVERED, progress-rank sorting or inferred predecessor. Late consistent evidence may be retained as history without regression; same ID/sequence contradictions and invalid terminal changes preserve established effects with reconciliation REQUIRED. Exact/equivalent already-applied evidence returns existing processing/current progress before state guards, without another Order/audit/notification effect. Equivalent current-state evidence at the next contiguous sequence may account for that sequence as a no-op under BR-SHIP-001; exact/older repeats do not advance twice. Genuine retry progress uses the graph even if that status appeared in earlier history.

Ordinary DELIVERED/RETURNED do not transition back to transit or to each other. Corrections need explicit authority/evidence/policy under OQ-005/010/015. A callback cannot create an unknown local Shipment, change association/address, cancel a confirmed Order, alter payment amount, release consumed holds, deduct stock again or restore inventory. Receipt-only interruption recovers with current recorded progress/sequence; coherent Shipment/Order progress and effective side effects are accepted together or none. Already observed physical facts remain linked to any financial exception.

Coverage: [UC-014](USE_CASES/UC-014-simulate-shipment.md) tracking/creation/recovery criteria, UC-013 handover and UC-015 private tracking; NFR-001/002/004/006. Protocol fields, signatures, exact sequence baseline, gaps/retry stopping/query authority/retention, cancellations after confirmation, multiple parcels and terminal corrections remain pending. No partner simulation or acceptance tests are executed.

<a id="sm-dispute-001"></a>
## SM-DISPUTE-001 — After-sales dispute and return case

- Status: Proposed; dispute states, return shipment and adjudication transitions are reviewable proposals under OQ-010.
- Owner: Project owner.
- Scope/source: SC-008/020/023; [BR-AFTERSALES-001](BUSINESS_RULES.md#br-aftersales-001), [BR-STOCK-RECEIPT-001](BUSINESS_RULES.md#br-stock-receipt-001), OQ-010.

### States

- `SUBMITTED`: Buyer created dispute request with reason, type, evidence, and requested amount; pending system dispatch.
- `AWAITING_SELLER_RESPONSE`: Seller notified with active response deadline (48h candidate); Order auto-completion frozen.
- `AWAITING_RETURN_SHIPMENT`: Seller or Staff approved return; Buyer provided return instructions and tracking deadline.
- `AWAITING_RETURN_RECEIPT`: Buyer dispatched return parcel; courier transit in progress to Seller address.
- `UNDER_STAFF_REVIEW`: Seller rejected request or return dispute arose; Support agent assigned to adjudicate.
- `APPROVED_PENDING_REFUND`: dispute resolved in Buyer favor (Full or Partial refund agreed); refund execution dispatched under SM-REFUND-001.
- `REFUNDED`: simulated refund confirmed executed; case closed.
- `REJECTED`: Support rejected dispute in Seller favor; Order proceeds to `COMPLETED`; case closed.
- `WITHDRAWN`: Buyer cancelled dispute before decision; Order resumes normal lifecycle; case closed.

### Transitions, guards, and effects

- No case → `SUBMITTED`: eligible Buyer, owned Shop order in `DELIVERED` (within 7-day window) or `DELIVERY_EXCEPTION`/`SHIPPED` (failed delivery/lost), no active dispute, requested amount <= Shop payable under BR-AFTERSALES-001. Freeze auto-completion timer on Order.
- `SUBMITTED` → `AWAITING_SELLER_RESPONSE`: system validates inputs, establishes stable case identity (`DISP-###`), sets 48h Seller response deadline.
- `AWAITING_SELLER_RESPONSE` → `APPROVED_PENDING_REFUND`: Seller explicitly accepts `REFUND_ONLY` request, or Seller response deadline expires without response (auto-accepted).
- `AWAITING_SELLER_RESPONSE` → `AWAITING_RETURN_SHIPMENT`: Seller explicitly accepts `RETURN_AND_REFUND` request; system issues return instructions to Buyer.
- `AWAITING_SELLER_RESPONSE` → `UNDER_STAFF_REVIEW`: Seller explicitly rejects request with recorded reason and counter-evidence.
- `AWAITING_RETURN_SHIPMENT` → `AWAITING_RETURN_RECEIPT`: Buyer provides valid return carrier tracking identity within return shipping window.
- `AWAITING_RETURN_SHIPMENT` → `REJECTED`: Buyer fails to provide return tracking within return shipping deadline; case closed in Seller favor.
- `AWAITING_RETURN_RECEIPT` → `APPROVED_PENDING_REFUND`: Seller inspects return parcel and confirms physical receipt of goods. If intact, Seller restocks under SM-INVENTORY-001; if damaged, no restock. Case moves to refund execution.
- `AWAITING_RETURN_RECEIPT` → `UNDER_STAFF_REVIEW`: Seller claims return parcel not received, empty, or fraudulent; case escalated to Support.
- `UNDER_STAFF_REVIEW` → `APPROVED_PENDING_REFUND`: Support agent adjudicates in Buyer favor (Full or Partial refund).
- `UNDER_STAFF_REVIEW` → `REJECTED`: Support agent adjudicates in Seller favor (denies refund). Order moves `DISPUTED` → `COMPLETED`.
- `APPROVED_PENDING_REFUND` → `REFUNDED`: simulated payment refund succeeds via SM-REFUND-001.
- `AWAITING_SELLER_RESPONSE` / `AWAITING_RETURN_SHIPMENT` → `WITHDRAWN`: Buyer explicitly cancels dispute. Unfreezes Order auto-completion timer.

### Coverage

[UC-016](USE_CASES/UC-016-request-return-refund.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); SM-ORDER-001/INVENTORY-001/REFUND-001; NFR-001/004.

<a id="sm-refund-001"></a>
## SM-REFUND-001 — Compensation and refund execution

- Status: Proposed; execution lifecycle, caps, and reconciliation disposition under OQ-009/010/015.
- Owner: Project owner.
- Scope/source: SC-006/020/026; [BR-REFUND-001](BUSINESS_RULES.md#br-refund-001), [BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001).

### States

- `REQUIRED`: compensation obligation established from pre-confirmation closure or dispute approval; awaiting execution dispatch.
- `EXECUTION_PENDING`: simulated refund request dispatched to simulated Payment Provider under stable refund ID (`REFUND-###`).
- `SUCCEEDED`: verified provider callback/response confirms refund execution; cumulative paid funds cap respected; terminal.
- `FAILED`: provider rejects refund (e.g., simulated network failure or invalid request); obligation remains open for retry or manual Support review.

### Transitions, guards, and effects

- None → `REQUIRED`: established atomically by paid Shop order cancellation/rejection/timeout ([BR-CANCEL-001](BUSINESS_RULES.md#br-cancel-001)) or dispute approval ([SM-DISPUTE-001](#sm-dispute-001)). Amount is snapshotted payable or approved partial amount.
- `REQUIRED` → `EXECUTION_PENDING`: system or staff dispatches refund call to simulated Payment Provider. Enforce `cumulative executed refunds + this refund <= verified paid funds` ([BR-REFUND-001](BUSINESS_RULES.md#br-refund-001)). Assign unique logical refund ID.
- `EXECUTION_PENDING` → `SUCCEEDED`: verified provider success callback received. Mark obligation/refund `SUCCEEDED`. If linked to a payment reconciliation requirement (e.g. late payment), mark reconciliation `RESOLVED`.
- `EXECUTION_PENDING` → `FAILED`: verified provider failure callback received. Preserve diagnosable failure; allow authorized retry under same refund ID or manual correction.

### Coverage

[UC-012](USE_CASES/UC-012-process-payment.md), [UC-015](USE_CASES/UC-015-track-cancel-orders.md), [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md); SM-PAYMENT-001/ORDER-001; NFR-002/004/006.

<a id="sm-account-001"></a>
## SM-ACCOUNT-001 — User account

- Status: Proposed under OQ-020/OQ-004.
- Owner: Project owner.
- Scope/source: SC-001; [BR-ACCESS-001](BUSINESS_RULES.md#br-access-001), [UC-001](USE_CASES/UC-001-manage-account-addresses.md).

### States

- `UNVERIFIED`: registered, contact not verified; browsing only.
- `ACTIVE`: verified; may buy and, with an `ACTIVE` Shop, operate.
- `RESTRICTED`: staff-limited actions; exact limits depend on UC-021/OQ-004.
- `SUSPENDED`: no sign-in-gated actions except reading own historical Orders.

### Transitions, guards, and effects

- None → `UNVERIFIED`: unique identifier and valid credential; issues a code.
- `UNVERIFIED` → `ACTIVE`: correct, unexpired, unused newest code; consumes the code.
- `ACTIVE` → `RESTRICTED`/`SUSPENDED` and back: authorized Internal Staff action with reason and audit (UC-021, not specified here).
- Invalid: verifying an `ACTIVE` account again has no effect; `SUSPENDED` Users cannot start new purchases or Shop actions.

### Coverage

UC-001; NFR-001/004.

<a id="sm-shop-001"></a>
## SM-SHOP-001 — Shop registration and operating state

- Status: Proposed under OQ-004/OQ-005.
- Owner: Project owner.
- Scope/source: SC-009/016; [BR-SHOP-001](BUSINESS_RULES.md#br-shop-001), [UC-002](USE_CASES/UC-002-register-manage-shop.md).

### States

- `PENDING_REVIEW`: registration awaiting Moderator decision.
- `ACTIVE`: may own purchasable Products and fulfill Orders.
- `REJECTED`: registration refused with a reason; terminal for that registration.
- `RESTRICTED`: new listing and new Orders limited; existing Orders continue.
- `SUSPENDED`: new listing, new Orders and new fulfillment actions blocked pending staff decision.

### Transitions, guards, and effects

- None → `PENDING_REVIEW`: verified User, unique name, within the per-User limit.
- `PENDING_REVIEW` → `ACTIVE` or `REJECTED`: Moderator decision; first accepted decision wins; rejection requires a reason.
- `ACTIVE` ↔ `RESTRICTED`, `ACTIVE`/`RESTRICTED` → `SUSPENDED` and recovery: authorized Internal Staff (UC-021, OQ-004). Existing Orders are never silently cancelled by a Shop state change.
- Invalid: a `REJECTED` registration cannot be reactivated; a new registration is created instead.

### Coverage

UC-002; NFR-001/004.

## Lifecycle models still required

- Staff intervention beyond dispute adjudication (e.g. sanctions, recovery from `RESTRICTED`/`SUSPENDED`, exceptional order lock): UC-021/022; OQ-004.
- Voucher/quota and cancellation restoration: UC-019/020/011; OQ-011.
- Logistics terminal corrections and multiple/replacement parcel models: UC-014; OQ-010/OQ-015.
- Category states are defined in [BR-CATEGORY-001](BUSINESS_RULES.md#br-category-001).

Each completed model must list states, transitions, guards, side effects, invalid actions, use cases, and acceptance coverage. Proposed transitions here await owner confirmation; file existence is not approval or gate evidence.
