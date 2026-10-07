# UC-004 — Create or update a Product and its SKUs

## Metadata

- Status: Draft; initial `DRAFT` state code and unconfirmed handling are Proposed.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-010/SC-011, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Let an authorized Seller maintain category-specific Product content and SKU operational data while respecting moderation rules.

## Primary actor

Seller / Shop Operator with Product-maintenance permission for the target Shop.

## Supporting actors

Admin supplies category/attribute definitions through UC-003. Moderator review is a separate [UC-006](UC-006-review-product.md), not a side effect of saving.

## Trigger

Seller starts a new Product or requests an edit to a Product/SKU operated by its Shop.

## Preconditions

- Seller identity and target Shop are known; required permission is checked before protected data changes.
- A category and its applicable attribute definitions exist for submission. The exact validity rules await OQ-002.
- For edits, the Product belongs to the target Shop and exists. Pending high-risk content must be successfully withdrawn before further risky edits under BR-PROD-003. If withdrawal competes with a review decision, the first accepted action wins. Staff-imposed restrictions remain OQ-004.

## Main flow

1. Seller identifies the Shop and provides Product category, name, description, media, category attributes, and variant/SKU data.
2. The system checks authorization and identifies the category's applicable definitions.
3. Seller supplies SKU prices, activation choices, and internal codes. Initial stock or later quantity adjustments are handled by UC-007, not a second inventory adjustment within this use case.
4. The system records editable Product/SKU content under the Shop, with the Proposed initial business state `DRAFT`.
5. The system reports the saved content and any known submission-blocking validation findings. Required-field and draft-save validation boundaries remain open under OQ-002.
6. The system records the attributable content changes for audit. Saving does not approve or publish a Product.
7. Seller may invoke [UC-005](UC-005-submit-product.md) to submit content for automatic checks and manual review.

## Alternative and error flows

- **A1 — Operational edit on an active Product:** valid SKU price, SKU activation/deactivation, or internal SKU code changes take effect immediately without manual re-review. Inventory quantity changes use UC-007. Automatic validation and authorization still apply.
- **A2 — High-risk edit:** category, name, description, media, category attributes, or added/changed variants require manual re-review under BR-PROD-001. Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) hides the entire Product immediately when an actual risky change is successfully saved, before submission. It remains unavailable through content preparation, validation failure, review, and rejection until manual approval. Opening an editor or a failed save does not satisfy the trigger.
- **A3 — Incomplete or invalid draft (Proposed):** under the proposed OQ-002 boundary, missing listing content may be retained in a draft, while invalid supplied values reject the attempted save without applying its changes. Report affected fields and unmet rules. Incomplete or invalid content cannot pass UC-005. Exact boundary values remain open under OQ-002.
- **A4 — Permission denied or wrong Shop:** reject the protected action and leave Product/SKU content unchanged. Exact permission assignments await OQ-005.
- **A5 — Further risky edit while review is pending:** Seller first requests withdrawal of the targeted pending submission. If withdrawal is accepted first, that submission becomes ineligible and Seller may edit/resubmit while the Product stays hidden. If a Moderator decision was accepted first, withdrawal is refused and Seller follows that decision's resulting path. Repeating withdrawal returns the same outcome without a new effect.
- **A6 — Rejected content:** for re-review rejection, Accepted OQ-016 requires correction and resubmission through UC-005 followed by manual approval; the entire Product stays hidden while corrections are saved and reviewed. First-publication correction and the `DRAFT` code mapping remain Proposed. Rejection reasons stay attributable; edit/withdraw/concurrency behavior follows Accepted BR-PROD-003/OQ-003/OQ-018.

## Business rules

[BR-PROD-001](../BUSINESS_RULES.md#br-prod-001) defines risk classification and the operational exemption. Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) starts hiding on successful risky saving and retains it until manual approval. Category/SKU validity follows approved validation rules once OQ-002/OQ-006 are resolved.

## State/data changes

New editable Product content and SKU operational data belong to the selected Shop. Proposed initial state: `DRAFT`. Seller creation/saving does not directly approve content. Successful risky saving on a previously active Product immediately hides it until manual approval, independently of submission. Operational-only edits do not require re-review and cannot unhide a Product restricted for unapproved risky content. Inventory effects remain UC-007; state-code mappings remain unconfirmed, while pending withdrawal/edit/conflict behavior follows Accepted BR-PROD-003.

## Postconditions

- Success: permitted Product/SKU changes are recorded and attributable; valid operational changes are effective without manual review.
- Failure: no unauthorized change or approval is applied.
- Content awaiting initial approval is not represented as an approved purchasable Product.

## Acceptance criteria

- **AC-UC-004-01 — Draft creation (Proposed):** given an authorized Shop Operator and content permitted by the draft-save policy, when a Product is saved, it is associated with that Shop in `DRAFT`, not `ACTIVE`, and has an attributable audit record.
- **AC-UC-004-02 — Shop authorization:** given an actor without Product-maintenance permission for the target Shop, saving/editing does not change that Shop's protected Product/SKU data.
- **AC-UC-004-03 — Operational changes:** given an active Product and valid SKU price, activation, or internal-code changes, permitted updates take effect without generating a required manual review; Product approval remains unchanged.
- **AC-UC-004-04 — Inventory ownership:** when Seller changes a stock quantity, the action is handled as a single UC-007 inventory operation; saving Product content does not apply an additional stock adjustment. Exact invariant tests depend on OQ-008.
- **AC-UC-004-05 — Variant changes:** when variants are added or changed on an active Product, the edited content requires manual re-review and is not automatically approved by saving.
- **AC-UC-004-06 — Risky content changes:** for each category/name/description/media/category-attribute change, manual re-review is required before edited content is treated as approved. Successful saving immediately hides the entire Product under Accepted BR-PROD-002; it stays hidden until manual approval, including before submission, validation failure, and rejection.
- **AC-UC-004-07 — Operational edits cannot bypass review hiding (Accepted policy):** given a Product hidden because risky content was saved and awaits preparation, validation, manual review, or recovery from rejection, when an otherwise permitted SKU activation, price, stock, or internal-code update is applied, the Product remains unavailable for sale until required content approval.
- **AC-UC-004-08 — Hide immediately on successful risky saving (Accepted policy):** given an active Product, when an authorized Seller successfully saves an actual change requiring re-review under BR-PROD-001, the entire Product immediately becomes hidden from Buyer-facing sale and cannot be newly purchased, including through previously selected cart items, even before submission. Opening the editor or attempting a failed save does not constitute this trigger. Operational-only changes do not trigger review-related hiding.
- **AC-UC-004-09 — Edit after withdrawal (Accepted policy):** given high-risk content in a pending review submission, when an authorized Seller successfully withdraws that submission, the Seller may save further high-risk edits; the withdrawn submission is no longer eligible for a decision and the Product remains hidden.
- **AC-UC-004-10 — Incomplete draft (Proposed under OQ-002):** given an authorized Seller and supplied values satisfying the agreed draft rules, when listing-required content is missing, saving retains the draft and reports the missing submission requirements without entering manual review or approving publication. A successful actual risky change to a previously active Product still hides it under BR-PROD-002.
- **AC-UC-004-11 — Invalid save has no partial effect (Proposed under OQ-002):** given an attempted save containing a supplied value that violates an applicable rule, saving reports the affected field and correction, applies none of that attempt's content changes, and leaves existing visibility unchanged. Existing review-related hiding therefore remains in force.

## Traceability

- SC-010/SC-011 → UC-004 → BR-PROD-001, [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002), [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003), [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-004-01 through AC-UC-004-11.
- Submission: UC-005; review: UC-006; inventory: [UC-007](UC-007-manage-inventory.md), Draft with Proposed quantity/hold/adjustment guards; exact units, SKU identity and permissions remain open.

## Open questions

[OQ-002](../OPEN_DECISIONS.md#oq-002) draft/submission validation; [OQ-004](../OPEN_DECISIONS.md#oq-004) restricted states; [OQ-005](../OPEN_DECISIONS.md#oq-005) permissions; [OQ-006](../OPEN_DECISIONS.md#oq-006) SKU identity; [OQ-008](../OPEN_DECISIONS.md#oq-008) stock invariants. OQ-001/OQ-003/OQ-016/OQ-017/OQ-018 are resolved.
