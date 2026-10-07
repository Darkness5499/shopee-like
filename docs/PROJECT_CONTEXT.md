# Project Context

- Status: Draft working snapshot; verified against repository artifacts on 2026-10-07.
- Owner: Project owner; maintainers update this snapshot during project work.
- Purpose: restart work without reconstructing the entire conversation.

This file summarizes current facts and links to their canonical sources. If a summary conflicts with a source, inspect the source and decision provenance, then correct this snapshot. It does not create product policy or approve a gate.

## Product and working preferences

- Build a multi-category Shopee-inspired marketplace **learning demo** covering complex business journeys and software delivery. See [Product Vision](01-product/PRODUCT_VISION.md).
- Payment Provider and Logistics Partner are simulated. Real money, real carrier integration, and production-scale operation are outside the current baseline.
- One User may act as Buyer and Seller / Shop Operator. Shop is a business entity. Internal Staff have separate permission responsibilities; exact account/permission boundaries remain open.
- Owner discussion is Vietnamese; canonical repository documentation remains English.
- Work by connected journeys across actors, refining prerequisites alongside the journey; do not finish an entire actor's feature list before considering another actor.
- User request on 2026-10-07: complete the product-development process with each activity's purpose and preserve project context for smooth continuation. This authorizes the workflow/documentation revision; it does not resolve unrelated product proposals.
- Later requests on 2026-10-07 authorize the next project step and continued work. Owner asks for a new context window if needed on context overflow or new functionality/requirements; persist a handoff before switching context or work scope. No new product-policy acceptance is implied.

## Current position and evidence

- **Phase 2 — Requirements Specification, in progress**, corresponding to activity 3 in the [11-activity handbook](PRODUCT_DEVELOPMENT_PROCESS.md). The six existing phase numbers remain in use.
- Phase 1 documents are a working Baseline; formal owner sign-off is not recorded. The MVP/P0/P1 proposal remains open under [OQ-014](02-requirements/OPEN_DECISIONS.md#oq-014).
- There are 26 scope capabilities and 24 catalog entries; fourteen use cases have detailed **Draft** specifications: UC-004 through UC-017 (229 criteria total). WI-005 contributes 42 criteria across UC-015 completion, UC-016 return/refund dispute intake, and UC-017 dispute adjudication/restock/refund execution. See the [requirements index](02-requirements/README.md).
- BR-PROD-001 is Baseline; BR-PROD-002/003 have Accepted policy; BR-PROD-004, inventory/purchase/fulfillment/cancellation/shipment/after-sales/refund rules are Proposed. Product lifecycle is Draft; separate inventory/reservation/Order/payment/Shipment/dispute/refund models are Proposed. The six NFRs remain Draft/Proposed.
- No whole use case or phase gate has been accepted. WI-002, WI-004, and WI-005 Draft preparation are Done; commitment-readiness checks are Not passed because dependent policy is unresolved. Acceptance criteria are specifications, not executed test evidence.
- Domain analysis and solution design contain planned indexes. Application implementation, deployment, and operational evidence have not started; no stack or API/database design is selected.
- Git currently has no commits; documentation and project guidance are local working-tree files. File persistence is not a remote backup.

## Confirmed decisions to preserve

- [BR-PROD-002](02-requirements/BUSINESS_RULES.md#br-prod-002): successful saving of an actual high-risk Product change immediately hides the Product and blocks new purchases until revised content obtains manual approval. Rejection does not automatically restore previously approved content. See Accepted [OQ-001](02-requirements/OPEN_DECISIONS.md#oq-001), [OQ-016](02-requirements/OPEN_DECISIONS.md#oq-016), and [OQ-017](02-requirements/OPEN_DECISIONS.md#oq-017).
- [BR-PROD-003](02-requirements/BUSINESS_RULES.md#br-prod-003): withdraw a pending submission before further risky editing; resubmission is validated again. First accepted withdrawal/approval/rejection wins, conflicting actions are refused, and repeats have no additional effects. See Accepted [OQ-003](02-requirements/OPEN_DECISIONS.md#oq-003) and [OQ-018](02-requirements/OPEN_DECISIONS.md#oq-018).

These confirmations accept their recorded policy boundaries, not all use cases, proposed UI behavior, or state-code mappings. Exact normative definitions remain in the linked sources.

## Decisions affecting upcoming work

All unresolved decisions remain canonical in [OPEN_DECISIONS.md](02-requirements/OPEN_DECISIONS.md). The following are immediate dependencies, not new decisions:

- Publication/cart consistency: OQ-002 validation, OQ-005 permissions, OQ-006 SKU identity, and OQ-019 listing versus purchase eligibility. Drafting can continue while dependent behavior remains Proposed.
- Purchase and fulfillment journey: OQ-007/OQ-008/OQ-009/OQ-010/OQ-015 now contain concrete options and recommended Proposed choices across checkout grouping, paid holds, Seller confirmation, pre-confirmation closure with per-Shop REQUIRED compensation, simulated Shipment tracking, delivery completion, dispute intake, physical return restock controls, and cumulative verified-funds refund caps.
- Release boundary and measurements: OQ-014 MVP priorities and OQ-012 demo quality targets. No confirmed schedule or effort budget is recorded.

## Next action

Review the prepared purchase, fulfillment, and after-sales choices in [OQ-007](02-requirements/OPEN_DECISIONS.md#oq-007), [OQ-008](02-requirements/OPEN_DECISIONS.md#oq-008), [OQ-009](02-requirements/OPEN_DECISIONS.md#oq-009), [OQ-010](02-requirements/OPEN_DECISIONS.md#oq-010) and [OQ-015](02-requirements/OPEN_DECISIONS.md#oq-015) with the project owner to achieve policy confirmation for the core MVP journey. [WI-005](WORK_ITEMS.md#wi-005) Draft preparation is complete; see detailed sources in its [handoff](WORK_LOG.md#handoff-2026-10-07-aftersales).

Proceed with [WI-003](WORK_ITEMS.md#wi-003) release-boundary and demo quality targets review ([OQ-012](02-requirements/OPEN_DECISIONS.md#oq-012)/[OQ-014](02-requirements/OPEN_DECISIONS.md#oq-014)). Once Phase 2 requirements are approved by the owner, initiate Phase 3 (Domain and Process Analysis). Authorized Draft/provisional work can continue while choices remain open; committed dependent implementation needs recorded choices and its scoped readiness check. No application or acceptance-test execution has started.

## Context maintenance

- **Current truth:** this file. Keep it compact and refresh it when state or next action changes.
- **Work, risk, and readiness evidence:** [WORK_ITEMS.md](WORK_ITEMS.md).
- **Dated history and handoff:** [WORK_LOG.md](WORK_LOG.md); append rather than replacing earlier entries.
- **Product decision provenance:** [OPEN_DECISIONS.md](02-requirements/OPEN_DECISIONS.md); technical decisions use [ADRs](04-solution-design/ADR/README.md).
- **Coverage:** [TRACEABILITY.md](02-requirements/TRACEABILITY.md).

At session start, read this snapshot and the affected work item, then load the linked artifacts needed for that task. At session end, update canonical sources first, synchronize this snapshot/work items, and append a handoff with verification and the next action. Read-only questions do not need a new log entry. Never copy secrets or private credentials into context files.
