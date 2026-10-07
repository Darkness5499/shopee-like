# Requirements Traceability and Coverage

## Current scoped handoff

Owner narrowed work on 2026-10-07: [learning plan](../01-product/LEARNING_PLAN.md) controls scheduling. All UC implementation/testing remains pending. Active SC-010/011/016 → UC-004/005/006 → BR-PROD-001/002/003 and SM-PRODUCT-001 → existing ACs → [Part 1 domain analysis](../03-analysis/PUBLICATION_ANALYSIS.md) → Phase 4 design/test/code pending. Minimal SC-002/003 reads and SC-022 audit support this slice. Every other journey is Deferred, closed for current work. Earlier seven-journey and full-MVP checklists below are historical/wider coverage, not prerequisites to this provisional Phase 3 handoff.

- Status: Draft register.
- Owner: Project owner.
- Updated: 2026-10-07.

This register shows specification coverage, not product approval or executed verification. Scope is the Baseline; catalog priorities are Proposed. No ADR, implementation, or executed acceptance test is linked because those phases have not started.

## Detailed coverage started

### Active Part 1 coverage

- SC-010/011/016/023 → UC-004/005/006 → BR-PROD-001/002/003, scoped BR-ACCESS-001, SM-PRODUCT-001, NFR-001/004 → AC-UC-004-01..11, AC-UC-005-01..09, AC-UC-006-01..10 → [publication analysis](../03-analysis/PUBLICATION_ANALYSIS.md) (Draft; learner review pending) → design/code/tests pending.
- SC-002/003 → minimal UC-008/009 listing/detail hiding demonstration → BR-PROD-002 and canonical eligibility policy → existing visibility ACs; advanced discovery/reviews/vouchers deferred.
- SC-022 → scoped UC-024 audit → audit ACs attached to core commands; full audit UI/inventory deferred.
- Learner evidence: own model/race explanation pending under [WI-007](../WORK_ITEMS.md#wi-007). No behavioral verification has run.

### Historical broad coverage inventory

The mappings below retain useful stable identifiers but were prepared before current scheduling and later recorded decisions. Open/Proposed summaries must be checked against OPEN_DECISIONS.md; they do not override Accepted provenance. Deferred requirements need a new scoped consistency review before use. No full-MVP gate is certified by this inventory.

- SC-010/SC-011 → [UC-004](USE_CASES/UC-004-create-update-product.md) → BR-PROD-001/002/003, SM-PRODUCT-001, NFR-001/NFR-004 → AC-UC-004-01 through AC-UC-004-11. Coverage: Draft; hiding, withdrawal/edit, and race outcomes are Accepted; draft-save boundaries are Proposed under OQ-002; validation limits, permissions, and SKU identity remain open.
- SC-010/SC-011/SC-016 → [UC-005](USE_CASES/UC-005-submit-product.md) → BR-PROD-001/002/003, SM-PRODUCT-001, NFR-001/NFR-004 → AC-UC-005-01 through AC-UC-005-09. Coverage: Draft; hiding, withdrawal/edit/resubmit, and repeat behavior are Accepted; draft-versus-submission checks are Proposed under OQ-002 and exact validation rules remain open.
- SC-016/SC-023 → [UC-006](USE_CASES/UC-006-review-product.md) → BR-PROD-001/002/003, SM-PRODUCT-001, NFR-001/NFR-004 → AC-UC-006-01 through AC-UC-006-10. Coverage: Draft; visibility, withdrawal ineligibility, first-accepted-action-wins, and approval eligibility are Accepted; interventions remain open.
- SC-012 → [UC-007](USE_CASES/UC-007-manage-inventory.md) → BR-INV-001, BR-PROD-001/002/004, SM-INVENTORY-001/RESERVATION-001, NFR-001/003/004 → AC-UC-007-01 through AC-UC-007-18. Coverage: Draft; baseline reserve/release/deduct and Accepted no-hiding-bypass boundary preserved; accounting, paid holds, adjustment/race/repeat policy Proposed under OQ-008; paid timeout/cancellation/rejection release is now Proposed in UC-013/015/OQ-009; consumed-stock restock, units, permissions and SKU identity remain open. Domain/design/tests pending.
- SC-002 → [UC-008](USE_CASES/UC-008-discover-products.md) → BR-PROD-002/003/004, SM-PRODUCT-001, NFR-005 → AC-UC-008-01 through AC-UC-008-10. Coverage: Draft; review-related hiding is Accepted; public listing/SKU/price guards are Proposed under OQ-019; category, ratings, stock, and quality boundaries remain open.
- SC-003 → [UC-009](USE_CASES/UC-009-view-product.md) → BR-PROD-002/003/004, SM-PRODUCT-001, NFR-005 → AC-UC-009-01 through AC-UC-009-12. Coverage: Draft; review-related hiding and blocked new purchases are Accepted; public detail and per-SKU eligibility are Proposed; reviews, vouchers, stock, and historical-purchase rules remain open.
- SC-004 → [UC-010](USE_CASES/UC-010-manage-cart.md) → BR-PROD-002/004, SM-PRODUCT-001, NFR-001 → AC-UC-010-01 through AC-UC-010-09. Coverage: Draft; previously carted review-hidden Products cannot be purchased; private-cart access and current-price/unavailable-entry presentation are Proposed; final checkout rules remain open.
- SC-005/SC-012 → [UC-011](USE_CASES/UC-011-checkout.md) → BR-PROD-002/004, BR-ORDER-001/INV-001/PAY-001, SM-PRODUCT-001/INVENTORY-001/RESERVATION-001/ORDER-001/PAYMENT-001, NFR-001/003/004 → AC-UC-011-01 through AC-UC-011-16. Coverage: Draft; AC-UC-011-03 preserves Accepted hidden-Product purchase block; grouping/quotes/snapshots/ordering/repeats Proposed OQ-007/OQ-008; concrete shipping/vouchers, currency/units, Shop/account guards and SKU identity incomplete. Domain/design/tests pending.
- SC-006/SC-026, with SC-005/007/012/022 → [UC-012](USE_CASES/UC-012-process-payment.md) → BR-PAY-001/ORDER-001/INV-001, SM-PAYMENT-001/ORDER-001/RESERVATION-001, NFR-001/002/003/004/006 → AC-UC-012-01 through AC-UC-012-24. Coverage: Draft; baseline simulated payment and Seller confirmation distinction retained; grouping, paid holds, deadline/event/conflict/recovery policy Proposed OQ-007/OQ-008/OQ-015. Reconciliation disposition, paid cancellation/refund and retry/retention unresolved; domain/design/tests pending.

- SC-013/SC-012, with SC-007/024/025/026 → [UC-013](USE_CASES/UC-013-fulfill-shop-order.md) → BR-ORDER-002/CANCEL-001/INV-001/ORDER-001/PAY-001, SM-ORDER-001/RESERVATION-001/INVENTORY-001/PAYMENT-001/SHIPMENT-001, NFR-001/002/003/004/006 → AC-UC-013-01 through AC-UC-013-22. Coverage: Draft/Proposed paid confirmation/all-line deduction, cutoff/rejection/timeout, complete compensation, packing/handover and recovery; duration/permissions/SKU/financial disposition remain Open. Domain/design/tests pending.
- SC-024/SC-025, with SC-013/007/006/012/022 → [UC-014](USE_CASES/UC-014-simulate-shipment.md) → BR-SHIP-001/ORDER-002/CANCEL-001/ORDER-001/PAY-001/INV-001, SM-SHIPMENT-001/ORDER-001/PAYMENT-001/RESERVATION-001, NFR-001/002/004/006 → AC-UC-014-01 through AC-UC-014-24. Coverage: Draft/Proposed single whole Shipment, creation recovery, authentic sequenced progress, gaps/repeats/conflicts, physical return exception and Order synchronization; query/sequence/correction/retry and after-sales choices remain Open. Domain/design/tests pending.
- SC-007, with SC-005/006/008/012/013/020/022/025/026 → [UC-015](USE_CASES/UC-015-track-cancel-orders.md) → BR-CANCEL-001/ORDER-002/ORDER-001/PAY-001/INV-001/SHIP-001/AFTERSALES-001, SM-ORDER-001/PAYMENT-001/RESERVATION-001/INVENTORY-001/SHIPMENT-001/DISPUTE-001, NFR-001/002/003/004/006 → AC-UC-015-01 through AC-UC-015-26. Coverage: Draft/Proposed owned tracking, unpaid whole-group versus paid-Shop intent, cutoff/races/recovery, compensation, explicit Buyer completion, and 7-day auto-completion timeout.
- SC-008/SC-007/SC-020 → [UC-016](USE_CASES/UC-016-request-return-refund.md) → BR-AFTERSALES-001/REFUND-001/ORDER-001, SM-ORDER-001/DISPUTE-001, NFR-001/004/006 → AC-UC-016-01 through AC-UC-016-16. Coverage: Draft/Proposed return and refund dispute intake, delivered/exception eligibility, requested amount caps, auto-completion suspension, 48h Seller response deadline, and Buyer withdrawal.
- SC-020/SC-023/SC-026, with SC-006/007/008/012/013/022 → [UC-017](USE_CASES/UC-017-resolve-refund-dispute.md) → BR-REFUND-001/STOCK-RECEIPT-001/AFTERSALES-001/CANCEL-001, SM-DISPUTE-001/REFUND-001/ORDER-001/INVENTORY-001/PAYMENT-001, NFR-001/002/003/004/006 → AC-UC-017-01 through AC-UC-017-22. Coverage: Draft/Proposed pre-confirmation compensation execution, Seller response, return physical inspection and restock controls, Support adjudication, exception disposition, and cumulative verified paid funds caps.

- SC-001 → [UC-001](USE_CASES/UC-001-manage-account-addresses.md) → BR-ACCESS-001/ORDER-001, SM-ACCOUNT-001, NFR-001/004 → AC-UC-001-01 through AC-UC-001-14. Coverage: Draft/Proposed registration, simulated verification, sign-in lock, profile/addresses and address snapshot isolation under OQ-020/OQ-005.
- SC-009/SC-016/SC-023 → [UC-002](USE_CASES/UC-002-register-manage-shop.md) → BR-SHOP-001/ACCESS-001, SM-SHOP-001, NFR-001/004 → AC-UC-002-01 through AC-UC-002-16. Coverage: Draft/Proposed Shop registration/review, operator roles, owner protection and cross-Shop isolation; sanctions/recovery remain UC-021/OQ-004.
- SC-017 → [UC-003](USE_CASES/UC-003-maintain-categories.md) → BR-CATEGORY-001/ACCESS-001/PROD-001, NFR-001/004 → AC-UC-003-01 through AC-UC-003-13. Coverage: Draft/Proposed hierarchy, versioned attribute schemas, deactivation and Admin-only access under OQ-002/OQ-021.
- SC-022 (supports SC-023) → [UC-024](USE_CASES/UC-024-record-audit.md) → BR-AUDIT-001/ACCESS-001, NFR-001/004 → AC-UC-024-01 through AC-UC-024-12. Coverage: Draft/Proposed audit inventory, atomicity, immutability, redaction and authorized inspection; retention depends on OQ-012.

Canonical rule/state/quality references: [Business Rules](BUSINESS_RULES.md), [State Machines](STATE_MACHINES.md), and [NFRs](NON_FUNCTIONAL_REQUIREMENTS.md).

## Confirmed policy coverage

[OQ-001](OPEN_DECISIONS.md#oq-001), [OQ-016](OPEN_DECISIONS.md#oq-016), and [OQ-017](OPEN_DECISIONS.md#oq-017), Accepted on 2026-10-07 → BR-PROD-002 → visibility ACs, including AC-UC-008-02/03, AC-UC-009-02/03/09, the blocked-purchase policy within AC-UC-010-04, AC-UC-007-05 and AC-UC-011-03. [OQ-003](OPEN_DECISIONS.md#oq-003) and [OQ-018](OPEN_DECISIONS.md#oq-018), Accepted on 2026-10-07 → [BR-PROD-003](BUSINESS_RULES.md#br-prod-003) → withdrawal/edit/resubmit, first-accepted-action-wins, and idempotent repeats → AC-UC-004-09, AC-UC-005-05/08, AC-UC-006-07/10. Checkout-versus-risky-edit ordering in AC-UC-011-10 is a separate Proposed OQ-007 policy. [OQ-012](OPEN_DECISIONS.md#oq-012), Accepted on 2026-10-07 → NFR-001 through NFR-006 demo quality targets and measurement plan confirmed for the learning demo. These decisions do not accept whole use cases, new Proposed presentation/purchase policies, or state-code mappings.

## Current cross-actor journey

UC-001/002/003 → UC-004/005/006 → UC-008/009/010 → UC-007/011/012 → UC-013/014/015 → UC-016/017, with UC-024 audit throughout, now has eighteen linked Draft specifications spanning foundation, publication, shopping, stock, checkout/payment, Seller fulfillment, simulated delivery, Buyer tracking/completion, dispute intake, refund resolution and audit (284 criteria total).

### Purchase and fulfillment decision impact

- OQ-007 → BR-ORDER-001 → SM-ORDER-001/PAYMENT-001/RESERVATION-001 → AC-UC-011-01/04 through 11/13/16 and AC-UC-012-01/13/19. Impacts UC-010 estimated price versus final acceptance; UC-013 per-Shop fulfillment; UC-015/017 sibling cancellation/refund allocation. Shipping quote validity/fees and voucher-bearing checkout remain incomplete.
- OQ-008 → BR-INV-001 → SM-INVENTORY-001/RESERVATION-001/ORDER-001 → AC-UC-007-01 through 18, AC-UC-011-09/12 and AC-UC-012-03/06 through 10/23/24. Impacts Buyer availability UC-008/009/010 and Seller confirmation UC-013; paid-hold timeout/cancellation is Proposed under UC-013/015/OQ-009; verified return restock is Proposed under UC-017/BR-STOCK-RECEIPT-001.
- OQ-015 → BR-PAY-001/SHIP-001/REFUND-001 → SM-PAYMENT-001/SHIPMENT-001/REFUND-001 → AC-UC-012-04/05/08 through 18/20/21/23; AC-UC-014-04/05/07/11 through 21/22; AC-UC-017-01/12/14. Idempotent partner callbacks and exception resolution.
- OQ-009/010 → BR-CANCEL-001/ORDER-002/AFTERSALES-001/REFUND-001/STOCK-RECEIPT-001 → SM-ORDER-001/SHIPMENT-001/DISPUTE-001/REFUND-001 → AC-UC-013-02 through 17/20; AC-UC-015-05 through 26; AC-UC-016-01 through 16; AC-UC-017-01 through 22. Closes the loop from delivery to dispute intake, physical return receipt, simulated refund execution, and exception resolution.
- Analysis/design/ADRs, implementation and executed acceptance evidence remain pending for all criteria.

## Full scope-to-catalog coverage

All scope identifiers below have catalog coverage:

- SC-001 → UC-001: Draft/Proposed account, verification, profile and address specification (OQ-020).
- SC-002 → UC-008: Draft with Accepted review hiding and Proposed listing/SKU guards; ratings depend on UC-018/OQ-013.
- SC-003 → UC-009: Draft with Accepted review hiding and Proposed detail/purchase guards; review/voucher details depend on UC-018/019/020.
- SC-004 → UC-010: Draft cart handling; checkout/reservation/price acceptance remain UC-011/OQ-007/OQ-008.
- SC-005 → UC-011: Draft selected checkout/quotes/snapshots; concrete shipping rules and voucher behavior under UC-019/020 incomplete.
- SC-006 → UC-012: Draft simulated outcomes/recovery; refund execution and exception resolution in UC-017.
- SC-007 → UC-015 Draft tracking/cancellation/completion, UC-013/014 fulfillment/Shipment, and UC-016/017 after-sales dispute/resolution.
- SC-008 → UC-016 Draft return/refund dispute intake; UC-018 review catalog only.
- SC-009 → UC-002: Draft/Proposed Shop registration, review and operator roles (OQ-004/OQ-005).
- SC-010 → UC-004/005: Draft, with category-policy dependency UC-003.
- SC-011 → UC-004/005: Draft; SKU activation and inventory boundaries remain separate.
- SC-012 → UC-007/011/012/013/015/017 Draft with Proposed quantity/reservation/confirmation/pre-confirmation closure guards and physical return restock controls.
- SC-013 → UC-013 Draft confirmation/rejection/timeout/packing/handover, linked UC-014 simulation.
- SC-014 → UC-019: catalog only, Proposed P1.
- SC-015 → UC-023: catalog only, Proposed P1.
- SC-016 → UC-002/005/006/021: Product review Draft plus Accepted hiding, withdrawal/edit/resubmit, concurrency, and repeat policy; Shop registration review Draft in UC-002; sanctions/interventions remain UC-021.
- SC-017 → UC-003: Draft/Proposed hierarchy and versioned attribute schemas (OQ-002/OQ-021).
- SC-018 → UC-021: catalog only, Proposed P1.
- SC-019 → UC-022: catalog only, Proposed P1.
- SC-020 → UC-016/017 Draft return/refund intake, physical receipt inspection, Support adjudication, and refund execution.
- SC-021 → UC-020: catalog only, Proposed P1.
- SC-022 → UC-024 Draft audit specification and each state-changing UC: NFR-004 Draft; audit criteria in UC-001 through UC-017.
- SC-023 → UC-002/006/017/020/021/022: Proposed matrix in BR-ACCESS-001 with permission checks in UC-001 through UC-017 and UC-024; UC-020/021/022 P1 detail and owner confirmation pending.
- SC-024 → UC-014 Draft single whole Shipment creation/identity/recovery.
- SC-025 → UC-014 Draft authenticated sequenced tracking/gaps/conflict/recovery and UC-015 Buyer observation.
- SC-026 → UC-012 Draft payment outcomes/recovery; UC-017 Draft simulated refund execution and payment exception reconciliation.

## Quality coverage

- NFR-001 → Shop/internal permissions and Proposed private-cart/purchase authorization; AC references in UC-004/005/006/007/010/011/012, including AC-UC-007-04, AC-UC-011-02, AC-UC-012-02; AC-UC-013-01, AC-UC-014-02, AC-UC-015-02/03 add fulfillment/private tracking denial cases; matrix pending OQ-005.
- NFR-002 → UC-012/014/017; Draft payment authenticity/replay/deadline/conflict criteria AC-UC-012-04/05/08 through 16/23; AC-UC-014-04/05/07/11 through 21/22 add logistics replay/sequence/path/conflict criteria and AC-UC-015-08 adds cancellation/success race. OQ-015 open; actual replay evidence and refund detail pending.
- NFR-003 → UC-007/011/012/013/015/017; Proposed counters/hold transition coverage AC-UC-007-02/06 through 15, AC-UC-011-04/08/09/12, AC-UC-012-03/06 through 10/23/24; AC-UC-013-03 through 12/15/16/20 and AC-UC-015-05 through 18 add confirmation/closure accounting and races. OQ-008/OQ-009/OQ-010 remain open.
- NFR-004 → UC-024 and all auditable actions; AC references in UC-004/005/006 plus AC-UC-007-16, AC-UC-011-15 and AC-UC-012-20; AC-UC-013-21, AC-UC-014-23 and AC-UC-015-21 add fulfillment/closure/logistics audit scenarios; full audit inventory/retention/failed-attempt policy pending OQ-005/OQ-012.
- NFR-005 → Draft UC-008/009; proposed workload/latency awaiting OQ-012, with executed performance evidence pending Phase 6.
- NFR-006 → UC-012/014/017; Draft AC-UC-012-13 through 18 recover same request/effect or expose reconciliation after expiry; numeric targets/retry stopping awaiting OQ-012/OQ-015, AC-UC-013-13 through 15, AC-UC-014-04/05/14/15/21 and AC-UC-015-15/16 add fulfillment/Shipment/cancellation recovery. Refund/disposition and actual recovery experiments pending.

## Phase 2 exit-gate checklist

This is the historical **full-MVP requirements coverage** inventory; OQ-014 now records Accepted baseline provenance, while the Learning Plan supersedes its broad scheduling. It does not require all listed P0 work to finish before provisional exploration or an independent ready slice proceeds. Record each named slice's relevant requirements/decision readiness in [WORK_ITEMS.md](../WORK_ITEMS.md#readiness-evidence) using the [handbook](../PRODUCT_DEVELOPMENT_PROCESS.md). No slice gate pass is recorded yet.

- [x] A glossary exists with baseline vocabulary and proposed definitions distinguished.
- [x] The catalog maps the complete Functional Scope and separates proposed P0/P1 priorities.
- [x] Initial Product-maintenance/submission/review drafts have main flows, alternatives, rule/state/quality links, and stable acceptance-criterion IDs.
- [x] OQ-014 records owner confirmation of the baseline/MVP proposal; current active scheduling is narrowed by the Learning Plan.
- [ ] Blocking moderation policies, exact validation boundaries, and permissions are resolved.
- [ ] All P0 use cases have complete specifications, including stock, checkout, payment, order, shipment, and minimal after-sales paths. Draft specifications now exist for all 18 P0 use cases; remaining completeness depends on owner decisions and review (not accepted).
- [ ] Each lifecycle has complete transitions, guards, side effects, and invalid-action criteria.
- [ ] Business rules and NFRs are measurable and confirmed for the MVP.
- [ ] Every P0 rule/state/NFR has acceptance coverage; cross-artifact consistency and owner review are complete.

Current conclusion: **Whole-MVP Phase 2 gate not passed; Part 1 provisional Phase 3 in progress.** Open decisions are tracked in [OPEN_DECISIONS.md](OPEN_DECISIONS.md). Once requirements are accepted, analysis/design and later executable tests extend this chain rather than changing stable IDs.
