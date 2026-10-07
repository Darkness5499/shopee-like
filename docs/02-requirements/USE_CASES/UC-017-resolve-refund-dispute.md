# UC-017 — Resolve a refund/dispute and simulate refund execution

## Metadata

- Status: Draft specification; dispute adjudication, return receipt inspection/restock, simulated refund execution, and reconciliation closure are **Proposed** under OQ-009/OQ-010/OQ-015.
- Owner: Project owner for product choices; analyst/assistant for specification.
- Scope: SC-020, SC-023, SC-026, SC-007; connected payment, fulfillment, inventory, and audit dependencies SC-006/SC-008/SC-012/SC-013/SC-022 in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).
- Source: [catalog](../USE_CASE_CATALOG.md), [UC-012](UC-012-process-payment.md), [UC-013](UC-013-fulfill-shop-order.md), [UC-015](UC-015-track-cancel-orders.md), [UC-016](UC-016-request-return-refund.md), [BR-REFUND-001](../BUSINESS_RULES.md#br-refund-001), [BR-STOCK-RECEIPT-001](../BUSINESS_RULES.md#br-stock-receipt-001), and the owner's 2026-10-07 WI-005 continuation request.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md); confirmation remains OQ-014.

## Goal

Provide end-to-end resolution of after-sales disputes through Seller agreement, timeout auto-approval, or Support staff adjudication; enforce physical return verification and restock controls; execute simulated payment refunds for dispute resolutions and pre-confirmation cancellation compensation obligations (`REQUIRED`); and resolve payment reconciliation exceptions under strict cumulative paid-funds caps.

## Primary actors

- Internal Staff (`Support` / Dispute Specialist) with permissions to adjudicate escalated disputes and authorize exception refunds ([OQ-005](../OPEN_DECISIONS.md#oq-005)).
- Seller / Shop Operator with permissions to accept/reject disputes and verify physical return receipts for the target Shop.

## Supporting actors

- Buyer receives dispute updates, return shipping instructions, and refund confirmations.
- Simulated Payment Provider receives simulated refund execution requests and delivers asynchronous callback evidence.
- The system enforces financial caps, monitors response deadlines, and coordinates inventory/order transitions.

## Trigger

A Seller or Support agent reviews an active dispute case, Seller confirms receipt of returned goods, system processes an unexecuted pre-confirmation compensation obligation, or Support addresses a pending payment reconciliation exception.

## Preconditions

- For **Dispute resolution**: target dispute case exists in an eligible state (`AWAITING_SELLER_RESPONSE`, `AWAITING_RETURN_RECEIPT`, or `UNDER_STAFF_REVIEW`) from [UC-016](UC-016-request-return-refund.md).
- For **Pre-confirmation compensation execution**: target Shop order has an active compensation obligation in state `REQUIRED` established by [UC-013](UC-013-fulfill-shop-order.md) or [UC-015](UC-015-track-cancel-orders.md).
- For **Payment exception disposition**: purchase group payment has reconciliation status `REQUIRED` (e.g. late payment arrived after expiry/cancellation from [UC-012](UC-012-process-payment.md)).
- Actor possesses the required role (target Shop Operator for Shop actions; Internal Support for adjudication and exception refunds).
- **Financial cap precondition:** `cumulative already-executed refunds + new refund amount <= verified successful paid funds` received for that purchase group ([BR-REFUND-001](../BUSINESS_RULES.md#br-refund-001)).

## Main flows

### Scenario 1: Execution of pre-confirmation compensation obligations

1. The system or authorized Support identifies a Shop order closed before confirmation (`CANCELLED`, `SELLER_REJECTED`, or `SELLER_TIMED_OUT`) with compensation status `REQUIRED`.
2. The system verifies the immutable snapshotted Shop payable amount and confirms that total group refund caps are satisfied.
3. The system assigns a unique, stable logical refund identity (`REFUND-###`) and dispatches a simulated refund request to the simulated Payment Provider.
4. The compensation obligation moves `REQUIRED` → `EXECUTION_PENDING` ([SM-REFUND-001](../STATE_MACHINES.md#sm-refund-001)).
5. The simulated Payment Provider returns an authentic success callback (`REFUND_SUCCEEDED`).
6. The system transitions the compensation obligation to `SUCCEEDED` and marks the Shop order `REFUNDED` ([SM-ORDER-001](../STATE_MACHINES.md#sm-order-001)).
7. An attributable audit record is logged; Buyer and Seller are notified of completed refund.

### Scenario 2: Seller dispute response and return receipt

1. Seller reviews a dispute in `AWAITING_SELLER_RESPONSE`:
   - **Path 2A (Accept Refund Only):** Seller clicks "Accept". Case moves `AWAITING_SELLER_RESPONSE` → `APPROVED_PENDING_REFUND`. System triggers simulated refund execution as in Scenario 1.
   - **Path 2B (Accept Return & Refund):** Seller clicks "Accept Return". Case moves `AWAITING_SELLER_RESPONSE` → `AWAITING_RETURN_SHIPMENT`. System generates simulated return instructions and sets Buyer return shipping deadline (candidate 5 days under OQ-010). Buyer inputs return carrier tracking; case moves to `AWAITING_RETURN_RECEIPT`.
   - **Path 2C (Reject / Dispute):** Seller clicks "Reject", submits reason and proof. Case transitions to `UNDER_STAFF_REVIEW` for Support intervention (Scenario 3).
2. For Path 2B, courier delivers return parcel to Seller. Seller performs physical inspection:
   - **Intact/resellable items:** Seller confirms receipt as intact. The system increases `on_hand` and `available` by verified quantity under [BR-STOCK-RECEIPT-001](../BUSINESS_RULES.md#br-stock-receipt-001) and [SM-INVENTORY-001](../STATE_MACHINES.md#sm-inventory-001). Case transitions `AWAITING_RETURN_RECEIPT` → `APPROVED_PENDING_REFUND`, triggering refund execution.
   - **Damaged/defective/empty parcel:** Seller records damage report with evidence; inventory is **not restocked** (`on_hand` unchanged). Case escalates to `UNDER_STAFF_REVIEW`.

### Scenario 3: Support staff adjudication

1. Support specialist opens a dispute in `UNDER_STAFF_REVIEW`, reviews Buyer claim and Seller rebuttal/evidence.
2. Specialist selects an authorized binding outcome:
   - **Full Refund:** Support approves Buyer's full requested amount. Case moves to `APPROVED_PENDING_REFUND` → refund execution dispatched.
   - **Partial Refund:** Support approves a negotiated/adjusted amount (e.g. 50% allowance). Approved amount is recorded; case moves to `APPROVED_PENDING_REFUND` → refund execution dispatched for the approved partial amount.
   - **Reject Dispute:** Support finds in Seller's favor. Case moves to `REJECTED`; Shop order transitions `DISPUTED` → `COMPLETED`. No refund is executed; consumed stock remains with Buyer.
3. System logs specialist ID, case ID, decision rationale, and outcome.

### Scenario 4: Payment reconciliation exception disposition

1. Support specialist reviews an unapplied payment exception where late funds were captured with reconciliation `REQUIRED`.
2. Specialist verifies the authentic late payment evidence and authorizes refund of the excess funds back to Buyer.
3. System verifies cumulative caps, assigns stable `REFUND-###` ID, and invokes simulated Payment Provider refund.
4. On `REFUND_SUCCEEDED`, system transitions payment reconciliation status from `REQUIRED` → `RESOLVED` ([SM-PAYMENT-001](../STATE_MACHINES.md#sm-payment-001)).
5. System logs audit evidence of resolved exception.

## Alternative and error flows

- **A1 — Unauthorized actor:** Seller attempting to adjudicate another Shop's dispute, or non-staff user attempting staff adjudication/exception refund, is refused without data mutation.
- **A2 — Seller response timeout:** if Seller does not respond before the 48-hour deadline, system automatically auto-accepts the dispute on Seller's behalf and transitions case to `APPROVED_PENDING_REFUND` (for `REFUND_ONLY`) or `AWAITING_RETURN_SHIPMENT` (for `RETURN_AND_REFUND`).
- **A3 — Buyer fails to return parcel:** if Buyer fails to enter return tracking before the return shipping deadline, dispute transitions `AWAITING_RETURN_SHIPMENT` → `REJECTED`. Order moves `DISPUTED` → `COMPLETED`.
- **A4 — Refund execution exceeds verified paid funds:** if requested refund execution would cause `total executed refunds > verified paid funds`, system halts execution, flags a financial anomaly, and alerts Senior Staff; no refund call is dispatched.
- **A5 — Simulated Payment Provider failure:** if provider returns `REFUND_FAILED`, obligation moves `EXECUTION_PENDING` → `FAILED`. System preserves error details; allows authorized retry under same refund ID or manual intervention.
- **A6 — Idempotent refund retry / callback duplicate:** if a refund request or callback is replayed with the same logical `REFUND-###` identity, system returns the established result without duplicate payment processing.
- **A7 — Damaged returned goods:** when return parcel arrives damaged or scrap, physical inspection rejects restocking under BR-STOCK-RECEIPT-001; on_hand remains unchanged. Case escalates to Support to determine whether Buyer or Seller absorbs the loss.

## Business rules

- Proposed [BR-REFUND-001](../BUSINESS_RULES.md#br-refund-001): cumulative refund caps by verified paid funds, per-Shop allocation, idempotent refund execution, reconciliation resolution.
- Proposed [BR-STOCK-RECEIPT-001](../BUSINESS_RULES.md#br-stock-receipt-001): physical return inspection required; only intact verified goods are restocked to inventory.
- Proposed [BR-AFTERSALES-001](../BUSINESS_RULES.md#br-aftersales-001): dispute adjudication paths, response deadlines, and completion effects.
- [NFR-001/002/003/004/006](../NON_FUNCTIONAL_REQUIREMENTS.md): role-based permissions, idempotent partner events, inventory ledger consistency, audit completeness, observable recovery.

## State/data changes

- [SM-DISPUTE-001](../STATE_MACHINES.md#sm-dispute-001): advances through `AWAITING_SELLER_RESPONSE` → `APPROVED_PENDING_REFUND` / `UNDER_STAFF_REVIEW` → `REFUNDED` / `REJECTED`.
- [SM-REFUND-001](../STATE_MACHINES.md#sm-refund-001): compensation/refund advances `REQUIRED` → `EXECUTION_PENDING` → `SUCCEEDED` / `FAILED`.
- [SM-ORDER-001](../STATE_MACHINES.md#sm-order-001): moves `DISPUTED` → `REFUNDED` (on full refund) or `COMPLETED` (on rejection/partial settlement). Pre-confirmation closed orders move to `REFUNDED`.
- [SM-INVENTORY-001](../STATE_MACHINES.md#sm-inventory-001): increases `on_hand` and `available` only upon authorized intact return receipt confirmation; damaged items leave counters untouched.
- [SM-PAYMENT-001](../STATE_MACHINES.md#sm-payment-001): reconciliation status moves `REQUIRED` → `RESOLVED` when excess payment refund succeeds.
- Audit log records adjudicating actor, case ID, refund ID, amounts, condition inspection notes, and timestamps.

## Postconditions

- On successful full refund: dispute case and compensation are `SUCCEEDED`/`REFUNDED`; simulated funds refunded; order closed as `REFUNDED`.
- On verified intact return: inventory counters accurately incremented; refund executed.
- On damaged return: inventory not inflated; dispute resolved by Support.
- On dispute rejection: order transitions to `COMPLETED`; no funds refunded.
- On exception resolution: reconciliation marked `RESOLVED`; audit trail complete.

## Acceptance criteria

All criteria are **Proposed** specification scenarios:

- **AC-UC-017-01 — Pre-confirmation compensation execution:** given a paid Shop order closed before confirmation with compensation `REQUIRED` of \$40, when refund is executed, system dispatches simulated refund under stable `REFUND-###` ID; on provider success, obligation becomes `SUCCEEDED` and order becomes `REFUNDED`.
- **AC-UC-017-02 — Cumulative refund cap strictly enforced:** given a purchase group with verified paid funds of \$100, where \$60 has already been refunded, an attempt to execute an additional \$50 refund is refused with a financial cap violation error; no provider call is made.
- **AC-UC-017-03 — Seller accepts refund only:** given a dispute in `AWAITING_SELLER_RESPONSE` requesting `REFUND_ONLY`, when Seller clicks Accept, case moves to `APPROVED_PENDING_REFUND` and triggers refund execution without inventory modification.
- **AC-UC-017-04 — Seller accepts return and refund:** given a dispute in `AWAITING_SELLER_RESPONSE` requesting `RETURN_AND_REFUND`, when Seller clicks Accept Return, case moves to `AWAITING_RETURN_SHIPMENT` with return instructions and deadline.
- **AC-UC-017-05 — Seller response timeout auto-approval:** given a dispute in `AWAITING_SELLER_RESPONSE` with 48h deadline elapsed, system automatically auto-accepts the request on Seller's behalf, transitioning to `APPROVED_PENDING_REFUND` or `AWAITING_RETURN_SHIPMENT`.
- **AC-UC-017-06 — Buyer return shipping deadline expiry:** given a case in `AWAITING_RETURN_SHIPMENT`, when Buyer fails to input return tracking within deadline, case transitions to `REJECTED` and Shop order transitions `DISPUTED` → `COMPLETED`.
- **AC-UC-017-07 — Verified intact return restock:** given returned goods physically received by Seller, when Seller verifies goods are intact and complete, system increments `on_hand` and `available` by returned quantity under SM-INVENTORY-001, and case moves to `APPROVED_PENDING_REFUND`.
- **AC-UC-017-08 — Damaged return restock blocked:** given returned goods physically received damaged or broken, Seller records damage inspection; system prevents inventory increment (`on_hand` unchanged), and case escalates to `UNDER_STAFF_REVIEW`.
- **AC-UC-017-09 — Support full refund adjudication:** given an escalated dispute in `UNDER_STAFF_REVIEW`, when Support specialist approves full refund, case moves to `APPROVED_PENDING_REFUND` and executes full refund under BR-REFUND-001.
- **AC-UC-017-10 — Support partial refund adjudication:** given an escalated dispute in `UNDER_STAFF_REVIEW`, Support specialist approves a partial refund of \$15 on a \$40 order; system records approved partial amount, executes \$15 refund, and moves order to `COMPLETED`.
- **AC-UC-017-11 — Support reject dispute adjudication:** given an escalated dispute in `UNDER_STAFF_REVIEW`, when Support specialist finds evidence fraudulent and rejects dispute, case transitions to `REJECTED`, order moves `DISPUTED` → `COMPLETED`, and no refund occurs.
- **AC-UC-017-12 — Payment reconciliation exception resolution:** given a group payment with reconciliation `REQUIRED` due to late payment of \$30, when Support authorizes refund of unapplied funds, system executes simulated refund and transitions reconciliation `REQUIRED` → `RESOLVED`.
- **AC-UC-017-13 — Provider refund failure recovery:** given simulated Payment Provider returns `REFUND_FAILED`, obligation moves to `FAILED` with error preserved; order remains pending resolution; subsequent authorized retry is permitted under same refund ID.
- **AC-UC-017-14 — Idempotent refund execution:** given a refund already completed, replaying execution with the same `REFUND-###` identity returns the established success outcome without duplicate provider transactions.
- **AC-UC-017-15 — Unauthorized staff action refused:** a Buyer or standard Seller attempting to access Support adjudication endpoints is refused with authorization error; no dispute decision or refund occurs.
- **AC-UC-017-16 — Unauthorized seller dispute decision refused:** a Seller attempting to accept or reject a dispute belonging to a different Shop is refused; case state remains unchanged.
- **AC-UC-017-17 — Sibling Shop independence:** executing a refund on Shop Order A does not affect sibling Shop Order B's order state, inventory counters, or payable allocation.
- **AC-UC-017-18 — Multi-order group refund sum:** in a two-Shop purchase group where both orders are cancelled and compensated, each has its own compensation obligation, and their combined executed refunds equal the sum of both Shop payables, matching group paid funds exactly.
- **AC-UC-017-19 — Attributable audit logging:** every dispute adjudication, restock confirmation, refund dispatch, and reconciliation resolution logs actor ID, role, case ID, refund ID, amounts, condition inspection evidence, and timestamps.
- **AC-UC-017-20 — No double compensation:** a Shop order cancelled pre-confirmation with compensation `REQUIRED` cannot subsequently have a dispute opened or a second compensation obligation generated for that same order.
- **AC-UC-017-21 — Courier undelivered return restock:** given courier returns undelivered parcel to Seller (`DELIVERY_EXCEPTION`), Seller inspects parcel: intact goods are restocked via stock-in; transit-damaged goods remain unrestocked.
- **AC-UC-017-22 — Simulated boundary:** all refund dispatches and carrier return receipts operate against simulated mocks without real money disbursement or commercial courier API calls.

Criteria AC-UC-017-01/02/10/12/14/18 relate to BR-REFUND-001 and SM-REFUND-001/PAYMENT-001. AC-UC-017-03/04/05/06/09/11/20 relate to BR-AFTERSALES-001 and SM-DISPUTE-001/ORDER-001. AC-UC-017-07/08/21 relate to BR-STOCK-RECEIPT-001 and SM-INVENTORY-001. AC-UC-017-15/16/17/19 relate to NFR-001/004. AC-UC-017-13/22 relate to NFR-002/006.

## Traceability

- SC-020, SC-023, SC-026, supported by SC-006/SC-007/SC-008/SC-012/SC-013/SC-022 → UC-017 → BR-REFUND-001/STOCK-RECEIPT-001/AFTERSALES-001/CANCEL-001, SM-DISPUTE-001/REFUND-001/ORDER-001/INVENTORY-001/PAYMENT-001, NFR-001/002/003/004/006 → AC-UC-017-01 through AC-UC-017-22. Canonical coverage belongs in [Traceability](../TRACEABILITY.md).
- Specifications are Draft requirements; no implementation or test execution is claimed.

## Open questions

- [OQ-010](../OPEN_DECISIONS.md#oq-010): delivery completion windows, return inspection standards, staff dispute authority, and appeal processes.
- [OQ-005](../OPEN_DECISIONS.md#oq-005): granular Support and Dispute specialist permissions.
- [OQ-015](../OPEN_DECISIONS.md#oq-015): partner refund event identity, callback signatures, and idempotent reconciliation.
- [OQ-011](../OPEN_DECISIONS.md#oq-011): partial voucher allocation recovery during partial refunds.
- [OQ-014](../OPEN_DECISIONS.md#oq-014): MVP prioritization confirmation.

