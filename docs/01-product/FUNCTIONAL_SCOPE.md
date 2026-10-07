# Shopee-like Demo — Core Functional Scope

- Status: Baseline — existing capabilities retained; identifiers and actor terminology normalized on 2026-10-07.
- Owner: Project owner.
- Priority: proposed in the [use case catalog](../02-requirements/USE_CASE_CATALOG.md), not implied by document order.

## Product intent

Build a multi-category marketplace demo inspired by Shopee. The project prioritizes complex end-to-end business flows and architectural learning, not production-scale commercial operation.

## Participants

- **Buyer:** a User acting in the purchasing role.
- **Seller / Shop Operator:** a User authorized to own or operate one or more Shops. A Shop is an entity, not an actor.
- **Internal Staff:** internal roles such as Admin, Moderator, Support, and Operations.
- **External Partners:** Payment Provider and Logistics / Delivery Partner.

One User account can be both a Buyer and a Seller / Shop operator.

Actor responsibilities are canonical in [Stakeholders and Actors](STAKEHOLDERS_AND_ACTORS.md); terms are canonical in the [glossary](../02-requirements/GLOSSARY.md).

## Core capabilities

### Buyer

- **SC-001 — Account and addresses:** register, sign in, verify account, and manage profile and delivery addresses.
- **SC-002 — Product discovery:** browse multi-level categories; search and filter by category attributes, price, rating, and shop.
- **SC-003 — Product detail:** view products with variants/SKUs, inventory, reviews, vouchers, and shop policies.
- **SC-004 — Shopping cart:** manage a multi-shop shopping cart.
- **SC-005 — Checkout:** split cart items into orders per shop, calculate shipping fees, apply vouchers, and validate inventory.
- **SC-006 — Simulated payment:** create a payment request, receive asynchronous callback/webhook, and prevent duplicate payment processing.
- **SC-007 — Order lifecycle:** track pending confirmation, packed, shipped, delivered, cancelled, returned, and refunded outcomes. Detailed requirements will distinguish order, payment, shipment, and refund states; these are not all one state field.
- **SC-008 — Feedback and support:** submit product/shop reviews and create refund or dispute requests.

### Seller / Shop Operator

- **SC-009 — Shop and staff:** register a Shop; manage multiple shop staff with distinct permissions.
- **SC-010 — Category-aware products:** manage products using dynamic category-specific attributes.
- **SC-011 — Product and SKU maintenance:** manage variants, SKUs, price, product media, and product status.
- **SC-012 — Inventory:** stock-in and adjustments; reserve inventory at checkout; release reservations on payment failure or expiry; deduct inventory when the order is confirmed.
- **SC-013 — Fulfillment:** confirm orders, reject where allowed, pack, and hand over to delivery.
- **SC-014 — Shop promotions:** create shop vouchers and promotions with eligibility rules, quota, and validity windows.
- **SC-015 — Shop reporting:** view revenue, orders, top products, and inventory change history.

### Internal Staff

- **SC-016 — Moderation:** moderate Shops and Products through review, revision-requested, approved, rejected, hidden, or locked outcomes. Product approval sets the Product to `ACTIVE` under the existing rule; approval is a review outcome rather than an additional Product state. Shop moderation and other intervention states need detailed specification.
- **SC-017 — Category administration:** manage hierarchical categories and dynamic attribute schemas by product category.
- **SC-018 — Violations:** warn, restrict, suspend, or lock Users/Shops according to policy.
- **SC-019 — Order intervention:** search and intervene in orders according to staff permissions.
- **SC-020 — Refunds and disputes:** intake, evidence collection, decision, refund execution, and closure.
- **SC-021 — Platform promotions:** manage platform-level vouchers and promotional programs.
- **SC-022 — Audit:** maintain audit logs recording actor, action, changed data, and timestamp.
- **SC-023 — Internal permissions:** enforce granular internal roles: Admin, Moderator, Support, and Operations.

### Logistics / Delivery Partner

- **SC-024 — Shipment creation:** receive shipment-creation requests for orders ready for delivery.
- **SC-025 — Shipment tracking:** publish awaiting-pickup, picked-up, in-transit, delivered, failed-delivery, and returned events; handle duplicate, late, and out-of-order events; synchronize shipment status with the order lifecycle.

### Payment Provider

- **SC-026 — Simulated provider:** create payment requests; simulate success, failure, and expiration; send result webhooks; support signature verification, idempotency, retries, and reconciliation with order payment state.

## Areas to develop in depth

- Catalog, variants / SKUs, and inventory consistency.
- Multi-shop checkout.
- Order, payment, and shipment state machines.
- Voucher and promotion rule engine.
- Moderation workflow.
- Multi-tenant / shop authorization.
- Webhooks, idempotency, retries, and asynchronous event processing.
- Audit logging and observability.

Scope identifiers are stable traceability anchors. A capability is not fully specified merely because it appears here; coverage is recorded in [TRACEABILITY.md](../02-requirements/TRACEABILITY.md).
