# UC-005 — Submit a Product for review

- Implementation status: Not implemented — active core design slice (Part 1).
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; state codes and validation boundaries remain Proposed/Open. Repeat/resubmission behavior is Accepted.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-010/SC-011/SC-016, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Pass deterministic Product checks before a Seller's content can enter manual moderation.

## Primary actor

Seller / Shop Operator with Product-submission permission for the target Shop.

## Supporting actors

Moderator receives valid content through [UC-006](UC-006-review-product.md). Admin maintains category definitions and validation policy; Admin is not the default reviewer.

## Trigger

Seller requests review of content prepared through [UC-004](UC-004-create-update-product.md).

## Preconditions

- Product and target Shop exist; actor permission is checked before a submission is created.
- The content to submit is identifiable. Proposed initial path: `DRAFT`; rejected-content resubmission and active-content re-review have additional open policies.
- Applicable category, data, and content validation rules are available. Exact boundaries await OQ-002.

## Main flow

1. Seller identifies the Product content to submit.
2. The system checks Shop authorization and whether the requested submission is permitted for the current business state.
3. The system validates required fields/category attributes, price, non-negative inventory, category validity, supported image format, and forbidden words in title/description.
4. If all checks pass, the system records the content submitted for review and makes it available to a Moderator, with Proposed initial-review state `PENDING_REVIEW`.
5. The system records actor, submission action, affected content, and time for audit.
6. Seller receives the pending-review outcome. Automatic validation alone does not activate a Product.

## Alternative and error flows

- **A1 — Deterministic validation failure:** provide the validation findings and do not admit the content to manual review. For the Proposed first-submission model, the Product remains `DRAFT`; no approval is applied.
- **A2 — Permission denied or wrong Shop:** reject the action without a new review submission or protected content change.
- **A3 — Rejected content corrected:** for re-review rejection, Accepted OQ-016 requires Seller correction through UC-004, resubmission, automatic checks, and manual approval; the entire Product remains hidden through these steps and any validation failure. First-publication correction code mapping stays Proposed; edit/withdraw/concurrency behavior follows Accepted BR-PROD-003/OQ-003/OQ-018.
- **A4 — Previously active Product re-review:** successful risky saving in UC-004 has already hidden the Product before this submission. Automatic validation, including failure, does not restore sale. Valid submission enters manual review with the entire Product still hidden; rejection keeps it hidden until corrected/resubmitted content receives manual approval under Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002). Code mapping remains Proposed.
- **A5 — Withdraw, edit, and resubmit:** after withdrawal wins against any competing review action, the withdrawn submission is ineligible. Seller edits through UC-004 and submits revised content; automatic validation runs again and a successful request creates a new eligible submission. Repeating unchanged submission while already pending returns the existing pending submission without duplicate moderation work. The Product stays hidden.
- **A6 — Staff-hidden/locked content:** intervention permissions/recovery remain OQ-004; no bypass is assumed. Review-related visibility and recovery follow BR-PROD-002.

## Business rules

[BR-PROD-001](../BUSINESS_RULES.md#br-prod-001): deterministic checks precede human review; only Moderator approval makes content approved; risky edits need re-review; operational edits do not need manual re-review. Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) hides a previously active Product during pending re-review and after rejection until corrected content obtains approval.

## State/data changes

Proposed first-submission model: `DRAFT` → `PENDING_REVIEW` only after valid submission. Validation failure leaves content out of manual review. Record submitted business content and attribution; persistence is a later design choice. A previously active Product has already been hidden on successful risky saving in UC-004; this use case keeps it hidden during validation and pending review. Approval is required to restore publication eligibility. The proposed re-review code mapping is `ACTIVE` → `DRAFT` at saving, then `DRAFT` → `PENDING_REVIEW` at valid submission; these names remain Proposed.

## Postconditions

- Success: valid submitted content awaits manual review and is attributable to the Seller; it is not newly approved by this use case.
- Failure: invalid or unauthorized submission does not enter manual review or activate a Product.

## Acceptance criteria

- **AC-UC-005-01 — Valid first submission (Proposed codes):** given permitted draft content passing all agreed checks, submission creates a pending-review outcome and makes that content available for UC-006; it does not make the Product `ACTIVE`.
- **AC-UC-005-02 — Validation gate:** for each missing-field, invalid-price, negative-inventory, invalid-category, unsupported-image-format, or forbidden-word fixture, submission reports the unmet check and does not enter manual review. Concrete fixtures depend on OQ-002.
- **AC-UC-005-03 — Shop authorization:** given an actor without submission permission for the target Shop, the request causes no review submission, Product state transition, or protected data change.
- **AC-UC-005-04 — Human approval required:** passing automatic checks never substitutes for a Moderator's approval; Seller cannot use submission to self-approve a Product.
- **AC-UC-005-05 — Same content and repeated requests (Accepted policy):** given unchanged content already represented by an eligible pending submission, when Seller repeats submission, the existing pending outcome is returned without another review item, transition, notification, or audit effect. Changed content after withdrawal requires new validation and cannot inherit the earlier submission's validation or decision.
- **AC-UC-005-06 — Audit:** a successful submission has attributable actor, action, affected submitted content, and time; rejected validation is distinguishable from successful submission in the reported outcome. Failed-attempt audit coverage awaits OQ-012.
- **AC-UC-005-07 — Preserve hiding through validation and submission (Accepted policy):** given a previously active Product already hidden on successful risky saving, when revised content passes or fails submission validation, the entire Product remains unavailable for sale, including through previously selected cart items. Valid submission enters manual review without restoring sale; failed validation does not enter manual review or restore the old approved content.
- **AC-UC-005-08 — Resubmit after withdrawal (Accepted policy):** given a successfully withdrawn submission and further Seller edits, when the revised content is submitted, automatic validation runs again; if it passes, a new eligible pending submission is created, while the withdrawn submission remains ineligible and the Product remains hidden.
- **AC-UC-005-09 — Draft saving does not satisfy submission (Proposed under OQ-002):** given a saved draft missing any required listing content, submission reports the affected fields, unmet rules, and required corrections without creating a pending review item. Given complete content with an enabled SKU at zero inventory and all other checks passing, zero inventory alone does not block review submission; purchase still requires the applicable stock guards.

## Traceability

- SC-010/SC-011/SC-016 → UC-005 → BR-PROD-001, [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002), [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003), [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-005-01 through AC-UC-005-09.
- Content maintenance: UC-004; manual review: UC-006.

## Open questions

[OQ-002](../OPEN_DECISIONS.md#oq-002) validation boundaries; [OQ-004](../OPEN_DECISIONS.md#oq-004) restricted states; [OQ-005](../OPEN_DECISIONS.md#oq-005) permissions; [OQ-012](../OPEN_DECISIONS.md#oq-012) failed-attempt audit coverage. OQ-001/OQ-003/OQ-016/OQ-017/OQ-018 are resolved.
