# UC-016 — Request a return, refund, or dispute

## Metadata

- Status: Draft specification; dispute intake windows, grounds, requested amount caps, and Seller response deadlines are **Proposed** under OQ-010.
- Owner: Project owner for product choices; analyst/assistant for specification.
- Scope: SC-008, SC-007, SC-020; connected fulfillment, tracking, inventory and financial dependencies SC-013/SC-014/SC-022/SC-023/SC-026 in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).
- Source: [catalog](../USE_CASE_CATALOG.md), [UC-014](UC-014-simulate-shipment.md), [UC-015](UC-015-track-cancel-orders.md), [BR-AFTERSALES-001](../BUSINESS_RULES.md#br-aftersales-001), and the owner's 2026-10-07 WI-005 continuation request.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md); confirmation remains OQ-014.

## Goal

Enable Buyer to open an after-sales dispute case (requesting Return & Refund or Refund Only) for an eligible Shop order with specified reasons, requested refund amounts, and supporting evidence. Establish stable dispute case identity, freeze auto-completion on the order, notify the Seller with a binding response deadline, and allow Buyer to track or withdraw the dispute without duplicate requests.

## Primary actor

Buyer acting through the User account that owns the target purchase group and Shop order.

## Supporting actors

- Seller / Shop Operator receives dispute notification, inspects Buyer evidence, and responds within deadline via [UC-017](UC-017-resolve-refund-dispute.md).
- Internal Staff (Support / Dispute Specialist) receives escalated disputes when Seller rejects or when complex exceptions arise ([UC-017](UC-017-resolve-refund-dispute.md)).
- The system verifies eligibility and windows, enforces caps, manages deadlines, and recovers interrupted requests idempotently.

## Trigger

Buyer opens an eligible delivered or delivery-exception Shop order and submits a request for Return & Refund or Refund Only.

## Preconditions

- The Shop order exists and is owned by Buyer; Buyer ownership is verified before displaying after-sales options or accepting requests.
- The Shop order is in an eligible lifecycle state under [BR-AFTERSALES-001](../BUSINESS_RULES.md#br-aftersales-001):
  - `DELIVERED`: physical delivery confirmed, strictly within the 7-day after-sales window (candidate duration under [OQ-010](../OPEN_DECISIONS.md#oq-010)).
  - `DELIVERY_EXCEPTION`: courier failed delivery and returned parcel to seller (`RETURNED` in [SM-SHIPMENT-001](../STATE_MACHINES.md#sm-shipment-001)).
  - `SHIPPED`: parcel confirmed lost in transit or delivery elapsed beyond maximum carrier tolerance.
- The Shop order is not already `COMPLETED` (unless exceptional staff reopening applies under OQ-004), `CANCELLED`, `SELLER_REJECTED`, `SELLER_TIMED_OUT`, or `REFUNDED`.
- Exactly **no active dispute case** exists for this Shop order. A previous withdrawn or closed dispute cannot be duplicated if the return window has expired.
- The requested refund amount is positive, in the order currency, and does not exceed the accepted Shop payable amount (item subtotal + attributed shipping - attributed discounts) from the original purchase snapshot.

## Main flow

1. Buyer navigates to an owned Shop order in `DELIVERED` or `DELIVERY_EXCEPTION` status and selects "Request Return / Refund".
2. The system checks Buyer ownership, verifies that the order is within the eligible after-sales window, and confirms that no active dispute case exists.
3. The system presents the dispute intake form with eligible request types:
   - `RETURN_AND_REFUND`: available for `DELIVERED` orders where physical goods were received (e.g., damaged item, defective, wrong product, missing accessories).
   - `REFUND_ONLY`: available for non-receipt (`DELIVERY_EXCEPTION`, lost in transit) or unusable/perishable items where return shipment is impossible or economically unviable.
4. Buyer selects the dispute type, chooses a predefined reason code, enters a narrative description, attaches simulated evidence files (e.g. photos/video descriptions), and inputs the requested refund amount (up to the snapshotted Shop payable cap).
5. Buyer submits the dispute request with a stable logical operation identity.
6. The system checks the operation identity: for an exact repeat, it returns the existing case record and progress; for a new submission, it validates mandatory inputs, reason code, evidence presence, and amount bounds.
7. The system creates a new dispute case record with a stable identity (`DISP-###`), links it to the Shop order, and sets initial state `SUBMITTED` ([SM-DISPUTE-001](../STATE_MACHINES.md#sm-dispute-001)).
8. Atomically, the system transitions the Shop order lifecycle to `DISPUTED` and **freezes the auto-completion timer** on that Shop order under [BR-AFTERSALES-001](../BUSINESS_RULES.md#br-aftersales-001). Sibling Shop orders remain unaffected.
9. The system sets the Seller response deadline (candidate 48 hours from submission under OQ-010) and transitions the dispute case `SUBMITTED` → `AWAITING_SELLER_RESPONSE`.
10. The system logs an attributable audit record (actor, case ID, order ID, requested type, amount, reason, deadline, timestamp) and returns the confirmed dispute details and tracking progress to Buyer.
11. Buyer can monitor dispute status, view Seller response progress, or withdraw the case while awaiting decision.

## Alternative and error flows

- **A1 — Unauthorized or wrong Buyer:** if actor does not own the Shop order, refuse access without disclosing order or seller information.
- **A2 — Ineligible order status:** if order is `PAYMENT_PENDING`, `PAID_AWAITING_CONFIRMATION`, `CONFIRMED`, or `PACKED`, refuse dispute submission; ordinary pre-confirmation cancellation ([UC-015](UC-015-track-cancel-orders.md)) or waiting for delivery must be used.
- **A3 — Window expired on completed order:** if order is already `COMPLETED` or more than 7 days have elapsed since `DELIVERED` timestamp, refuse submission. Direct Buyer to contact Support for exceptional intervention under OQ-004.
- **A4 — Active dispute already exists:** if a dispute case is currently in `AWAITING_SELLER_RESPONSE`, `AWAITING_RETURN_SHIPMENT`, `AWAITING_RETURN_RECEIPT`, or `UNDER_STAFF_REVIEW`, refuse creation of a second case; return the current active case.
- **A5 — Invalid requested amount:** if requested amount is <= 0 or exceeds the snapshotted Shop payable amount, refuse submission with specific boundary validation error; do not create a case.
- **A6 — Missing required evidence or reason:** if mandatory reason code, description, or proof attachments are omitted for a damage/defect claim, reject submission with input validation failure.
- **A7 — Buyer withdraws dispute:** while in `AWAITING_SELLER_RESPONSE` or `AWAITING_RETURN_SHIPMENT`, Buyer may explicitly submit a withdrawal request. System verifies ownership, marks case `WITHDRAWN`, and unfreezes the Shop order auto-completion timer. If remaining delivery window is < 24 hours, a grace period of 24 hours is applied before auto-completion.
- **A8 — Repeat submission or network retry:** if Buyer resubmits using the same logical operation identity and matching instructions, return the already created case details without creating duplicate cases or resetting deadlines.
- **A9 — Seller response timeout:** if Seller fails to respond before the 48-hour response deadline, system automatically accepts the dispute on Seller's behalf and transitions case to `APPROVED_PENDING_REFUND` (for `REFUND_ONLY`) or `AWAITING_RETURN_SHIPMENT` (for `RETURN_AND_REFUND`) in UC-017.

## Business rules

- Proposed [BR-AFTERSALES-001](../BUSINESS_RULES.md#br-aftersales-001): delivery completion vs dispute windows, dispute types, auto-completion freeze, Seller response deadline.
- Proposed [BR-REFUND-001](../BUSINESS_RULES.md#br-refund-001): maximum refund amount capped at snapshotted Shop payable amount; per-Shop liability.
- Proposed [BR-ORDER-001](../BUSINESS_RULES.md#br-order-001): immutable purchase snapshot facts.
- [NFR-001/004/006](../NON_FUNCTIONAL_REQUIREMENTS.md): Buyer privacy, complete audit logging, observable idempotent retry.

## State/data changes

- [SM-ORDER-001](../STATE_MACHINES.md#sm-order-001): target Shop order transitions `DELIVERED` / `DELIVERY_EXCEPTION` → `DISPUTED`. Auto-completion countdown is frozen. Sibling Shop orders remain unchanged.
- [SM-DISPUTE-001](../STATE_MACHINES.md#sm-dispute-001): new case created in `SUBMITTED`, then moves to `AWAITING_SELLER_RESPONSE` with recorded 48h deadline.
- Inventory is unchanged: no inventory holds or physical stock changes occur on dispute intake.
- Payment is unchanged: no refund execution happens at intake.
- Audit log records case ID, order ID, buyer actor, type, requested amount, reason, evidence metadata, and timestamp.

## Postconditions

- On successful submission: a unique dispute case is created; order is marked `DISPUTED`; auto-completion is suspended; Seller response deadline is established; Buyer can track progress.
- On withdrawal: dispute is marked `WITHDRAWN`; order resumes normal lifecycle countdown toward `COMPLETED`.
- On refused request: order state, inventory, and payment remain untouched; no dispute case is created.

## Acceptance criteria

All criteria are **Proposed** specification scenarios:

- **AC-UC-016-01 — Eligible delivered order intake:** given an owned Shop order in `DELIVERED` status within 7 days of delivery and no active dispute, when Buyer submits a valid `RETURN_AND_REFUND` request with reason, evidence, and valid amount, system creates a dispute case in `AWAITING_SELLER_RESPONSE`, sets order to `DISPUTED`, and suspends auto-completion.
- **AC-UC-016-02 — Eligible delivery exception intake:** given an owned Shop order in `DELIVERY_EXCEPTION` status (courier returned to seller), when Buyer submits a valid `REFUND_ONLY` request, system creates a dispute case in `AWAITING_SELLER_RESPONSE` and freezes order completion.
- **AC-UC-016-03 — Unauthorized Buyer rejected:** given another Buyer's delivered order, an unauthorized Buyer cannot view after-sales options or submit a dispute; no case is created and no order state changes.
- **AC-UC-016-04 — Ineligible pre-delivery states:** given an order in `PAID_AWAITING_CONFIRMATION`, `CONFIRMED`, `PACKED`, or normal in-transit `SHIPPED`, dispute submission is refused; ordinary pre-confirmation cancellation or awaiting delivery is required.
- **AC-UC-016-05 — Window expired on completed order:** given a Shop order `COMPLETED` past the 7-day delivery window, Buyer dispute submission is refused with an eligibility expired message; no dispute case is opened.
- **AC-UC-016-06 — Single active dispute constraint:** given an existing dispute case in `AWAITING_SELLER_RESPONSE` for a Shop order, a second dispute submission on that same order is refused; existing case is returned without duplication.
- **AC-UC-016-07 — Maximum amount cap:** given a Shop order with snapshotted payable amount of \$50, when Buyer requests a refund of \$55, submission is refused with amount exceeded validation error; no case is created.
- **AC-UC-016-08 — Partial refund requested amount:** given a Shop order with snapshotted payable amount of \$50, Buyer may submit a valid request for \$20 (e.g. minor damage allowance); requested amount of \$20 is recorded on the case.
- **AC-UC-016-09 — Mandatory reason and evidence:** given a defect/damage dispute submission missing required reason code or photo description, system refuses submission with validation errors; order remains in `DELIVERED`.
- **AC-UC-016-10 — Sibling Shop isolation:** in a multi-shop purchase group, opening a dispute on Shop Order A marks only Order A as `DISPUTED`; sibling Shop Order B in `DELIVERED` status continues its independent auto-completion countdown.
- **AC-UC-016-11 — Submission idempotency:** given a dispute submitted with a unique operation identity, replaying the exact submission returns the created dispute case without generating a duplicate ID or resetting the 48h Seller deadline.
- **AC-UC-016-12 — Buyer withdrawal of dispute:** given an active dispute in `AWAITING_SELLER_RESPONSE`, when Buyer explicitly withdraws the dispute, case transitions to `WITHDRAWN`, order transitions `DISPUTED` → `DELIVERED`, and auto-completion countdown resumes with at least 24h grace period.
- **AC-UC-016-13 — Withdrawal blocked in later stages:** given a dispute that has moved to `APPROVED_PENDING_REFUND` or `REFUNDED`, Buyer withdrawal is refused; established resolution cannot be retracted.
- **AC-UC-016-14 — Seller deadline recording:** when dispute enters `AWAITING_SELLER_RESPONSE`, system records a strict 48-hour response deadline timestamp calculated from submission time; refreshing or viewing the case does not extend the deadline.
- **AC-UC-016-15 — Attributable audit logging:** upon dispute creation or withdrawal, system logs actor ID, order ID, case ID, action type, amount, timestamps, and before/after states; sensitive partner credentials are not logged.
- **AC-UC-016-16 — Simulated boundary:** all evidence uploads and after-sales tracking operate within the simulated demo environment without invoking external customer-support CRM platforms.

Criteria AC-UC-016-01/02/04/05/10 relate to BR-AFTERSALES-001, SM-ORDER-001 and SM-DISPUTE-001. AC-UC-016-03/15 relate to NFR-001/004. AC-UC-016-06/07/08/09 relate to BR-AFTERSALES-001/REFUND-001. AC-UC-016-11/12/13/14 relate to SM-DISPUTE-001 and NFR-006.

## Traceability

- SC-008, supported by SC-007/SC-020/SC-022/SC-023 → UC-016 → BR-AFTERSALES-001/REFUND-001/ORDER-001, SM-ORDER-001/DISPUTE-001, NFR-001/004/006 → AC-UC-016-01 through AC-UC-016-16. Canonical coverage belongs in [Traceability](../TRACEABILITY.md).
- Detailed resolution and refund execution belong to [UC-017](UC-017-resolve-refund-dispute.md).
- Specifications are Draft requirements; no implementation or test execution is claimed.

## Open questions

- [OQ-010](../OPEN_DECISIONS.md#oq-010): delivery completion windows, return evidence standards, dispute escalation criteria.
- [OQ-005](../OPEN_DECISIONS.md#oq-005): Buyer and Seller after-sales permission boundaries.
- [OQ-011](../OPEN_DECISIONS.md#oq-011): voucher benefit restoration on partial versus full dispute.
- [OQ-014](../OPEN_DECISIONS.md#oq-014): MVP prioritization confirmation.

