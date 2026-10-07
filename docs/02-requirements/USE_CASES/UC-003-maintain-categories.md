# UC-003 — Maintain categories and attribute definitions

- Implementation status: Not implemented — deferred; closed for current work. Resume only when the owner selects a later part.
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; hierarchy limits, attribute types and change effects are Proposed under [OQ-002](../OPEN_DECISIONS.md#oq-002) and [OQ-021](../OPEN_DECISIONS.md#oq-021).
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-017; supports SC-002/SC-010, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Let an Admin maintain a hierarchical category tree and category-specific attribute definitions so Sellers can list Products with valid dynamic attributes and Buyers can browse and filter by them.

## Primary actor

Admin with category-administration permission.

## Supporting actors

Seller ([UC-004](UC-004-create-update-product.md)/[UC-005](UC-005-submit-product.md)) consumes definitions; Buyer ([UC-008](UC-008-discover-products.md)) browses and filters by them.

## Trigger

Admin creates, renames, moves, activates/deactivates a category, or changes its attribute definitions.

## Preconditions

- The actor holds the category-administration permission ([BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001)).
- Hierarchy and attribute limits follow [BR-CATEGORY-001](../BUSINESS_RULES.md#br-category-001).

## Main flow

1. Admin creates a category under a parent (or as a root) with a unique name among its siblings.
2. Admin defines attributes on a category: name, type (`TEXT`, `NUMBER`, `ENUM`, `BOOLEAN`), required or optional, filterable or not, and allowed values for `ENUM`.
3. Admin activates the category. Only active leaf categories accept new Product listings.
4. The system stores a new immutable version of the category's attribute schema for each change and keeps the identity of each attribute stable.
5. The system records attributable audit entries for each administrative change.

## Alternative and error flows

- **A1 — Invalid structure:** exceeding the maximum depth, a duplicate sibling name, a cycle on move, or making a category with Products a non-leaf is refused with no change.
- **A2 — Invalid attribute definition:** a missing type, an `ENUM` without allowed values, or a duplicate attribute name within the category is refused with no change.
- **A3 — Schema change with existing Products:** a change affects new saves and submissions from its effective moment; existing Products are not automatically re-reviewed, hidden or changed, but a later risky edit or submission must satisfy the then-current required attributes.
- **A4 — Deactivation:** deactivating a category blocks new Product creation or submission under it and removes it from Seller category selection; existing Products and accepted Orders are unchanged by this use case. Any hiding of existing Products is a separate intervention under OQ-004.
- **A5 — Removing an attribute value or attribute in use:** the definition is retired, not erased; existing Product values stay readable but are not offered for new selection or filtering.
- **A6 — Permission denied:** a non-Admin actor, including a Moderator or Seller, is refused with no change.
- **A7 — Concurrent administration:** concurrent edits to the same category are ordered; a change based on a stale version is refused rather than silently overwriting.
- **A8 — Repeated request:** an identical administrative request returns the existing outcome with no duplicate category, version or audit effect.

## Business rules

[BR-CATEGORY-001](../BUSINESS_RULES.md#br-category-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001) for the automatic-validation baseline that consumes definitions.

## State/data changes

Creates and versions Category and AttributeDefinition records. Categories are `DRAFT`, `ACTIVE` or `INACTIVE`; transitions are Proposed in [BR-CATEGORY-001](../BUSINESS_RULES.md#br-category-001).

## Postconditions

- Success: the category tree and attribute schemas are consistent, versioned and attributable.
- Failure: no category, attribute or version change.

## Acceptance criteria

- **AC-UC-003-01 — Category creation (Proposed):** Admin can create a category with a name unique among siblings within the maximum depth; the new category is not listable until activated.
- **AC-UC-003-02 — Structure guards (Proposed):** exceeding the maximum depth, a duplicate sibling name, a move creating a cycle, or making a category with Products a non-leaf is refused with no change.
- **AC-UC-003-03 — Attribute definition (Proposed):** Admin can define attributes with a supported type, required/optional and filterable flags and, for `ENUM`, allowed values; invalid definitions are refused with no change.
- **AC-UC-003-04 — Leaf-only listing (Proposed):** only an `ACTIVE` leaf category accepts new Product saves for listing and submission; non-leaf or non-active categories do not.
- **AC-UC-003-05 — Versioned schema (Proposed):** every attribute-schema change creates a new immutable version with stable attribute identities, and a Product submission is validated against the version effective at its validation moment.
- **AC-UC-003-06 — Existing Products unaffected (Proposed):** a schema change does not hide, re-review or alter existing Products or Orders; a later risky edit or submission must satisfy the current required attributes.
- **AC-UC-003-07 — Deactivation (Proposed):** deactivating a category blocks new Product creation/submission under it and removes it from Seller selection without changing existing Products or accepted Orders.
- **AC-UC-003-08 — Retired attributes (Proposed):** retiring an attribute or `ENUM` value in use preserves existing values for reading but excludes them from new selection and filters.
- **AC-UC-003-09 — Buyer-facing consistency (Proposed):** category navigation and filters offered to Buyers use only `ACTIVE` categories and filterable, non-retired attributes.
- **AC-UC-003-10 — Permission (Proposed):** only an actor with category-administration permission can change categories or attributes; Moderators, Sellers and Buyers are refused with no change.
- **AC-UC-003-11 — Stale change refused (Proposed):** a change based on a superseded category or schema version is refused rather than overwriting the newer version.
- **AC-UC-003-12 — Idempotent repeats (Proposed):** repeating an identical administrative request creates no duplicate category, version or audit effect.
- **AC-UC-003-13 — Audit (Proposed):** each category and attribute change records actor, action, before/after data and timestamp.

## Traceability

- SC-017 (supports SC-002/SC-010) → UC-003 → [BR-CATEGORY-001](../BUSINESS_RULES.md#br-category-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [BR-PROD-001](../BUSINESS_RULES.md#br-prod-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-003-01 through AC-UC-003-13.

## Open questions

[OQ-002](../OPEN_DECISIONS.md#oq-002) validation limits; [OQ-021](../OPEN_DECISIONS.md#oq-021) category hierarchy and schema change policy; [OQ-004](../OPEN_DECISIONS.md#oq-004) intervention on existing Products; [OQ-005](../OPEN_DECISIONS.md#oq-005) permissions.
