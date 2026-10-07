# Documentation Architecture

- Status: Draft — project documentation convention updated on 2026-10-07.
- Owner: Project owner.

## Purpose

This repository uses a lifecycle-based, docs-as-code documentation structure. Documentation lives with source code, is versioned in Git, and is organized by the decision it supports rather than by the person who writes it.

This structure is a project-specific convention. It is informed by ISO/IEC/IEEE 29148 requirements engineering, IIBA business analysis practices, domain-driven design, C4 architecture documentation, and Architecture Decision Records (ADR). No single source mandates this exact directory tree.

## Design principles

- Clarify relevant requirements before committing dependent solution choices. Provisional prototypes and feasibility experiments may help define those requirements.
- Every committed later artifact traces to relevant requirements and recorded decisions; exploratory artifacts link assumptions and open questions without claiming acceptance.
- Business language is separated from implementation language.
- Architecture decisions record context, alternatives, and consequences.
- Documentation changes with the product; it is not a one-time deliverable.

## Structure

```text
docs/
├── README.md
├── PRODUCT_DEVELOPMENT_PROCESS.md
├── DOCUMENTATION_ARCHITECTURE.md
├── PROJECT_CONTEXT.md
├── WORK_ITEMS.md
├── WORK_LOG.md
├── 01-product/
├── 02-requirements/
├── 03-analysis/
└── 04-solution-design/
```

## Root files

### `README.md`

The documentation map. It identifies canonical documentation and the current project phase.

### `PRODUCT_DEVELOPMENT_PROCESS.md`

The project lifecycle handbook. It defines phases, required artifacts, phase exit gates, and rules for moving forward.

The 11 activities map into the existing six phases: activities 1–2 to Phase 1; activity 3 to Phase 2; activity 4 to Phase 3; activities 5–7 to Phase 4; activity 8 to Phase 5; activities 9–11 to Phase 6. Readiness applies to a named journey/slice/release. Full-MVP coverage is tracked separately from individual slice readiness.

### `DOCUMENTATION_ARCHITECTURE.md`

Explains why the documentation is structured this way and what each directory owns.

### `PROJECT_CONTEXT.md`

The compact current snapshot: product boundary, working preferences, current phase/activity, verified facts, accepted-decision links, unresolved dependencies, and next action. Canonical policy remains in its source artifact. Refresh the snapshot after meaningful state changes; do not append session history here.

### `WORK_ITEMS.md`

The lightweight work and risk register. `WI-###` items record goal, scope, owner, dependencies, outputs, checks/evidence, readiness, and next action. `RISK-###` entries record impact, evidence, mitigation, owner, review trigger, and closure evidence. These IDs do not replace `UC-###` requirements or accept the catalog's Proposed MVP priorities.

Gate assessments identify scope, criteria, evidence, assessor/date, unresolved dependencies, and outcome. Work-item completion, product acceptance, and a gate passing are distinct.

### `WORK_LOG.md`

The append-only dated history of meaningful changes, decisions/provenance links, verification, remaining issues, and the next action. Use the owner's timezone. Read it when history matters rather than loading all past sessions at every startup.

### Repository instructions

`AGENTS.md` is the shared startup/work/handoff instruction source. `.cursor/rules/project-context.mdc` is a small always-applied pointer to it. The local delivery skill routes to the handbook and canonical sources; avoid keeping independent copies of the process in these entrypoints.

## `01-product/` — Product Discovery and Scope

Answers: **What product is being built, why, for whom, and within what boundary?**

- `PRODUCT_VISION.md`: product goal, learning goal, intended outcome, scope boundary, exclusions, and success criteria.
- `STAKEHOLDERS_AND_ACTORS.md`: system actors, stakeholder responsibilities, and actor relationships.
- `FUNCTIONAL_SCOPE.md`: high-level capabilities by actor; not detailed requirements.

## `02-requirements/` — Requirements Specification

Answers: **What must the system do and what rules must it satisfy?**

- `GLOSSARY.md`: canonical business vocabulary.
- `USE_CASE_CATALOG.md`: prioritized inventory of use cases.
- `USE_CASES/`: one detailed specification per use case, named `UC-###-short-name.md`.
- `BUSINESS_RULES.md`: cross-cutting business rules, named `BR-{DOMAIN}-###`.
- `STATE_MACHINES.md`: valid states, transitions, guards, and side effects for lifecycle entities.
- `NON_FUNCTIONAL_REQUIREMENTS.md`: measurable quality requirements, named `NFR-###`.

No framework, database, API, service boundary, or UI decision belongs here unless it is an externally imposed requirement.

UX research findings can inform requirements. Provisional solution sketches belong in the design area with their assumptions linked, rather than being presented as product requirements.

## `03-analysis/` — Domain and Process Analysis

Answers: **Which business concepts, processes, events, and consistency boundaries are implied by approved requirements?**

- `DOMAIN_MODEL.md`: domain concepts and relationships.
- `BUSINESS_PROCESS_DIAGRAMS.md`: end-to-end business workflows.
- `EVENT_CATALOG.md`: business events, producers, consumers, and payload meaning.
- Optional consistency/ownership notes: transactions, invariants, and data ownership analysis.

Draft domain sketches can clarify a selected journey alongside requirements; record unresolved rules and model only as deeply as the demo's complexity warrants. They do not constitute accepted domain boundaries.

## `04-solution-design/` — Solution Design

Answers: **How will the system meet approved requirements?**

- `C4_ARCHITECTURE.md`: context, containers, components, and deployment views.
- `DATA_MODEL.md`: persistence design derived from the domain model and use cases.
- `API_DESIGN.md`: API contracts derived from use cases.
- `SECURITY_DESIGN.md`: authentication, authorization, and protection controls.
- `INTEGRATION_AND_DEPLOYMENT.md`: external integrations, environments, and deployment design.
- `ADR/`: architecture decision records, named `ADR-###-short-name.md`.
- `UX/`: journey/flow/wireframe/prototype and interaction specifications when needed. Discovery-stage artifacts are explicitly provisional; a directory name does not imply Phase 4 acceptance.

## Planned delivery and verification areas

Create substantive files when the corresponding work exists; do not create empty documents to make the lifecycle look complete.

- `05-delivery/`: selected-slice delivery plan, test strategy, environment/CI/CD notes, and links to code/tests/build evidence. The initial central backlog remains `WORK_ITEMS.md`; detailed plans link their WI IDs rather than maintaining competing item states.
- `06-verification/`: integrated verification evidence, demo/UAT scenarios, release checklist/manifest, rollout and recovery evidence, runbooks, and iteration learning. Actual tests and environments determine the needed documents.

Commercial acquisition plans and production-scale operations are optional for this learning demo. Document demo participants/feedback and operating scope; change the product boundary explicitly before expanding it.

## Documentation conventions

- Aggregate documents use `UPPER_SNAKE_CASE.md`; directory indexes use `README.md`.
- Detailed use cases use `USE_CASES/UC-###-short-name.md`.
- Stable identifiers: `SC-###`, `UC-###`, `BR-{DOMAIN}-###`, `SM-{ENTITY}-###`, `NFR-###`, `ADR-###`, `OQ-###`, `AC-UC-###-##`, `WI-###`, and `RISK-###`.
- Keep the existing `BR-PROD-001`. Do not renumber identifiers; record a replacement if an identifier is retired.
- Use relative Markdown links to canonical artifacts. Avoid duplicating vocabulary or policy definitions.
- Documents identify status and owner. Use cases also identify scope source, proposed priority, related requirements, and acceptance criteria.
- Documentation remains in English to match the existing baseline; owner discussion may be in Vietnamese.
- Follow [the local templates](../.cursor/skills/marketplace-product-delivery/TEMPLATES.md).

### Status meanings

- **Baseline:** inherited from existing project documents; does not imply recorded owner sign-off.
- **Draft:** a specification being developed, with unresolved parts explicit.
- **Proposed:** a new policy, priority, or target offered for owner review.
- **Accepted:** a decision explicitly confirmed by the owner or made within recorded delegated decision authority, with applicable provenance recorded.

Mark individual entries when a document mixes baseline rules and proposed extensions. Document presence is not phase-gate approval. Planned folders have not started their phase; Planned describes scheduling, not product-decision acceptance.

Work-item states (Planned/In progress/Blocked/Done) and gate outcomes (Not assessed/Not passed/Passed) are separate from artifact/decision status. Provisional describes exploratory intent, usually with Draft status. It does not introduce another acceptance state.

### Phase 2 coordination

- `02-requirements/OPEN_DECISIONS.md`: the central decision-provenance and open-question register across product, requirements, and later design scopes. Its existing location and OQ IDs are retained. ADRs hold technical alternatives/rationale and link any affected OQ entry.
- `02-requirements/TRACEABILITY.md`: scope-to-use-case coverage, rules, states, NFRs, and acceptance criteria, including missing coverage.
- Acceptance criteria live in their detailed use case rather than a duplicated separate document.

Existing future phase folders contain an index until substantive artifacts are ready. Planned delivery/verification locations are defined above. Phase 2 does not require application scaffolding. Independent prototypes or experiments are permitted when useful, explicitly provisional, and within the owner's requested scope.

## Traceability

The intended trace is:

```text
Product Vision / Functional Scope
  → UC-### + BR-{DOMAIN}-### + SM-{ENTITY}-### + NFR-### + AC-UC-###-##
    → Domain Model / Process / Event
      → ADR / Architecture / Data Model / API
        → Backlog Item / Test / Implementation
```

When a decision changes, update every impacted artifact along this chain.

## Change and handoff maintenance

For a scope/rule/state/quality/design change, identify affected IDs, update the canonical definition and decision provenance first, then affected use cases/criteria, models/design/tests and traceability. Reopen dependent readiness assessments when their evidence is invalidated. Keep pending effects explicit rather than marking the whole chain complete.

Update work evidence and the compact context snapshot, then append a handoff for meaningful changes. Existing user instructions and Accepted decisions persist; process updates do not silently replace them. Links to planned artifacts should be inline-code paths with a Planned label until the files exist, not broken Markdown links.
