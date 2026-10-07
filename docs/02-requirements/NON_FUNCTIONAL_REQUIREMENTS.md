# Non-Functional Requirements

- Status: Draft; numerical thresholds and new verification policies are Proposed.
- Owner: Project owner.
- Source: [Functional Scope](../01-product/FUNCTIONAL_SCOPE.md) and [Product Vision](../01-product/PRODUCT_VISION.md).
- Decisions: [OQ-012](OPEN_DECISIONS.md#oq-012) tracks pending confirmation of proposed demo quality targets; related product decisions remain explicit below.

These are observable quality requirements, not framework, infrastructure, or API selections. Some safeguards refine baseline capabilities; their proposed measurements do not imply existing acceptance or executed tests.

<a id="nfr-001"></a>
## NFR-001 — Authorization and Shop isolation

- Basis: Baseline granular Shop/internal permissions (SC-009/SC-023); verification threshold Proposed.
- Requirement: only actors with the required permission and Shop scope may read/change protected Shop data or perform a restricted staff action.
- Measurement: zero unauthorized successes in a defined permission matrix, including two distinct Shops, owner/operator roles, and Admin/Moderator/Support/Operations allow/deny cases.
- Scope: UC-001/002/003/004/005/006/007/013/017/019/020/021/022/023; Proposed extension UC-010/011/012/014/015 for private cart/purchase/payment/tracking access.
- Rationale: a User operating several Shops must not acquire rights to unrelated Shops; Admin is not implicitly the default reviewer.
- Verification: positive and negative actor/action/resource scenarios after the matrix is defined under OQ-005. Permission denial must leave protected data unchanged.
- Proposed cart extension: include two distinct Buyers and deny reading/changing the other Buyer's private cart under AC-UC-010-02. Shop operating permission alone does not grant access to a Buyer's cart; exact account/cart authorization remains OQ-005.
- Proposed purchase extension: AC-UC-007-04, AC-UC-011-02 and AC-UC-012-02 cover wrong-Shop inventory and wrong-Buyer checkout/payment access. Each Shop operator sees only their permitted Shop-order details, without sibling-Shop private records or unrestricted group data. Exact matrix remains OQ-005. UC-013/014/015 add Shop/action, Buyer-owned view/cancel/recovery and simulator/query denial scenarios with protected delivery-address and sibling data boundaries; these criteria do not settle the allow matrix.

<a id="nfr-002"></a>
## NFR-002 — Safe asynchronous partner outcomes

- Basis: Baseline signature verification, idempotency, retries/reconciliation, and duplicate/late/out-of-order handling (SC-006/SC-025/SC-026); experiment counts Proposed.
- Requirement: accept only verified payment outcomes; repeated logical events must not repeat payment, stock, refund, or shipment effects. Valid late/out-of-order events are handled according to approved transition policy.
- Measurement: replay the same logical event 10 times, including concurrent delivery; observe one effective transition and each defined side effect once. Invalid-signature scenarios cause zero payment changes. Replay permitted reordered/late sequences with zero forbidden transitions.
- Scope: UC-012/014/017 and related stock/order effects.
- Rationale: partner retry is expected and must not duplicate business effects.
- Verification: simulated partner scenarios comparing final states, amounts, stock, and audit records. Logical identity, conflict resolution, and terminal-state behavior must first be resolved in OQ-015/OQ-008/OQ-010.
- Draft scenario coverage: AC-UC-012-04/05/08 through 16/23 links authenticity/correlation, repeated/conflicting events, deadline races and interrupted processing to Proposed BR-PAY-001/SM-PAYMENT-001. UC-014 adds authenticated Shipment creation/event identity, contiguous sequence/gap recovery, equivalent no-op evidence, conflicts and paired Order progress; UC-015 adds unpaid-cancel versus success/late evidence. No replay experiment has been executed.

<a id="nfr-003"></a>
## NFR-003 — Inventory consistency under contention

- Basis: Baseline non-negative inventory, reserve/release/deduct lifecycle (SC-012); consistency invariant/experiment Proposed.
- Requirement proposal: quantities remain non-negative and outstanding reservations never exceed physical stock; each release or deduction takes effect once according to the agreed lifecycle.
- Measurement proposal: for one available SKU unit and two simultaneous checkout attempts, at most one reservation succeeds; after failure/expiry/confirmation experiments, stock and reservation totals equal the expected ledger of business changes.
- Scope: UC-007/011/012/013/015/017.
- Rationale: concurrent purchase attempts must not oversell; payment success alone is not assumed to deduct stock.
- Verification: contention and lifecycle replay scenarios using the definitions and timing confirmed under OQ-008/OQ-009/OQ-010. Concurrent adjustment limits and exact reservation formula remain open.
- Proposed accounting coverage: BR-INV-001/SM-INVENTORY-001/SM-RESERVATION-001 and AC-UC-007-02/06 through 15, AC-UC-011-04/08/09/12, AC-UC-012-03/06 through 10/23/24. Payment converts unpaid to paid holds without deduction; Seller confirmation decreases on_hand and reserved together. Deadline crossing alone cannot double-release quantity. UC-013/015 propose confirmation/closure races and complete active-paid release/compensation without physical deduction; after-confirmation restock remains incomplete. OQ-009 duration/closure choices remain Open.

<a id="nfr-004"></a>
## NFR-004 — Audit completeness

- Basis: Baseline actor, action, changed data, and timestamp (SC-022); coverage/verification details Proposed.
- Requirement: auditable business actions preserve those four elements; rejection includes its reason under BR-PROD-001. Under Accepted OQ-003, each moderation decision links to the Product and the exact immutable content set reviewed.
- Measurement proposal: 100% of a defined action inventory has an attributable audit record. Initial inventory: Product content/operational edits, submission, review, stock adjustment/reservation changes, payment/shipment outcomes, permission changes, and refund/staff intervention decisions.
- Scope: UC-024 and all affected detailed use cases; Draft criterion coverage UC-004/005/006/007/011/012/013/014/015. Executed verification is pending.
- Rationale: explain business changes and diagnose simulated asynchronous flows.
- Verification: inspect before/after business data and corresponding records; rejected/denied actions are distinguishable from successful changes if included in the agreed audit inventory.
- Open details: retention, viewer permissions, sensitive-data redaction, and failed-attempt coverage under OQ-005/OQ-012. No production compliance claim is made.
- Draft purchase inventory: AC-UC-007-16, AC-UC-011-15 and AC-UC-012-20 correlate accepted inventory/checkout/payment effects and distinguish callback delivery/diagnostics from a repeated effective business change. Callback secrets are excluded; address/payment-data access/redaction is not settled by drafting these criteria.

<a id="nfr-005"></a>
## NFR-005 — Interactive response time for the demo

- Status: Proposed.
- Requirement proposal: product discovery/detail responds within a p95 system response time of 2 seconds, excluding user think time and simulated-partner waiting.
- Measurement proposal: 100 Shops, 10,000 active SKUs, 20 concurrent users, a 1-minute warmup then a 5-minute measurement; report discovery and detail results separately and record the chosen environment.
- Scope: UC-008/009. Checkout/payment timing needs separate targets once OQ-007/OQ-008/OQ-015 are resolved.
- Rationale: demonstrate usable buyer discovery at a bounded learning workload.
- Verification: repeatable load experiment in Phase 6; confirm target/workload under OQ-012. Hardware/runtime are recorded when chosen in Phase 4, not implied here.

<a id="nfr-006"></a>
## NFR-006 — Observable recovery of simulated partner processing

- Basis: Baseline retry/reconciliation capability and learning goal of basic observability; recovery threshold Proposed.
- Requirement proposal: accepted partner outcomes are not lost after interruption; pending/failed processing can be correlated to its Payment Request or Shipment and inspected for reconciliation.
- Measurement proposal: interrupt processing after acceptance and before the business effect; after restart, recover the intended effect within 60 seconds in the agreed demo environment, with no duplicated side effects.
- Scope: UC-012/014/017.
- Rationale: asynchronous failures must be explainable and recoverable during demonstrations.
- Verification: controlled interruption/restart/reconciliation scenarios. Accepted-event meaning, retry limits, terminal failures, correlation, and environment are unresolved under OQ-015/OQ-012; no queue, storage mechanism, or service topology is prescribed.
- Draft recovery coverage: AC-UC-012-13 through 18 distinguishes lost initiation/success responses, receipt-only interruption and recovery before/after expiry. Receipt of verified success alone does not guarantee later paid application: recovery rechecks current holds/deadline and exposes reconciliation when the purchase is no longer eligible. The proposed 60-second measurement concerns the correct permitted recovery outcome, including an exception, rather than forced fulfillment after expiry. UC-013/015 add interrupted Seller/cancellation outcomes rechecked against current deadlines/holds and complete compensation; UC-014 adds unknown creation, receipt-only progress and gap/path recovery. Numeric targets and final disposition remain Proposed/unconfirmed.

## Coverage still to specify

Availability expectations, media/data limits, accessibility/usability goals, backup/restore, and additional performance targets require owner prioritization if they become demo requirements. This initial NFR draft is not a complete quality specification or a production SLA.
