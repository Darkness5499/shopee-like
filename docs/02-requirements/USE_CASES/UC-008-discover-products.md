# UC-008 — Discover Products

## Metadata

- Status: Draft; public-listing and SKU-availability policy is Proposed under OQ-019. Accepted review-related hiding remains binding under BR-PROD-002.
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope: SC-002, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).
- Source: baseline category/search/filter capability; [glossary](../GLOSSARY.md); Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002); Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004) / [OQ-019](../OPEN_DECISIONS.md#oq-019).

## Goal

Let a Buyer find publicly listed Products using categories, search, and supported filters, while distinguishing visible listings from SKUs currently available for purchase.

## Primary actor

Buyer discovering Products.

## Supporting actors

Seller / Shop Operator maintains Product content and SKU operational data through [UC-004](UC-004-create-update-product.md) and UC-007. Admin maintains categories through UC-003; Moderator establishes content approval through [UC-006](UC-006-review-product.md). These actors do not participate in each discovery request.

## Trigger

Buyer browses a category, submits a search, or changes supported filters.

## Preconditions

- Category and attribute definitions exist for the requested supported category filters.
- The system can determine Product content approval, review-related hiding, Shop selling eligibility, and SKU activation/validity. Exact Shop restrictions and permissions remain OQ-004/OQ-005; SKU validity remains OQ-002/OQ-006.
- No assumption is made here about guest access or account-verification requirements; access boundaries remain OQ-005.

## Main flow

1. Buyer selects a category or supplies a search request and supported filters for category attributes, price, rating, and Shop.
2. The system checks that supplied criteria reference supported category/attribute definitions and valid filter values. Exact limits and rating calculations remain open.
3. The system identifies Products eligible for public listing. Under Proposed BR-PROD-004, this requires approved current content, a Shop permitted to sell, and at least one enabled valid SKU. Accepted BR-PROD-002 independently excludes Products hidden for risky-content changes.
4. The system applies the requested criteria to eligible Products. Under Proposed OQ-019, displayed prices and price filtering derive only from enabled valid SKUs; an out-of-stock SKU may contribute because listing visibility does not itself require positive stock.
5. The system presents matching Products with approved listing content, Shop identity, supported price information, and availability information. Rating data is represented only when supported by actual review data and the eventual OQ-013 calculation rules.
6. Buyer may refine the criteria or open a matching Product through [UC-009](UC-009-view-product.md). Discovery does not reserve inventory or create an Order.

## Alternative and error flows

- **A1 — No matches:** report that no eligible Products match the supplied criteria; Buyer may adjust them. Do not substitute hidden or unapproved Products to fill the result.
- **A2 — Invalid or unsupported criterion:** identify the affected criterion and required correction rather than presenting a result as though that criterion had been applied. Exact category/value limits remain OQ-002; rating semantics remain OQ-013.
- **A3 — Product hidden for re-review (Accepted policy):** exclude the entire Product from Buyer-facing results from the successful save of an actual risky change, including before submission, during validation failure, while pending or withdrawn, and after rejection until manual approval. Neither old approved content nor revised unapproved content remains a sale listing. BR-PROD-003 withdrawal/resubmission does not bypass BR-PROD-002.
- **A4 — Enabled valid SKUs have no available stock (Proposed):** keep the Product listed when the other visibility conditions hold, identify the affected SKU/Product availability as unavailable or out of stock, and do not imply that listing visibility guarantees a new purchase. Exact available-stock calculation remains OQ-008.
- **A5 — All SKUs disabled or no enabled valid SKU (Proposed):** omit the Product from public listings. An otherwise valid activation can restore eligibility only if content approval and Shop selling conditions also hold; it cannot remove review-related hiding.
- **A6 — Operational changes after a previous result:** reflect permitted price, activation, and inventory changes when producing subsequent results. A previously displayed result does not preserve eligibility for later detail, Cart, or checkout actions; those use cases check their own current guards.
- **A7 — Rating absent or unavailable:** do not fabricate a rating or treat it as an observed zero. Whether an unrated Product matches a rating filter awaits OQ-013; the final rating-filter criterion cannot be completed until that decision is resolved.
- **A8 — Shop restricted from selling (Proposed boundary):** omit Products when the applicable Shop rule disallows public selling. Which Shop outcomes impose this condition and their effect on existing Orders remain OQ-004/OQ-005.

## Business rules

- Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002) governs immediate review-related hiding across discovery and later purchase entry points.
- Accepted [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003) governs withdrawal/resubmission; these actions cannot restore sale before required approval.
- Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004) governs public listing versus per-SKU purchase eligibility; its confirmation remains [OQ-019](../OPEN_DECISIONS.md#oq-019).
- Content approval concerns the high-risk descriptive content that requires moderation. Discovery combines that approved content with current permitted operational price, activation, and inventory information; those operational updates do not require a new Moderator approval under BR-PROD-001.
- Category/filter validity follows UC-003/OQ-002. SKU identity and validity remain OQ-006; rating calculation and filter treatment remain OQ-013.

## State/data changes

Discovery reads listing information and does not change Product approval, [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001), SKU activation, stock, reservations, Cart contents, or Orders. Search history, recommendation behavior, and ranking are not specified by this use case.

## Postconditions

- Success: Buyer receives matching eligible listing information, with availability distinguished from visibility.
- No matches or invalid criteria: the outcome is explicit; no protected or unapproved content is substituted.
- Viewing a result creates no stock reservation or right to purchase at a prior price or availability.

## Acceptance criteria

- **AC-UC-008-01 — Supported discovery:** given Products eligible for public listing and valid supported criteria, when Buyer browses/searches/filters, results satisfy the supplied category, attribute, Shop, price, and rating criteria once their definitions are resolved. Rating-related cases depend on OQ-013.
- **AC-UC-008-02 — Immediate exclusion (Accepted policy):** given a previously listed Product, when an authorized Seller successfully saves an actual risky change under BR-PROD-001, subsequent discovery results exclude the whole Product under BR-PROD-002 even before submission. Opening the editor or a failed save does not itself trigger exclusion.
- **AC-UC-008-03 — Review recovery cannot expose content (Accepted policy):** given a Product hidden under BR-PROD-002, when preparation, validation failure, submission, withdrawal, or rejection occurs, discovery does not expose either the old approved listing or the revised unapproved content; required manual approval is necessary before sale eligibility can return.
- **AC-UC-008-04 — Public listing guards (Proposed under OQ-019):** given a Product, discovery includes it only if current content is approved, the Shop permits selling, and at least one SKU is enabled and valid, with no review-related hiding. Exact restriction/validity scenarios depend on OQ-004/OQ-005/OQ-002/OQ-006.
- **AC-UC-008-05 — Out-of-stock listing (Proposed under OQ-019):** given a Product meeting public-listing guards whose enabled valid SKUs have zero available stock, discovery retains its listing eligibility and returns it when it matches the requested criteria, identifying it as unavailable or out of stock; it does not present those SKUs as purchasable. Stock test values depend on OQ-008.
- **AC-UC-008-06 — Disabled SKUs and prices (Proposed under OQ-019):** given enabled valid and disabled/invalid SKUs with different prices, displayed price information and price-filter matching use only enabled valid SKUs. A disabled/invalid SKU's price cannot make the Product match a price filter or determine its displayed price range.
- **AC-UC-008-07 — All SKUs disabled (Proposed under OQ-019):** given an otherwise approved Product, when every SKU is disabled, it is absent from public discovery. Re-enabling a valid SKU restores eligibility only when the other guards hold and never bypasses BR-PROD-002.
- **AC-UC-008-08 — Empty and invalid requests:** when valid criteria yield no eligible matches, the outcome states no matches; when supplied criteria are unsupported or invalid, the affected criteria are reported rather than silently treated as applied.
- **AC-UC-008-09 — No fabricated rating:** given no applicable review data, discovery does not show an invented numeric rating. Matching of unrated Products to rating filters remains pending OQ-013.
- **AC-UC-008-10 — Read-only discovery:** given repeated discovery requests, Product/review state, SKU activation, stock/reservations, Cart contents, and Orders remain unchanged. Prior discovery results do not bypass current eligibility checks at later purchase steps.

## Traceability

- SC-002 → UC-008 → Accepted [BR-PROD-002](../BUSINESS_RULES.md#br-prod-002), [BR-PROD-003](../BUSINESS_RULES.md#br-prod-003); Proposed [BR-PROD-004](../BUSINESS_RULES.md#br-prod-004); [SM-PRODUCT-001](../STATE_MACHINES.md#sm-product-001); Proposed [NFR-005](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-005) → AC-UC-008-01 through AC-UC-008-10.
- Related catalog use cases: UC-003 category definitions, UC-007 inventory, [UC-009](UC-009-view-product.md) detail, UC-018 reviews.
- Canonical coverage: [TRACEABILITY.md](../TRACEABILITY.md). Domain/design and acceptance-test artifacts remain pending. NFR-005 workload/latency verification remains Proposed under OQ-012; it does not relax Accepted hiding.

## Open questions

[OQ-002](../OPEN_DECISIONS.md#oq-002) category/filter/SKU validity; [OQ-004](../OPEN_DECISIONS.md#oq-004) restriction outcomes; [OQ-005](../OPEN_DECISIONS.md#oq-005) Shop eligibility/access; [OQ-006](../OPEN_DECISIONS.md#oq-006) SKU identity; [OQ-008](../OPEN_DECISIONS.md#oq-008) available stock; [OQ-012](../OPEN_DECISIONS.md#oq-012) quality targets; [OQ-013](../OPEN_DECISIONS.md#oq-013) rating calculation/filter treatment; [OQ-019](../OPEN_DECISIONS.md#oq-019) public listing versus purchasability. OQ-001/OQ-003/OQ-016/OQ-017/OQ-018 are resolved and are not reopened here.
