---
name: marketplace-product-delivery
description: Guides requirements-led, iterative delivery and persistent project context for this Shopee-like marketplace demo. Use for product scope, requirements, domain and solution design, delivery planning, readiness checks, or resuming project work.
---

# Marketplace Product Delivery

## Purpose

Keep decisions traceable to user needs while delivering in small, complete journeys across actors. The repository's process is a project convention, not a claim of certification or a universally mandated sequence.

## Start and resume

Read `docs/PROJECT_CONTEXT.md`, `docs/README.md`, and `docs/PRODUCT_DEVELOPMENT_PROCESS.md` to establish the current phase, selected slice, and readiness evidence. Read the relevant `WI-###` entries in `docs/WORK_ITEMS.md` and the latest relevant handoff in `docs/WORK_LOG.md`, then load the canonical requirements and decisions those entries reference. Do not reload every project artifact on every turn; expand only where the task or a dependency requires it.

Use `docs/PROJECT_CONTEXT.md` as a navigation and continuity snapshot. Canonical definitions and explicit decision provenance take precedence over a stale summary. Reconcile discrepancies and record the correction without rewriting prior history.

## Canonical documentation

`docs/PRODUCT_DEVELOPMENT_PROCESS.md` owns objectives, activities, inputs, outputs, responsibilities, readiness checks, and feedback loops. Follow it rather than maintaining a second process manual here.

- `docs/01-product/`: product discovery and scope baseline.
- `docs/02-requirements/`: requirements, rules, states, and acceptance criteria.
- `docs/03-analysis/`: domain and process analysis.
- `docs/04-solution-design/`: architecture and implementation-facing design.
- `docs/05-delivery/` and `docs/06-verification/`: planned locations for delivery and verification artifacts; create files when the selected work needs them. A planned location is not evidence of completed work.

Treat the corresponding files under these folders as canonical. `docs/PROJECT_CONTEXT.md`, `docs/WORK_ITEMS.md`, and `docs/WORK_LOG.md` coordinate work and link to those definitions; do not duplicate normative product policy in them or create parallel root-level product documents.

`docs/02-requirements/OPEN_DECISIONS.md` is the central register of unresolved product, requirements, domain, and architecture decisions. Local Open questions sections link to its `OQ-###` entries; they do not maintain independent copies of the same decision. ADRs preserve technical rationale and link to relevant open decisions.
`docs/02-requirements/TRACEABILITY.md` records links from scope through requirements and acceptance criteria to later analysis, design, tests, and implementation. A missing downstream artifact is recorded as pending, not invented to complete the chain.

## Iteration and readiness

Preserve the six repository phases. The eleven lifecycle activities map as follows: Discovery and Definition/MVP → Phase 1; Requirements → Phase 2; Domain → Phase 3; UX/UI, Architecture, and Detailed Design → Phase 4; Implementation → Phase 5; Quality/Security/Performance, Release, and Operate/Learn → Phase 6. Use the process document for the details.

Evaluate implementation readiness for an explicitly named slice; evaluate a phase or release gate against its declared scope in the process document. A slice readiness result does not pass an entire phase gate or require every capability in the catalog to be specified first. Record actual evidence, accountable owner, unresolved dependencies, and the next check; never infer a passed gate from document existence.

Draft UX flows, prototypes, API sketches, and reversible technical feasibility spikes may run alongside requirements discovery. Label them provisional, link their requirement assumptions and `OQ-###` dependencies, and state what they validate. Findings can revise requirements, domain models, scope proposals, and designs. A prototype or spike does not establish product policy or production readiness.

Before implementing committed behavior for the selected slice, check that its user goal, flows, rules/states, applicable NFRs, acceptance criteria, and material dependencies are clear enough to implement and verify. Keep unresolved decisions visible and progress on work independent of them. Do not silently turn an unconfirmed policy into committed behavior or require unrelated future capabilities to be specified first. Apply the release checks in the process document before an actual release.

## Requirement rules

- Use stable identifiers: `SC-###` for scope, `UC-###` for use cases, `BR-{DOMAIN}-###` for business rules, `SM-{ENTITY}-###` for state models, `NFR-###`, `ADR-###`, and `OQ-###` for open decisions. Preserve the existing `BR-PROD-001`; the product state model uses `SM-PRODUCT-001`.
- Give each acceptance criterion an ID `AC-UC-###-##`, using the owning use case number and a stable criterion sequence. Do not reuse retired IDs.
- Identify document Status and Owner. Add Scope and Source for specifications, and Priority where prioritization applies; entries may inherit shared metadata. Rule/state/requirement entries with a different status identify it locally.
- State requirements in observable, testable language.
- Separate business requirement from solution/design choice.
- Record assumptions and open questions explicitly; do not silently invent product policy.
- Maintain traceability: scope → use case → rule/state/NFR → acceptance criterion → domain/design → acceptance test. Use links to canonical definitions rather than duplicating their normative text.
- Update related artifacts when a decision changes.
- Preserve existing accepted rules, recorded priority provenance, and MVP boundary. Refining the process does not accept pending proposals or change P0/P1 priorities.

## Status and confirmation

- **Baseline:** content carried forward from existing project documents; explicit owner sign-off provenance is not recorded. Baseline is not a new acceptance claim.
- **Draft:** content being specified or checked; incomplete flows and unresolved decisions remain visible.
- **Proposed:** a new policy, priority, or measurable target awaiting an explicit project-owner decision.
- **Accepted:** content explicitly confirmed by the project owner or decided under the owner's recorded, applicable delegation of decision authority. Record the confirmation or delegation source and date; never infer acceptance from file existence, phase position, or general authorization to prepare a draft.

Document status does not override the status of a linked proposal. Phase 2 may start with baseline product scope and draft requirements; phase or slice readiness is a separate evidence check. Work-item execution states such as In progress or Done do not confer Accepted status on a requirement, proposal, or gate.
Reviewing, normalizing, linking, or creating draft documentation within the user's request does not require a separate approval. Continue that work while recording proposed product choices in `OPEN_DECISIONS.md`. Changing a proposal to Accepted needs explicit confirmation or applicable recorded decision delegation; preparing it for review does not. Routine reversible implementation choices within authorized scope do not need renewed permission.

## Working and handing off

1. Identify the current phase, selected slice, work item, and canonical sources relevant to the request. Continue the user's authorized work without repeated permission requests for drafting or reversible tasks.
2. Progress on review, specification, design, or implementation at the readiness level supported by evidence. Record new policy, priority, or target choices as Proposed and link an `OQ-###` entry.
3. When a decision is explicitly confirmed or made under applicable recorded delegation, update its canonical definition, decision entry, and affected traceability; preserve the confirmation or delegation source and date.
4. After actual changes, update `docs/PROJECT_CONTEXT.md` with the current position, `docs/WORK_ITEMS.md` with work state, evidence, risks, and next action, and append a dated handoff in `docs/WORK_LOG.md`. Preserve earlier history and identify checks that were not run. A read-only discussion needs no fabricated completion entry.
5. End with the concrete result, verification evidence or limitations, and the next actionable step or unresolved dependency. Keep product decisions in the canonical register, not only in chat or the handoff.

## Templates

Use [TEMPLATES.md](TEMPLATES.md) when creating a corresponding artifact. It includes concise optional templates for specifications, work items, readiness evidence, handoffs, and risks; complete only fields relevant to the work.
