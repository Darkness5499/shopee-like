# Documentation Map

- Status: Draft index, updated 2026-10-07.
- Owner: Project owner.

This repository follows a requirements-led, iterative product-development workflow. Committed analysis/design/implementation must trace to sufficiently clear relevant requirements. Provisional UX, domain sketches, and feasibility experiments can refine Draft requirements; unresolved decisions remain visible before any dependent gate is passed.

## Resume work

1. Read [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for current facts and the next action.
2. Read the relevant entry in [WORK_ITEMS.md](WORK_ITEMS.md) for goals, dependencies, risks, and readiness evidence.
3. Follow [PRODUCT_DEVELOPMENT_PROCESS.md](PRODUCT_DEVELOPMENT_PROCESS.md) and load the linked canonical sources needed for the task.
4. Consult [WORK_LOG.md](WORK_LOG.md) for history; update the snapshot/work item and append a handoff after meaningful changes.

The handbook defines 11 lifecycle activities within the existing six phases. These are repeatable activities for a selected journey/slice, not a whole-product serial checklist. [AGENTS.md](../AGENTS.md) contains the shared startup and handoff instructions.

## Current position

The project is in **Phase 2 — Requirements Specification, in progress**, corresponding to activity 3. Phase 1 documents are the existing working baseline; a separate owner sign-off record is not present in this repository. The phase number does not prohibit provisional discovery or appropriately ready work in other activities.

The glossary and scope-wide catalog are available. Twelve Draft use cases connect Seller Product preparation/submission, Moderator review, Buyer discovery/detail/cart, inventory, checkout, simulated payment, Seller fulfillment and Buyer delivery/tracking/cancellation. UC-015 completion remains incomplete. Accepted [BR-PROD-002](02-requirements/BUSINESS_RULES.md#br-prod-002) and [BR-PROD-003](02-requirements/BUSINESS_RULES.md#br-prod-003) visibility/withdrawal policy is preserved. Proposed [BR-PROD-004](02-requirements/BUSINESS_RULES.md#br-prod-004) defines public/purchase eligibility; [purchase rules](02-requirements/BUSINESS_RULES.md#br-inv-001) and separate inventory/reservation/order/payment models specify pending stock/payment choices. [WI-002](WORK_ITEMS.md#wi-002) Draft preparation is complete; [WI-004](WORK_ITEMS.md#wi-004) Draft preparation is complete with Proposed fulfillment/closure/Shipment models and 68 new criteria. Both WI-002/WI-004 commitment checks are Not passed; [WI-005](WORK_ITEMS.md#wi-005) continues completion/after-sales/financial disposition after review of linked choices. MVP, purchase policy, validation/permissions/SKU boundaries and quality targets remain proposals or open. Phase 2 has **not passed its exit gate**.

Last normalization review: **2026-10-07**. See the [requirements index](02-requirements/README.md), [open decisions](02-requirements/OPEN_DECISIONS.md), and [traceability register](02-requirements/TRACEABILITY.md).

## Directory structure

- `01-product/`: product discovery and scope baseline.
- `02-requirements/`: testable functional and non-functional requirements.
- `03-analysis/`: domain and process analysis derived from requirements; planned.
- `04-solution-design/`: architecture and implementation-facing design; planned.

Planned locations are `05-delivery/` for work-item-linked implementation/test/environment evidence and `06-verification/` for release checks, demo evidence, runbooks, and learning. They are documented conventions, not directories or completed artifacts created by this process revision. UX artifacts will live under `04-solution-design/UX/` when needed, with Draft/provisional status during discovery.

## Canonical references

- [Product vision](01-product/PRODUCT_VISION.md)
- [Stakeholders and actors](01-product/STAKEHOLDERS_AND_ACTORS.md)
- [Functional scope](01-product/FUNCTIONAL_SCOPE.md)
- [Glossary](02-requirements/GLOSSARY.md)
- [Use case catalog](02-requirements/USE_CASE_CATALOG.md)

Document statuses and identifier conventions are defined in [Documentation Architecture](DOCUMENTATION_ARCHITECTURE.md).

See [PRODUCT_DEVELOPMENT_PROCESS.md](PRODUCT_DEVELOPMENT_PROCESS.md) for phase gates and required artifacts.
See [DOCUMENTATION_ARCHITECTURE.md](DOCUMENTATION_ARCHITECTURE.md) for the rationale and ownership of each document group.
