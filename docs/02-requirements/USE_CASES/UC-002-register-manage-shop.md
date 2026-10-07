# UC-002 — Register and manage a Shop and its operators

- Implementation status: Not implemented — deferred; closed for current work. Resume only when the owner selects a later part.
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; Shop lifecycle, roles and permission matrix are Proposed under [OQ-004](../OPEN_DECISIONS.md#oq-004) and [OQ-005](../OPEN_DECISIONS.md#oq-005).
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-009, SC-016, SC-023, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Let a verified User register a Shop, obtain Moderator approval, and delegate bounded Shop permissions to operators, so every Shop-owned action in the other use cases has a defined owner and scope.

## Primary actor

Seller (a verified User becoming a Shop owner).

## Supporting actors

Moderator (Shop registration review); Shop operators (invited Users); Admin (internal role/permission administration, outside Shop-level permissions).

## Trigger

A verified User submits a Shop registration, or an owner invites, changes or removes an operator.

## Preconditions

- The registering User is `ACTIVE` ([UC-001](UC-001-manage-account-addresses.md)).
- Shop name uniqueness and per-User Shop limits are Proposed in [BR-SHOP-001](../BUSINESS_RULES.md#br-shop-001).
- The invited operator is an existing `ACTIVE` User.

## Main flow

1. The User submits the Shop name, description and contact data. The system validates it and creates the Shop in `PENDING_REVIEW` ([SM-SHOP-001](../STATE_MACHINES.md#sm-shop-001)) with the User as its single `OWNER`.
2. A Moderator reviews the registration. Approval moves the Shop to `ACTIVE`; rejection requires a reason and moves it to `REJECTED`.
3. The `OWNER` of an `ACTIVE` Shop invites an operator and assigns one Shop role ([BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001) role/permission matrix). The invitee accepts; the grant takes effect only for that Shop.
4. The `OWNER` may change an operator's role or remove the operator. Effects apply to subsequent actions; accepted business effects stay attributable to the actor who performed them.
5. The system records attributable audit entries for registration, review, role grants and removals.

## Alternative and error flows

- **A1 — Rejected registration:** the Shop is `REJECTED` with a reason; the User may correct and submit a new registration. A rejected Shop cannot sell or hold Shop-owned Products.
- **A2 — Duplicate or over-limit registration:** a duplicate Shop name or exceeding the per-User Shop limit is refused with no Shop created.
- **A3 — Unverified User:** registration is refused until [UC-001](UC-001-manage-account-addresses.md) verification succeeds.
- **A4 — Pending-review actions:** a `PENDING_REVIEW` Shop cannot create purchasable Products or accept Orders; Shop-owned data reads remain limited to the `OWNER`.
- **A5 — Concurrent review decisions:** the first accepted Moderator decision wins; later conflicting decisions are refused and repeats return the existing outcome ([BR-PROD-003](../BUSINESS_RULES.md#br-prod-003) behavior is mirrored for Shop review as a Proposed policy).
- **A6 — Permission denied or wrong Shop:** an actor without the permission for the target Shop is refused with no data change; holding a role in one Shop gives no right in another.
- **A7 — Owner protection:** the `OWNER` cannot be removed or demoted by an operator; ownership transfer is out of scope for the first baseline.
- **A8 — Removed operator with pending work:** removal revokes new actions immediately; in-flight accepted actions complete and remain attributable; Shop-level deadlines (for example Seller confirmation) continue to run against the Shop, not the removed operator.
- **A9 — Restricted/suspended Shop:** [SM-SHOP-001](../STATE_MACHINES.md#sm-shop-001) guards block new listing/fulfillment as Proposed; handling of existing Orders belongs to [UC-021](../USE_CASE_CATALOG.md#uc-021--handle-user-and-shop-violations) and OQ-004.
- **A10 — Repeated request:** an identical invitation, role change or review action returns the existing outcome with no duplicate grant or effect.

## Business rules

[BR-SHOP-001](../BUSINESS_RULES.md#br-shop-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [SM-SHOP-001](../STATE_MACHINES.md#sm-shop-001). Product moderation stays in [UC-006](UC-006-review-product.md) and is not reused as the Shop state machine.

## State/data changes

Creates Shop, role grants and review decisions. Shop `OWNER` and operator roles are scoped to one Shop and carried into every Shop-owned resource check.

## Postconditions

- Success: an `ACTIVE` Shop with exactly one `OWNER` and scoped operator roles, or a recorded rejection.
- Failure: no Shop, role or review change beyond recorded evidence.

## Acceptance criteria

- **AC-UC-002-01 — Registration (Proposed):** a verified User with a valid unique Shop name creates exactly one `PENDING_REVIEW` Shop with that User as its single `OWNER`.
- **AC-UC-002-02 — Registration guards (Proposed):** an unverified User, a duplicate Shop name or exceeding the per-User Shop limit creates no Shop.
- **AC-UC-002-03 — Approval (Proposed):** a Moderator approval moves the Shop to `ACTIVE`; only an `ACTIVE` Shop may own purchasable Products or receive Orders.
- **AC-UC-002-04 — Rejection (Proposed):** rejection requires a reason, moves the Shop to `REJECTED` and enables a corrected new registration; the reason is attributable.
- **AC-UC-002-05 — Review authority (Proposed):** only an internal Moderator permission may review a Shop registration; Shop operators, Buyers and other internal roles are refused with no change.
- **AC-UC-002-06 — First decision wins (Proposed):** among concurrent Moderator decisions, the first accepted one is final; conflicting later decisions are refused and repeats return the existing outcome.
- **AC-UC-002-07 — Operator invitation (Proposed):** the `OWNER` can invite an `ACTIVE` User with exactly one Shop role; the grant is effective only for that Shop and only after acceptance.
- **AC-UC-002-08 — Role scope (Proposed):** an operator may perform only actions permitted by their role in that Shop; the same User has no rights in a second Shop without a separate grant.
- **AC-UC-002-09 — Owner-only administration (Proposed):** only the `OWNER` can invite, change or remove operators; an operator attempting it is refused with no change.
- **AC-UC-002-10 — Removal effect (Proposed):** after removal the operator is refused new Shop actions; previously accepted actions remain attributable to them and Shop deadlines are unaffected.
- **AC-UC-002-11 — Owner protection (Proposed):** the `OWNER` cannot be removed or demoted through the operator-management actions.
- **AC-UC-002-12 — Pending/rejected Shop restrictions (Proposed):** a `PENDING_REVIEW` or `REJECTED` Shop cannot publish Products, receive Orders or confirm fulfillment.
- **AC-UC-002-13 — Restricted states (Proposed):** a `RESTRICTED` or `SUSPENDED` Shop is refused the actions blocked by its state, with existing Orders unchanged by this use case.
- **AC-UC-002-14 — Cross-Shop isolation (Proposed):** two Shops operated by the same User or by different Users cannot read or change each other's Shop-owned data through role grants in the other Shop.
- **AC-UC-002-15 — Idempotent repeats (Proposed):** repeating an identical registration, invitation, role change or review action returns the existing outcome with no duplicate Shop, grant or audit effect.
- **AC-UC-002-16 — Audit (Proposed):** registration, review, invitation acceptance, role change and removal record actor, action, changed data and timestamp; rejection includes its reason.

## Traceability

- SC-009/SC-016/SC-023 → UC-002 → [BR-SHOP-001](../BUSINESS_RULES.md#br-shop-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [SM-SHOP-001](../STATE_MACHINES.md#sm-shop-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-002-01 through AC-UC-002-16.

## Open questions

[OQ-004](../OPEN_DECISIONS.md#oq-004) Shop interventions and recovery; [OQ-005](../OPEN_DECISIONS.md#oq-005) permission matrix; [OQ-020](../OPEN_DECISIONS.md#oq-020) account prerequisites; [OQ-012](../OPEN_DECISIONS.md#oq-012) audit retention.
