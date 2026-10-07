# UC-006 — Review a Product Submission

## Metadata

- Status: **Draft**; not an approved Phase 2 requirement.
- Priority: **P0 — Proposed**; priority requires Project owner confirmation.
- Scope/source: [SC-016 — Moderation; SC-023 — Internal permissions](../../01-product/FUNCTIONAL_SCOPE.md).
- Owner: Project owner.
- Boundary: first-publication review plus Accepted re-review visibility, withdrawal/edit/resubmit, and competing/repeated-action behavior under OQ-001/OQ-003/OQ-016/OQ-017/OQ-018. Validation and intervention details remain Draft.

## Goal

A Moderator decides whether a submitted product may become active, or rejects it with a reason the Seller can act on.

## Primary actor

Moderator with permission to review products.

## Supporting actors

- Seller: submits the product and receives the decision.
- Admin: owns policies and exceptional interventions; is not the default reviewer in this use case.

## Trigger

A Moderator selects a product submission awaiting manual review.

## Preconditions

- A Seller has created the product through [UC-004](UC-004-create-update-product.md) and submitted it through [UC-005](UC-005-submit-product.md).
- The submission passed the deterministic validation required by [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001).
- The acting staff member has the required product-review permission; the detailed permission matrix remains to be specified.
- In the **proposed** [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), the first submission is `PENDING_REVIEW`.

## Main flow

1. The Moderator selects the eligible submission.
2. The System presents its submitted product information and automatic-validation outcome.
3. The Moderator reviews the submitted content against the applicable moderation policy.
4. The Moderator chooses approval or rejection. For rejection, the Moderator supplies a reason.
5. For first publication, approval makes the Product `ACTIVE`; rejection makes it `REJECTED` with a reason. For re-review, rejection records its reason and keeps the entire Product hidden until corrected/resubmitted content obtains manual approval; approval makes revised content eligible for publication under BR-PROD-002.
6. The System records the actor, action, changed data, and timestamp, and makes the decision available to the Seller. Notification channel and delivery timing are unspecified.

## Alternative and error flows

- **Automatic validation failed:** the submission cannot enter this manual-review flow. The Seller corrects invalid data through UC-004 and submits through UC-005; the exact validation boundaries are open in [OQ-002](../OPEN_DECISIONS.md#oq-002).
- **Rejection without a reason:** the System does not accept the rejection; the Moderator must supply a reason before rejection is accepted.
- **Insufficient permission:** the review decision is refused and the Product state does not change.
- **Withdrawal or competing decision:** the first accepted action wins. If withdrawal wins, the submission is ineligible for approval/rejection. If approval/rejection wins, later withdrawal or the competing opposite decision is refused. Repeating the winning decision returns its existing outcome without another transition, publication action, notification, or audit effect.
- **Active-product re-review:** Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) hides the entire Product while review is pending and after rejection. Rejection requires a reason; old approved content is not automatically restored. Seller corrects/resubmits the content, and only eventual manual approval makes revised content eligible for publication, subject to normal purchasing guards.
- **Staff-imposed hiding, locking, or exceptional intervention:** handled outside this use case; their permissions and state transitions remain open in [OQ-004](../OPEN_DECISIONS.md#oq-004). Temporary pending-review hiding follows BR-PROD-002.

## Business rules

- [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001) requires automatic validation before manual review, assigns ordinary review to the Moderator, and defines first-publication outcomes. Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) adds pending re-review hiding and continued hiding after rejection until corrected content is approved.
- Rejection requires a reason; approval and rejection cannot bypass the automatic-validation gate.
- High-risk edits to active Products require re-review. Operational SKU changes do not require re-review, subject to valid business data, and cannot bypass Product-level pending-review hiding.
- Staff actions must respect assigned permissions and produce audit records, as established in [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md) and elaborated in draft [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001) and [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004).

## State/data changes

- First-publication outcomes: Product becomes `ACTIVE` on approval or `REJECTED` on rejection, with a rejection reason.
- Proposed first-submission lifecycle: `DRAFT → PENDING_REVIEW → ACTIVE | REJECTED`. The first transition belongs to UC-005; UC-006 performs the review transition. `DRAFT` and `PENDING_REVIEW` are proposed canonical state names, not previously recorded decisions.
- Review outcomes and Product publication state must not be confused: first-publication approval leads to Product `ACTIVE`; re-review approval makes revised content eligible for publication, with code mapping still Proposed. No separate Product `APPROVED` state is introduced here.
- Record the decision and the required audit information. No persistence format or implementation mechanism is specified.
- Hiding from successful risky saving through manual approval is Accepted through BR-PROD-002. Withdrawal/edit/resubmit and first-accepted-action-wins behavior are Accepted through BR-PROD-003/OQ-003/OQ-018. Approval makes revised content eligible for publication; code mappings remain Proposed. Staff intervention stays OQ-004.

## Postconditions

- Successful first-publication approval leaves the Product `ACTIVE`. Re-review approval makes the revised content eligible for publication subject to the normal purchasing guards.
- Successful first-publication rejection leaves the Product `REJECTED` with its reason available to the Seller. Re-review rejection records the reason and leaves the entire Product hidden until corrected/resubmitted content receives manual approval; old content is not automatically restored.
- An accepted decision has the audit information required by Functional Scope.
- A refused decision due to missing permission or missing rejection reason does not change the Product state.

## Acceptance criteria

- **AC-UC-006-01 — Approval:** Given a first submission that passed automatic validation and is awaiting review, when an authorized Moderator approves it, then the Product becomes `ACTIVE` and the approval outcome is available to the Seller.
- **AC-UC-006-02 — Rejection:** Given an eligible first submission, when an authorized Moderator rejects it with a reason, then the Product becomes `REJECTED` and the Seller can read that reason.
- **AC-UC-006-03 — Reason required:** Given an eligible first submission, when rejection is attempted without a reason, then no rejection is accepted and the Product state is unchanged.
- **AC-UC-006-04 — Validation gate:** Given a submission that failed automatic validation, when manual review is attempted, then neither approval nor manual rejection is accepted through UC-006.
- **AC-UC-006-05 — Permission:** Given an actor without product-review permission, when that actor attempts a review decision, then the decision is refused and the Product state is unchanged.
- **AC-UC-006-06 — Audit:** Given an accepted approval or rejection, when its audit record is inspected, then it identifies the actor, action, changed data, and timestamp.
- **AC-UC-006-07 — Withdrawn submission is ineligible (Accepted policy):** given a pending submission whose withdrawal was accepted first, when a Moderator attempts to approve or reject it, the decision is refused and no Product publication change occurs. A later resubmission is reviewed as a new eligible submission.
- **AC-UC-006-08 — Re-review approval (Accepted policy):** Given a previously active Product hidden while valid revised content awaits review, when an authorized Moderator approves that content, the revised content becomes eligible for publication. Normal Shop/SKU/stock guards still apply; this approval does not override staff restrictions.
- **AC-UC-006-09 — Continued hiding after rejection (Accepted policy):** Given a previously active Product hidden for re-review, when an authorized Moderator rejects revised content with a reason, the reason is available to the Seller and the entire Product stays hidden and unavailable for purchase; old approved content is not automatically restored. Saving corrections, failed validation, and successful resubmission/automatic checks keep it hidden. Corrected content becomes eligible for publication only after manual approval, subject to the normal purchasing guards.
- **AC-UC-006-10 — First accepted action wins (Accepted policy):** given withdrawal, approval, or rejection attempts competing for the same pending submission, when one action is accepted first, every later conflicting action is refused and cannot overwrite the outcome. Repeating the accepted action returns the existing outcome without another transition, publication action, notification, or audit effect.

## Traceability

- [Product Vision](../../01-product/PRODUCT_VISION.md) and [SC-016/SC-023](../../01-product/FUNCTIONAL_SCOPE.md) → UC-006 → [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001), [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002), [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003), [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-006-01 through AC-UC-006-10.
- Related use cases: [UC-004 — Create or update product](UC-004-create-update-product.md), [UC-005 — Submit product](UC-005-submit-product.md).
- Acceptance criteria are specification criteria; implementation and test evidence belong to later phases.

## Open questions

- [OQ-018](../OPEN_DECISIONS.md#oq-018) is resolved: Seller may withdraw the pending submission before editing/resubmitting; the Product remains hidden and the withdrawn submission is ineligible.
- [OQ-002](../OPEN_DECISIONS.md#oq-002): Which required fields, valid price limits, media formats, category constraints, and forbidden-word rules define the automatic-validation gate? Manual-review policy detail must also be made testable.
- [OQ-003](../OPEN_DECISIONS.md#oq-003) is resolved: the first accepted withdrawal/approval/rejection wins; conflicting later actions are refused and repeats return the existing outcome without new effects.
- [OQ-004](../OPEN_DECISIONS.md#oq-004): Which staff permissions, reasons, and transitions govern hide, lock, and exceptional interventions?
