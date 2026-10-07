# Work Items, Risks, and Readiness

- Status: Draft coordination register, initialized 2026-10-07.
- Owner: Project owner; contributors maintain work and evidence.
- Source: existing [requirements next work](02-requirements/README.md#next-work) and the owner's workflow/context request on 2026-10-07.

This register coordinates work; it does not replace the use case catalog, decide MVP priorities, or accept product policy. Item states are Planned, In progress, Blocked, or Done. A planned item order is a suggested continuation, separate from Proposed P0/P1 priorities. Blocked records the specific dependent task; unrelated authorized work can continue.

## Work items

<a id="wi-001"></a>
### WI-001 — Complete the project workflow and persistent context

- State: Done — documentation/guidance work only.
- Owner: Project owner; prepared by the coding assistant at the owner's request.
- Goal: give every lifecycle activity a purpose, minimum output, readiness criteria, and feedback path; preserve context between sessions.
- Boundary: process, documentation architecture, indexes, project instructions, and local delivery skill. Existing Accepted product policy is retained.
- Outputs: [handbook](PRODUCT_DEVELOPMENT_PROCESS.md), [documentation architecture](DOCUMENTATION_ARCHITECTURE.md), [project context](PROJECT_CONTEXT.md), this work register, [handoff log](WORK_LOG.md), [project instructions](../AGENTS.md), and [delivery skill](../.cursor/skills/marketplace-product-delivery/SKILL.md).
- Completion evidence: all 11 activities documented; legacy six phases mapped; startup/handoff sources linked; local documentation links and skill validation checked. Verification details are recorded in the 2026-10-07 [handoff](WORK_LOG.md#handoff-2026-10-07-process).
- Readiness assessed: documentation consistency only. No requirements, implementation, or release gate is passed by this item.
- Next action: use WI-002 when requirements work resumes; update the workflow when project experience reveals a needed adjustment.

<a id="wi-002"></a>
### WI-002 — Specify inventory, checkout, and simulated payment as one journey

- State: Done — Draft specification and decision-review preparation only, completed 2026-10-07; policy acceptance and implementation are pending.
- Owner: Project owner for product choices; analyst/assistant for Draft specifications.
- Goal: connect Buyer purchasing with stock reservation/release/deduction and simulated payment outcomes.
- Boundary: UC-007/011/012, with downstream UC-013/014/015 consequences and prerequisite permissions/SKU/validation rules made explicit.
- References: [catalog](02-requirements/USE_CASE_CATALOG.md), [business rules](02-requirements/BUSINESS_RULES.md), [state machines](02-requirements/STATE_MACHINES.md), [NFRs](02-requirements/NON_FUNCTIONAL_REQUIREMENTS.md), and [traceability](02-requirements/TRACEABILITY.md).
- Decisions: [OQ-007](02-requirements/OPEN_DECISIONS.md#oq-007), [OQ-008](02-requirements/OPEN_DECISIONS.md#oq-008), and [OQ-015](02-requirements/OPEN_DECISIONS.md#oq-015), plus affected OQ-002/005/006/009/019. These block commitment to dependent policy, not preparing alternatives or drafts.
- Minimum outputs: linked detailed UCs; separate inventory/reservation/order/payment state definitions where needed; invariants and failure/race scenarios; stable acceptance criteria; updated coverage/decision impacts.
- Completion check: sources and Proposed choices are distinguished; success/failure/expiry/late-callback paths are coherent across actors; relevant permission and audit requirements are covered; unresolved dependencies are linked. This is Draft specification completion, not automatic product acceptance.
- Results: linked [UC-007](02-requirements/USE_CASES/UC-007-manage-inventory.md), [UC-011](02-requirements/USE_CASES/UC-011-checkout.md) and [UC-012](02-requirements/USE_CASES/UC-012-process-payment.md), with 58 stable criteria; Proposed BR-INV-001/ORDER-001/PAY-001 and separate inventory/reservation/purchase-order/payment models. OQ-007/008/015 contain concrete alternatives and recommended working choices, preserving Seller confirmation as separate from payment success.
- Checks: independent read-only consistency review checked accounting, proposal status, races and recovery; callback classification and repeat-before-new-action guards were clarified. Final documentation validation is recorded in the [purchase handoff](WORK_LOG.md#handoff-2026-10-07-purchase). Acceptance tests are not executed; no application exists.
- Unresolved: paid-hold/Seller timeout, cancellation/rejection/refund and exception disposition; exact shipping/voucher/money/quantity rules, SKU identity and permission matrix. A conditional simulated-shipping/no-voucher walkthrough is not full checkout acceptance or complete fulfillment coverage.
- Readiness: Not passed for commitment to the specified purchase slice; assessed 2026-10-07 against activity 3 in the [handbook](PRODUCT_DEVELOPMENT_PROCESS.md#3-requirements-engineering). Important dependent policy remains Proposed/Open. Draft preparation is complete; provisional exploration may continue.
- Next action: review OQ-007/008/015 alongside the completed WI-004 closure/logistics proposal; continue WI-005 completion/after-sales/financial disposition. WI-003 still prepares release/quality choices.

<a id="wi-003"></a>
### WI-003 — Review the initial demo boundary and measurable goals

- State: Planned — existing unresolved owner decisions.
- Owner: Project owner; analyst/assistant prepares a concrete review package.
- Goal: establish what the first end-to-end demo must prove and how success will be measured within the available effort.
- References: [Product Vision](01-product/PRODUCT_VISION.md), [Functional Scope](01-product/FUNCTIONAL_SCOPE.md), [catalog priorities](02-requirements/USE_CASE_CATALOG.md#prioritization-proposal), [OQ-014](02-requirements/OPEN_DECISIONS.md#oq-014), and [OQ-012](02-requirements/OPEN_DECISIONS.md#oq-012).
- Outputs: proposed release journey and exclusions/deferred capabilities; agreed effort constraints if supplied; metric definitions/measurement plan; recorded owner choices or explicit remaining proposals.
- Completion check: proposed scope is reviewable against the learning goal; chosen decisions have provenance; no P1 capability is silently removed from the product baseline.
- Readiness: not assessed; MVP priorities and quality targets remain Proposed.
- Next action: prepare the release-boundary/quality proposal from canonical artifacts, then obtain or apply explicit owner decision authority where required. WI-004 Draft work can proceed alongside this review.

<a id="wi-004"></a>
### WI-004 — Specify paid-order confirmation, fulfillment, and tracking

- State: Done — Draft specification and decision-review preparation only, completed 2026-10-07; policy acceptance, completion/after-sales and implementation remain pending.
- Owner: Project owner for policy; analyst/assistant for Draft specifications.
- Goal: connect paid reservations to Seller confirmation/packing/handover, simulated delivery, Buyer tracking/cancellation, and their stock/payment consequences.
- Boundary: detailed UC-013/014/015 with necessary UC-007/011/012 updates; minimal after-sales consequences linked to UC-016/017 without silently defining refunds.
- References: [purchase specifications](02-requirements/README.md), [rules](02-requirements/BUSINESS_RULES.md#br-inv-001), [state models](02-requirements/STATE_MACHINES.md#sm-order-001) and [traceability](02-requirements/TRACEABILITY.md#purchase-decision-impact-and-missing-downstream-work).
- Dependencies: review Proposed OQ-007/008/015 and resolve dependent OQ-009/010; authorization/SKU/validation remain OQ-002/005/006, shipping quote policy OQ-007 and logistics events OQ-015. Material choices require owner confirmation or applicable explicit delegation. Drafting alternatives may proceed while decisions remain open.
- Minimum outputs: Seller/Buyer/partner main and failure flows; per-Shop confirmation and paid-hold timeout options; cancellation/rejection/sibling-payment consequences; separate shipment/order updates and late/duplicate tracking cases; stable criteria and linked rule/state/decision impacts.
- Completion check: the paid-to-fulfillment path and failure/timeout/cancellation consequences are coherent and reviewable; unanswered after-sales and permission rules remain explicit. Draft completion is distinct from acceptance or a passed gate.
- Results: [UC-013](02-requirements/USE_CASES/UC-013-fulfill-shop-order.md), [UC-014](02-requirements/USE_CASES/UC-014-simulate-shipment.md) and [UC-015](02-requirements/USE_CASES/UC-015-track-cancel-orders.md) contain 22/24/22 stable Proposed criteria (68 total). Added Proposed BR-ORDER-002/CANCEL-001/SHIP-001 and SM-SHIPMENT-001; extended Order/reservation/payment models and upstream UC-007/011/012 bridges. OQ-009/010/015 now have concrete alternatives and recommended choices, retaining independent per-Shop fulfillment/closure after group payment.
- Checks: independent read-only reviews covered inventory/compensation/cutoff, Buyer intent/isolation and logistics sequence/graph/recovery. Corrected the packed-only Shipment guard, unpaid cancellation replay without a compensation obligation, stale pending links and equivalent no-op sequence handling. Documentation validation and preservation results are recorded in the [fulfillment handoff](WORK_LOG.md#handoff-2026-10-07-fulfillment). No application/simulator execution or acceptance tests exist.
- Unresolved: confirm purchase/closure/logistics choices and the candidate Seller deadline (24 hours from applied success); permissions, restrictions, SKU/units/money/shipping/vouchers, protocol/query/retry/retention. Buyer completion is not specified; UC-016/017 refund execution/financial disposition, return receipt/restock and staff intervention remain pending. REQUIRED compensation is an observable obligation, not a completed refund or passed gate.
- Readiness: **Not passed** for commitment to this downstream slice; assessed 2026-10-07 against activity 3 of the handbook. Reviewable flows/criteria are drafted, but material policy is still Proposed/Open and detailed dependencies/financial closure remain missing. Draft preparation is complete.
- Next action: review OQ-007/008/009/010/015 using the linked options, then continue WI-005 completion/after-sales/financial disposition and refine affected permission/SKU/validation/shipping prerequisites. WI-003 release/quality review remains Planned; independent Draft/provisional work can continue.

<a id="wi-005"></a>
### WI-005 — Specify completion, after-sales, and financial disposition

- State: Done — Draft specification and decision-review preparation only, completed 2026-10-07; policy acceptance and implementation remain pending.
- Owner: Project owner for policy; analyst/assistant for Draft preparation.
- Goal/boundary: define UC-015 Buyer/system completion, UC-016 return/refund/dispute intake and UC-017 decisions/simulated execution/closure, connecting paid-Shop compensation obligations, late/conflicting payment evidence and returned delivery to stock receipt and financial caps.
- References: [catalog](02-requirements/USE_CASE_CATALOG.md), [OQ-010](02-requirements/OPEN_DECISIONS.md#oq-010), [BR-CANCEL-001](02-requirements/BUSINESS_RULES.md#br-cancel-001), [BR-AFTERSALES-001](02-requirements/BUSINESS_RULES.md#br-aftersales-001), [BR-REFUND-001](02-requirements/BUSINESS_RULES.md#br-refund-001), [BR-STOCK-RECEIPT-001](02-requirements/BUSINESS_RULES.md#br-stock-receipt-001), [payment](02-requirements/STATE_MACHINES.md#sm-payment-001), [Shipment](02-requirements/STATE_MACHINES.md#sm-shipment-001), [dispute](02-requirements/STATE_MACHINES.md#sm-dispute-001), and [refund](02-requirements/STATE_MACHINES.md#sm-refund-001) models, [traceability](02-requirements/TRACEABILITY.md#purchase-and-fulfillment-decision-impact).
- Dependencies: OQ-009/010/015 closure/exception/return/completion choices; OQ-005 staff/Buyer/Seller authority; OQ-002/007/011 amount/shipping/voucher definitions.
- Minimum outputs: explicit completion versus delivery; eligibility/windows/evidence/authority; separate Return/Refund/Dispute/compensation/receipt states; verified-funds and prior-execution caps; stable refund identity and recovery; no automatic stock restoration without verified physical receipt; linked criteria/rules/decisions/traceability.
- Results: extended [UC-015](02-requirements/USE_CASES/UC-015-track-cancel-orders.md) with 4 new criteria (26 total) for delivery completion and 7-day auto-completion timeout; created [UC-016](02-requirements/USE_CASES/UC-016-request-return-refund.md) with 16 criteria for return/refund intake; created [UC-017](02-requirements/USE_CASES/UC-017-resolve-refund-dispute.md) with 22 criteria for pre-confirmation compensation execution, dispute adjudication, physical return restock controls, and financial caps (42 new criteria; 229 total across 14 detailed UCs). Added Proposed BR-AFTERSALES-001, BR-REFUND-001, BR-STOCK-RECEIPT-001, SM-DISPUTE-001, and SM-REFUND-001. Updated OQ-010 with concrete options and recommended working choices.
- Checks: independent read-only consistency review checked delivery completion vs after-sales windows, single dispute concurrency, physical return inspection/restock invariants, cumulative paid funds cap, and idempotent refund recovery. Documentation validation recorded in the [after-sales handoff](WORK_LOG.md#handoff-2026-10-07-aftersales). No application code or acceptance tests exist.
- Readiness: **Not passed** for commitment to this downstream slice; assessed 2026-10-07 against activity 3 of the handbook. Reviewable flows and criteria are drafted, but underlying policy choices remain Proposed/Open. Draft preparation is complete.
- Next action: review open decisions with project owner (OQ-007, OQ-008, OQ-009, OQ-010, OQ-014, OQ-015) to achieve policy confirmation for the core MVP journey. Proceed with [WI-003](WORK_ITEMS.md#wi-003) release-boundary and quality targets review. Phase 2 exit gate remains open.

## Current delivery risks

Risks identify consequences and mitigation, while normative unanswered policy stays in the OQ register. Qualitative severity is an initial planning assessment, not a measured score.

<a id="risk-001"></a>
### RISK-001 — Unconfirmed release boundary can cause excessive first-release scope

- State: Open; impact: High for effort/rework.
- Evidence: 26 capabilities are cataloged, but [OQ-014](02-requirements/OPEN_DECISIONS.md#oq-014) is Open and P0/P1 priorities are Proposed.
- Mitigation: WI-003 defines a bounded reviewable demo journey and deferrals, keeping full scope visible.
- Owner: Project owner; review trigger: selecting a committed implementation slice or changing the release plan.
- Closure evidence: recorded release boundary/priority decision and a linked scoped work plan.

<a id="risk-002"></a>
### RISK-002 — Unresolved stock/payment/event semantics can produce inconsistent purchases

- State: Mitigating; impact: High for domain correctness.
- Evidence: [OQ-007](02-requirements/OPEN_DECISIONS.md#oq-007), [OQ-008](02-requirements/OPEN_DECISIONS.md#oq-008), and [OQ-015](02-requirements/OPEN_DECISIONS.md#oq-015) remain Open. WI-002 provides Draft criteria and Proposed states/invariants; WI-004 now adds Proposed paid-hold timeout/cancellation and stable required compensation. Actual refund/exception disposition, accepted policy and executable evidence remain missing.
- Mitigation: review the WI-002 proposed choices; WI-004 connects protected paid stock to Seller timeout/rejection/cancellation; review its proposals and use WI-005 to specify financial disposition/receipt before committing dependent design. Verify contention/replays later when code exists.
- Owner: analyst/assistant for specification; project owner for product policy; review trigger: purchase-state or integration design.
- Closure evidence: coherent accepted policy and linked criteria/design; executable verification is recorded when implementation exists.

<a id="risk-003"></a>
### RISK-003 — Cross-shop/internal permission gaps can undermine the demo's isolation

- State: Open; impact: High for correctness/security.
- Evidence: [OQ-005](02-requirements/OPEN_DECISIONS.md#oq-005) remains Open; [NFR-001](02-requirements/NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001) is Draft/Proposed.
- Mitigation: specify allowed/denied actions alongside each journey and later verify cross-shop access with negative tests.
- Owner: analyst/assistant for requirements; implementer/reviewer for controls; review trigger: shop-owned resource or staff action design.
- Closure evidence: recorded permission matrix and corresponding acceptance/verification evidence for the selected slice.

## Readiness evidence

- Current overall conclusion: Phase 2 is in progress; the full-MVP requirements gate in [TRACEABILITY.md](02-requirements/TRACEABILITY.md#phase-2-exit-gate-checklist) has not passed.
- No selected slice has a recorded requirements/design/build/release gate pass. WI-002 and WI-004 commitment readiness were assessed as Not passed on 2026-10-07; see the scoped evidence below.
- WI-001 proves documentation delivery only; a Done work item is not a product-decision approval.
- Before recording a gate outcome, state its scope (WI/UC/release), criterion, evidence, assessor/date, unresolved dependencies, and next action. Use Passed, Not passed, or Not assessed for the assessment; documentary acceptance is tracked separately.
- A full-MVP checklist does not block exploratory work or independent ready slices. Block only a dependent commitment when its scoped requirements or critical decisions remain unresolved.

### WI-002 purchase requirements commitment check — 2026-10-07

- Scope: UC-007/011/012 plus linked inventory/reservation/purchase-order/payment rules/models at this Draft revision; excludes a claim of complete UC-013 fulfillment or full SC-005 voucher readiness.
- Result: **Not passed** for committed downstream purchase implementation; documentation delivery is recorded separately as Done.
- Assessor/accountable roles: assistant assessed repository evidence; project owner remains accountable for product-policy acceptance.
- Criteria/evidence: three detailed UCs have goals, flows, errors, protected actor boundaries and 58 stable criteria; canonical rule/state/decision/coverage links exist; independent review corrected callback conflict precedence and lost-response replay ordering. Policy-resolution criterion is unmet: OQ-007/008/015 remain Proposed/Open, and concrete shipping/discount/money/units/permissions/SKU and paid-order closure still depend on OQ-002/005/006/009/010/011. No implementation or acceptance execution exists.
- Decision provenance: 2026-10-07 requests authorize continuation and persistent handoff; they do not accept these proposed product choices. Accepted BR-PROD-002/003 policy is preserved.
- Next check: after relevant owner choices and scoped dependency refinements are recorded, assess the specifically selected slice again; continue independent Draft/provisional work now under WI-004 and WI-003.

### WI-004 fulfillment requirements commitment check — 2026-10-07

- Scope: UC-013/014/015 tracking/cancellation portion plus linked fulfillment/closure/Shipment proposals and necessary purchase bridges at this Draft revision; excludes Buyer completion, UC-016/017 refund execution/return receipt, full shipping/voucher policy and exceptional staff interventions.
- Result: **Not passed** for commitment to this downstream implementation slice. Draft delivery is separately Done.
- Assessor/accountable roles: assistant assessed repository evidence; project owner remains accountable for product choices.
- Criteria/evidence: three Draft UCs with actor boundaries, main/error/recovery flows and 68 stable criteria; separate Order/payment/Shipment/reservation meanings; inventory/compensation all-or-none outcomes and sibling scope; independent reviews and corrected guard/replay/sequence findings. Sources, decision alternatives and traceability are linked. This meets reviewable Draft preparation, not the policy-resolution requirement.
- Unmet/dependent criteria: OQ-007/008/009/010/015 remain Proposed/Open; exact permissions/SKU/validation/money/units/shipping/vouchers/authority/retention remain unresolved. UC-015 completion, UC-016/017 financial execution/disposition and consumed-stock receipt have no detailed accepted transitions or executed checks. No code/simulator/acceptance execution exists.
- Decision provenance: owner's 2026-10-07 request authorizes WI-004 continuation/Draft preparation, without confirming proposed policy or extending moderation-only OQ-003 decision authority. Accepted BR-PROD-002/003 and five Accepted OQ entries remain preserved.
- Next check: record relevant owner choices and refine named dependent paths, then reassess the selected slice. Continue WI-005 Draft/provisional exploration and WI-003 release/quality review independently; the full-MVP/Phase 2 gate remains unpassed.
