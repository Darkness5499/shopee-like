# Product Development Process

- Status: Baseline lifecycle revised on 2026-10-07; product decisions retain their recorded statuses.
- Owner: Project owner.
- Scope: The learning marketplace demo, from discovery through operation and the next iteration.

## Purpose and current position

This handbook explains the purpose, inputs, work, evidence, ownership, and readiness checks for eleven product-development activities. It also explains how work continues across sessions. It defines a project-specific workflow; it does not record product approval or claim conformance to a formal lifecycle standard.

As of this revision on 2026-10-07, the project is in **Phase 2 — Requirements Specification, in progress**. Product documents are the working baseline, formal Phase 1 sign-off is not recorded, and the Phase 2 exit gate has not passed. MVP priorities and unresolved policy/quality proposals remain unconfirmed. Application implementation has not started. Status statements in this handbook are dated snapshots; read [Project Context](PROJECT_CONTEXT.md) for the maintained current position and [the requirements index](02-requirements/README.md) for detailed progress.

The [Product Vision](01-product/PRODUCT_VISION.md) defines a learning demo with simulated payment and logistics. Discovery and validation therefore focus on coherent user journeys, credible business behavior, demonstrable software quality, and learning outcomes. Real-market demand research, commercial go-to-market planning, real money movement, and production-scale operations are not prerequisites for this scope. A future commercial expansion would require its own discovery and scope decisions.

## Eleven activities within the existing six phases

The eleven activities preserve the repository's six phase names and numbers:

- **Phase 1 — Product Discovery and Scope:** activity 1, Product Discovery; activity 2, Product Definition & MVP.
- **Phase 2 — Requirements Specification:** activity 3, Requirements Engineering.
- **Phase 3 — Domain and Process Analysis:** activity 4, Domain Design.
- **Phase 4 — Solution Design:** activity 5, UX/UI; activity 6, System Architecture; activity 7, Detailed Design / LLD.
- **Phase 5 — Delivery Planning and Implementation:** activity 8, Engineering & Implementation.
- **Phase 6 — Verification, Demo, and Learning:** activity 9, Quality / Security / Performance; activity 10, Release & Deployment; activity 11, Operate / Observe / Improve.

The numbering describes the kinds of work and their usual dependencies. It is not a requirement to complete every capability before starting another activity. Refine the scope broadly, then deepen the requirements and design for the selected journey or vertical slice. UX exploration, requirement clarification, domain modeling, and architectural feasibility work can inform one another.

Early journey maps, wireframes, prototypes, and feasibility spikes may run during discovery or requirements work. Label them **Provisional**, link their assumptions to open decisions, timebox the question they investigate, and record the result. Provisional describes exploratory work; it does not replace the document-status convention or establish accepted product policy. Committed implementation-facing designs must trace to sufficiently specified requirements for the slice they serve.

## 1. Product Discovery

**Purpose / outcome:** Understand the problem, intended users, constraints, and learning value well enough to choose what deserves further investigation. Produce a reasoned direction, not a predetermined feature list.

**Inputs:** The initial idea, existing product baseline, owner goals, actor knowledge, analogous marketplace flows, available time, and technical constraints.

**Activities:**

- Describe the problem and intended outcomes for Buyer, Seller, and Internal Staff, using the canonical actor definitions.
- Review comparable flows when useful; distinguish observations, owner statements, and assumptions.
- Identify user, business, and technical uncertainties. Investigate the uncertainty most likely to invalidate the intended journey.
- Consider lightweight interviews or scenario walkthroughs where participants are available. Record a lack of real-user evidence explicitly.
- Establish the demo boundary, constraints, and initial vision. Early sketches or spikes may explore questions without fixing a solution.

**Minimum deliverables / canonical locations:** Update [Product Vision](01-product/PRODUCT_VISION.md) and [Stakeholders and Actors](01-product/STAKEHOLDERS_AND_ACTORS.md). Record investigation tasks, evidence links, and delivery risks in [Work Items](WORK_ITEMS.md); product-policy questions belong in [Open Decisions](02-requirements/OPEN_DECISIONS.md).

**Completion criteria:** The target users, problem, learning outcome, demo exclusions, and key assumptions are explicit. The owner can decide whether to continue, narrow, or reconsider the idea. Any actual confirmation has provenance; the existence of these documents does not imply sign-off.

**Responsible roles:** Project owner leads direction; analyst and UX researcher/designer investigate needs; engineer assesses feasibility. One person may perform several roles.

**Feedback / reopen conditions:** A walkthrough reveals the wrong user problem, an essential flow is infeasible within the demo constraints, or later evidence changes the intended outcome. Reopen the affected discovery question rather than repeating unrelated research.

## 2. Product Definition & MVP

**Purpose / outcome:** Turn the direction into a bounded product goal and a proposed smallest useful end-to-end demonstration, with success measurements and explicit exclusions.

**Inputs:** Discovery findings, actor needs, baseline scope, constraints, open assumptions, and existing use case priorities.

**Activities:**

- Map the intended journey across actors and identify the capabilities it needs.
- Separate complete product scope from the proposed MVP and from the next delivery slice.
- Prioritize by learning value, user outcome, dependencies, implementation effort, and risk. A priority does not silently remove a baseline capability.
- Propose a release boundary, exclusions, and sequencing. For example, a publication-to-shopping journey can precede checkout and simulated fulfillment; this example does not approve the MVP.
- Define success measures with an owner, collection method, evaluation point, and a proposed target. Use demo outcomes and quality evidence rather than inventing commercial conversion targets.

**Minimum deliverables / canonical locations:** Keep goals and success criteria in [Product Vision](01-product/PRODUCT_VISION.md), capabilities and exclusions in [Functional Scope](01-product/FUNCTIONAL_SCOPE.md), and use case priorities in [Use Case Catalog](02-requirements/USE_CASE_CATALOG.md). Maintain the selected work, dependencies, and proposed metric-evaluation tasks in [Work Items](WORK_ITEMS.md). Record product-boundary and priority decisions in [Open Decisions](02-requirements/OPEN_DECISIONS.md).

**Completion criteria:** The proposed MVP describes a coherent journey, its exclusions and dependencies are visible, and success can be assessed. Before a proposed boundary or priority is marked Accepted, record owner confirmation or a decision within documented delegated product-decision authority. Draft requirements can progress while that decision is open.

**Responsible roles:** Project owner owns the goal, scope, and priorities; analyst maps journeys; engineer and UX designer estimate constraints and dependencies.

**Feedback / reopen conditions:** Requirement analysis exposes a missing prerequisite, the proposed MVP cannot demonstrate its outcome, or effort/quality evidence requires a smaller release. Update the boundary, priorities, work items, and affected specifications together.

## 3. Requirements Engineering

**Purpose / outcome:** Specify observable behavior and measurable quality requirements for the selected journey so actors, designers, and engineers share the same understanding.

**Inputs:** Vision, functional scope, actor definitions, proposed slice, discovery findings, owner decisions, and provisional journey/prototype findings.

**Activities:**

- Maintain the shared glossary and scope-wide use case catalog, then deepen the selected connected use cases.
- Specify triggers, permissions, preconditions, happy paths, alternatives, invalid actions, postconditions, and lifecycle changes.
- Link cross-cutting rules, state models, and NFRs rather than repeating their normative definitions.
- Write acceptance criteria with stable `AC-UC-###-##` identifiers. Include relevant failure, competing-action, and repeated-action behavior.
- Define security, privacy, performance, accessibility, reliability, and observability requirements in proportion to the demo and the current slice. Targets remain Proposed until confirmed.
- Surface policy gaps in Open Decisions; maintain coverage and missing dependencies in Traceability.

**Minimum deliverables / canonical locations:** [Glossary](02-requirements/GLOSSARY.md), [Use Case Catalog](02-requirements/USE_CASE_CATALOG.md), detailed specifications in `02-requirements/USE_CASES/`, [Business Rules](02-requirements/BUSINESS_RULES.md), [State Machines](02-requirements/STATE_MACHINES.md), [Non-Functional Requirements](02-requirements/NON_FUNCTIONAL_REQUIREMENTS.md), [Open Decisions](02-requirements/OPEN_DECISIONS.md), and [Traceability](02-requirements/TRACEABILITY.md). Acceptance criteria live in their owning use case.

**Completion criteria:** For the selected slice, its requirements are understandable and testable, important alternatives and state changes are specified, and blocking policy questions have recorded resolutions. Remaining gaps are explicit and do not invalidate the proposed downstream work. This slice check does not claim that Phase 2 or every MVP use case is complete.

**Responsible roles:** Analyst leads specifications; project owner resolves product policy; UX and engineering contributors challenge ambiguity; quality reviewer checks testability and risk coverage.

**Feedback / reopen conditions:** A prototype, domain model, implementation, or test reveals contradictory behavior, missing rules, or unmeasurable quality targets. Update the canonical requirement, its decision provenance, and all affected traceability links before treating the changed behavior as settled.

### Current requirements working order

Work follows connected business journeys across actors: Seller Product preparation/submission → Moderator review → Buyer discovery/detail/cart, followed by inventory/checkout/simulated payment, fulfillment/simulated delivery/tracking, and the minimal after-sales path. Account/Shop permissions, categories, validation, and audit needs are refined with the journey that depends on them.

The [requirements index](02-requirements/README.md) owns the current specification queue. This order neither approves proposed priorities nor requires completing one actor's entire feature set before working on another actor. The Phase 2 overview remains in progress while any properly bounded slice is explored or reviewed downstream.

## 4. Domain Design

**Purpose / outcome:** Explain the business concepts, ownership boundaries, valid lifecycles, invariants, and events implied by the selected requirements, independently of a framework or database.

**Inputs:** Relevant use cases, glossary, rules, state models, NFRs, resolved product decisions, and explicit remaining assumptions.

**Activities:**

- Identify domain concepts and relationships using the canonical vocabulary.
- Map business workflows across Buyer, Seller, Staff, and simulated partners.
- Define candidate bounded contexts and their responsibilities where they clarify ownership. A bounded context does not automatically require a separate deployed service.
- Identify entities, aggregate consistency boundaries where useful, invariants, and valid transitions. Trace each constraint to its business source.
- Describe domain events, their meaning, producers/consumers, and consistency needs without prematurely fixing transport or storage.

**Minimum deliverables / canonical locations:** Planned artifacts in `03-analysis/`: `DOMAIN_MODEL.md`, `BUSINESS_PROCESS_DIAGRAMS.md`, and `EVENT_CATALOG.md`. Keep consistency and ownership notes in the domain model unless a separate document would improve clarity. [The analysis index](03-analysis/README.md) tracks what actually exists. Extend [Traceability](02-requirements/TRACEABILITY.md).

**Completion criteria:** The selected journey can be explained through consistent concepts, ownership, state transitions, and invariants. Unresolved consistency risks are visible. Use the amount of domain modeling needed for the slice; a full DDD decomposition is not mandatory for this demo.

**Responsible roles:** Analyst/domain designer and engineer collaborate; project owner answers domain-policy questions; quality reviewer checks invalid-state and consistency cases.

**Feedback / reopen conditions:** UX exposes a missing business distinction, architecture reveals an ownership conflict, or concurrency tests show an invariant is underspecified. Reopen the relevant requirement or domain boundary, not the whole model.

## 5. UX/UI

**Purpose / outcome:** Make the intended journey usable and understandable, including permissions, business-state visibility, errors, and recovery.

**Inputs:** Actors, goals, selected use cases and acceptance criteria, lifecycle rules, domain vocabulary, quality requirements, and technical constraints.

**Activities:**

- Define information architecture and the user flow across the relevant actors.
- Sketch wireframes and prototype uncertain interactions. Exploratory work may begin in activities 1–3 with assumptions labelled Provisional.
- Walk through happy, empty, loading, denied, invalid, conflict, and recovery states as relevant to the slice.
- Check whether users can understand moderation status, purchase eligibility, payment/shipment simulation, and what action they can take next.
- Refine responsive UI behavior, accessible interaction, copy, and reusable components in parallel with architectural feasibility.

**Minimum deliverables / canonical locations:** Planned UX artifacts under `04-solution-design/UX/`: `USER_FLOWS.md`, `WIREFRAMES.md`, and `UI_SPECIFICATION.md`. Link prototypes or image assets from those documents and record walkthrough findings there. A short flow and annotated wireframe may be sufficient for a small slice; maintain [Traceability](02-requirements/TRACEABILITY.md).

**Completion criteria:** The selected flow can be walked through against its acceptance criteria; important error/recovery behavior is represented; material usability findings are resolved or tracked. Implementation-facing UI specifications distinguish settled behavior from provisional exploration.

**Responsible roles:** UX/UI designer leads; analyst checks rules; engineer checks feasibility; owner or available participant reviews the intended user outcome.

**Feedback / reopen conditions:** Users cannot complete a key task, UI states conflict with domain rules, or implementation exposes an unsupported interaction. Feed findings into requirements, domain design, and architecture as appropriate.

## 6. System Architecture

**Purpose / outcome:** Choose a proportionate system structure and technical approach that satisfies the slice's functional behavior, quality needs, and demo constraints.

**Inputs:** Relevant requirements/NFRs, domain boundaries, UX findings, constraints, risk investigations, and expected deployment needs.

**Activities:**

- Identify architecture drivers: authorization, consistency, asynchronous callbacks, maintainability, performance, and operational visibility where relevant.
- Describe context and container views; add component or deployment views only when they answer a concrete question.
- Assess data ownership, integration contracts, trust boundaries, dependency risks, and the simulated payment/logistics interfaces.
- Compare viable approaches and choose the simplest adequate structure. Microservices, event infrastructure, or a particular stack are decisions to justify, not default prerequisites.
- Record the chosen runtime, framework, persistence, and tooling stack with the constraints and trade-offs that justify it.
- Plan security controls, secret handling, deployment environments, telemetry, and release/recovery mechanisms early enough to influence design.
- Use timeboxed feasibility spikes for consequential uncertainty; record evidence separately from accepted architecture decisions.

**Minimum deliverables / canonical locations:** Planned `04-solution-design/C4_ARCHITECTURE.md`, `SECURITY_DESIGN.md`, and `INTEGRATION_AND_DEPLOYMENT.md`; technical decisions in [ADR](04-solution-design/ADR/README.md). A compact architecture description can cover the MVP without separate documents for every diagram. Link drivers and decisions in [Traceability](02-requirements/TRACEABILITY.md).

**Completion criteria:** Major technical choices for the slice have reasons, alternatives, consequences, and requirement links. Architecture-critical uncertainties are resolved or deliberately bounded by a spike. The deployment and integration plan respects the simulated-provider scope.

**Responsible roles:** Engineer/architect leads technical design; analyst and UX designer assess behavioral impact; security/operations reviewer checks relevant risks. Owner decisions retain explicit provenance.

**Feedback / reopen conditions:** A spike invalidates a choice, quality testing misses a target, a dependency changes, or the deployment model cannot support safe recovery. Update affected ADRs and downstream designs.

## 7. Detailed Design / LLD

**Purpose / outcome:** Make the selected slice implementable by specifying contracts, persistence, sequences, transactions, competing actions, and failure recovery.

**Inputs:** Slice-ready requirements and domain model, UX flow, architectural decisions, integration boundaries, and quality constraints.

**Activities:**

- Design API contracts, validation, permission checks, errors, and stable identifiers.
- Derive the persistence model from domain concepts; describe constraints, indexes, and schema evolution where needed.
- Describe important request/event sequences and transaction boundaries.
- Specify conflict detection, idempotency, callback ordering, retries, timeouts, and reconciliation where the slice needs them.
- Explain what happens when a dependency fails, a process stops mid-action, or a migration needs recovery. Use fault scenarios from the requirements instead of inventing new business policy.
- Keep UX, contracts, data design, tests, and requirement traceability consistent.

**Minimum deliverables / canonical locations:** Planned `04-solution-design/API_DESIGN.md`, `DATA_MODEL.md`, and `DETAILED_DESIGN.md`; add sequence and transaction notes to the latter. Integration/recovery details remain in `INTEGRATION_AND_DEPLOYMENT.md`; architecture trade-offs remain in ADRs. Do not duplicate the canonical business rules.

**Completion criteria:** The selected slice has sufficient contracts, persistence, state changes, concurrency semantics, and failure behavior to implement and test. Blocking questions have resolutions; optional details can be refined during delivery with recorded impact.

**Responsible roles:** Implementing engineer leads; architect/domain designer checks consistency; UX designer checks UI implications; quality reviewer evaluates failure and concurrency coverage.

**Feedback / reopen conditions:** Implementation reveals an incompatible contract, a race permits an invalid state, or a failure cannot be recovered as designed. Revisit the affected requirement/domain/architecture decision and update the contract before relying on it.

## 8. Engineering & Implementation

**Purpose / outcome:** Deliver a small working increment with traceable behavior, reviewed code, suitable automated checks, and reproducible execution.

**Inputs:** Selected work items, relevant acceptance criteria, sufficient design, dependencies, Definition of Done, and an iteration goal.

**Activities:**

- Break the journey into vertical slices with visible outcomes, dependencies, risks, and implementation tasks.
- Set up the application, local environments, seed/demo data, CI, and necessary tooling when implementation begins; current requirements work does not require scaffolding.
- Implement code and UI together with appropriate unit, integration, and journey checks. Add tests where they prove meaningful behavior or mitigate risk.
- Review code and verify authorization, validation, state invariants, callback handling, and logging as relevant to the slice.
- Keep requirements/design links current when implementation changes understanding. Expose blockers and remaining work promptly.
- Produce a reproducible build and an increment that satisfies its Definition of Done.

**Minimum deliverables / canonical locations:** Source code and tests in the application structure chosen during setup; backlog and delivery-risk entries in [Work Items](WORK_ITEMS.md). Planned `05-delivery/DELIVERY_PLAN.md`, `DEFINITION_OF_DONE.md`, and `CI_AND_ENVIRONMENTS.md` cover iteration sequencing, shared quality criteria, and build/environment instructions. Link commits, review results, and checks from work items and [Traceability](02-requirements/TRACEABILITY.md).

**Completion criteria:** The increment meets its acceptance criteria and shared Definition of Done, required checks pass, relevant review findings are addressed, and its evidence is linked. Work that is coded but unverified stays in progress. This is not an automatic release decision.

**Responsible roles:** Engineer implements and maintains CI; quality reviewer checks evidence; UX contributor verifies intended interaction; project owner orders work and reviews outcomes.

**Feedback / reopen conditions:** A dependency blocks delivery, checks reveal incorrect behavior, or actual effort changes the slice boundary. Replan the work and revisit affected specifications rather than silently cutting acceptance behavior.

## 9. Quality / Security / Performance

**Purpose / outcome:** Establish evidence that the selected release demonstrates the intended outcome and satisfies its relevant quality criteria, with known limitations visible.

**Inputs:** Candidate increment, acceptance criteria, NFRs, risk register entries, Definition of Done, release scope, and test/CI evidence already produced during delivery.

**Activities:**

- Run connected journey, integration, regression, and failure tests appropriate to the release.
- Verify role/Shop ownership boundaries, sensitive-data handling, input protection, dependency findings, and callback trust as applicable.
- Measure performance under a documented demo workload against agreed targets; verify high-risk competing or repeated actions.
- Conduct owner/participant acceptance walkthroughs against the release's criteria, including recovery behavior.
- Classify defects and limitations; assign mitigation, owner, and verification or reassessment tasks.
- Evaluate the release gate using existing evidence plus necessary release-wide checks. Activity 9 consolidates quality work that began earlier.

**Minimum deliverables / canonical locations:** Planned `06-verification/TEST_EVIDENCE.md`, `DEMO_SCENARIOS.md`, and `RELEASE_GATE.md`. Record defects and delivery risks in [Work Items](WORK_ITEMS.md); keep measurements tied to [NFRs](02-requirements/NON_FUNCTIONAL_REQUIREMENTS.md) and acceptance evidence tied to [Traceability](02-requirements/TRACEABILITY.md).

**Completion criteria:** Required evidence for the specified release exists, relevant AC/NFR checks pass, blocking defects are resolved, and remaining limitations have an accountable disposition. Record the release-gate outcome and scope; no gate has passed merely because the checklist exists.

**Responsible roles:** Quality reviewer leads verification; engineer investigates failures; security/performance reviewer handles applicable specialist checks; project owner evaluates demo acceptance and recorded product limitations.

**Feedback / reopen conditions:** A failed test or walkthrough exposes a requirement ambiguity, inadequate design, unsafe access, or a missed target. Return the finding to its owning artifact and work item; repeat only the checks affected by the fix and any necessary regression coverage.

## 10. Release & Deployment

**Purpose / outcome:** Put the verified demo release into its intended environment reproducibly, confirm it works there, and retain a usable recovery path.

**Inputs:** Release-gate evidence, versioned build, environment configuration, deployment design, migration plan, demo data, monitoring checks, and rollback/recovery instructions.

**Activities:**

- Deploy to a test/staging environment and verify configuration, migrations, seed data, and simulated integrations.
- Use a limited beta or review environment only if it helps evaluate the release. For this project, the final target may be a hosted demo rather than a commercial production environment.
- Record the version, included scope, known limitations, and deployment outcome.
- Run smoke checks and confirm telemetry after deployment.
- Verify rollback or forward-recovery steps, including data/schema compatibility. An application rollback does not automatically reverse a database migration.
- Restore service or data using the documented plan if deployment checks fail; record what happened.

**Minimum deliverables / canonical locations:** Planned `05-delivery/RELEASE_PLAN.md` owns versions, environment sequence, configuration, migration and rollback procedures. Planned `06-verification/RELEASE_RECORDS.md` records execution and smoke/recovery evidence. Long-lived environment and integration decisions remain in `04-solution-design/INTEGRATION_AND_DEPLOYMENT.md`; deployment scripts and migrations live with the application.

**Completion criteria:** The intended release is running in its declared environment, smoke checks pass, observers can identify the running version and critical failures, and recovery instructions are practical. Release evidence identifies its actual environment and scope without claiming production readiness.

**Responsible roles:** Release/operations contributor executes deployment; engineer owns build and migration behavior; quality reviewer checks smoke evidence; project owner confirms the demo outcome.

**Feedback / reopen conditions:** Configuration drift, migration failure, missing telemetry, or a deployment-specific defect invalidates the release. Recover first, then update the release/design/work-item evidence and reassess the affected gate.

## 11. Operate / Observe / Improve

**Purpose / outcome:** Learn from the running demo, diagnose failures, evaluate the product and quality outcomes, and choose the next improvement.

**Inputs:** Running release, metric plan, logs/traces, demo feedback, incidents, known limitations, and previous work items.

**Activities:**

- Observe journey completion, relevant error counts, callback processing, and response times according to the agreed measurement plan.
- Use correlated logs and traces where they help explain a failure; keep secrets and sensitive values out of telemetry.
- Compare observed results with success criteria and NFR targets. Document workload, sample size, and missing evidence so results are not overstated.
- Gather owner/participant feedback, troubleshoot incidents, and record root causes and corrective work.
- Review what improved learning or delivery effectiveness, then prioritize the next experiment or slice.
- Feed new evidence into discovery, scope, requirements, domain/design decisions, or engineering practice as appropriate.

**Minimum deliverables / canonical locations:** Planned `06-verification/OPERATIONS_RUNBOOK.md` and `LEARNING_REVIEW.md` hold troubleshooting, monitoring, incident, metric-result, and retrospective notes. Track actionable follow-up in [Work Items](WORK_ITEMS.md), dated handoffs in [Work Log](WORK_LOG.md), and the current position in [Project Context](PROJECT_CONTEXT.md). Update canonical requirements or ADRs when lessons change a decision.

**Completion criteria:** The released slice has been observed for a declared period or demo session, key findings and limitations are recorded, incidents have follow-up ownership, and the next action is explicit. Operation continues; this check closes an evaluation cycle rather than declaring the product permanently finished.

**Responsible roles:** Operations/engineering contributor investigates behavior; project owner assesses outcomes and priorities; analyst/UX contributor interprets journey feedback; all contributors improve the process.

**Feedback / reopen conditions:** Observed failures, unmet outcomes, confusing interactions, or changed constraints reopen the relevant earlier activity. Discovery may begin again while the existing demo continues to run.

## Readiness checks and phase gates

Completion criteria apply to the **named slice, release, or evaluation cycle**. Each readiness record identifies its scope, evidence, unresolved risks, accountable reviewer, outcome, and date. A blocked dependency blocks the work that relies on it, not every independent activity.

Keep execution status separate from document/decision status: work uses **Planned / In progress / Blocked / Done**, and readiness checks use **Not assessed / Not passed / Passed**. A document marked Baseline, Draft, Proposed, or Accepted does not establish execution or gate status. At this revision, the Phase 2 gate is **Not passed**.

- **Phase 1:** The direction and proposed boundary are coherent; confirmed goals, scope, exclusions, and success criteria have decision provenance. Baseline documents alone do not pass this gate.
- **Phase 2:** The selected scope has testable use cases, rules, states, NFRs, and acceptance criteria, with blocking product decisions resolved. Marking a complete MVP requirements package ready also requires the MVP boundary and its included coverage to be confirmed; one ready slice does not establish that larger result.
- **Phase 3:** Domain concepts, workflows, ownership, events, and invariants are consistent for the scope entering solution design.
- **Phase 4:** Relevant UX, architecture, contracts, persistence, concurrency, and failure behavior are sufficient for the slice to enter implementation; important design risks have evidence or a bounded investigation plan.
- **Phase 5:** The increment satisfies its Definition of Done and has traceable implementation/review/check evidence.
- **Phase 6:** The specified demo release has verification, deployment/recovery, observation, and learning evidence. A release gate can precede operation; completing Phase 6 also needs the release's evaluation and next-action record.

The global phase in Project Context describes the primary work in progress. Record narrower readiness results alongside their work items; do not advance the global phase or mark the whole MVP accepted from a partial slice check. At this revision, the global position remains Phase 2 in progress; maintain subsequent progress in Project Context and the relevant canonical work/requirements evidence.

### Acceptance criteria, Definition of Done, and release gate

- **Acceptance criteria** describe the observable behavior of a particular use case. Their canonical definition stays in `02-requirements/USE_CASES/`; a draft criterion is not an executed or passed test.
- **Definition of Done** describes shared quality conditions for an implemented increment. Plan it before the first slice is delivered. The initial project-specific version should cover relevant AC evidence, code review, required CI checks, authorization/invariant checks, current documentation, reproducible execution, and telemetry needed to investigate the slice.
- **Release gate** decides whether a particular candidate is ready for a particular environment and audience. It adds release-wide journey verification, relevant NFR/security evidence, configuration/migration readiness, smoke checks, and recovery preparation to completed increment evidence.

A feature can meet its acceptance criteria while failing the Definition of Done. An increment can meet the Definition of Done while the release still lacks a working migration or complete demo journey. Keep these outcomes separate and record evidence rather than inferring readiness.

## Work that spans every activity

**Security and privacy:** Identify assets, actors, ownership permissions, and data sensitivity during requirements work; identify trust boundaries and controls during design; implement and verify them during each affected slice. Use simulated partners and suitable demo data. Track actual findings rather than imposing an unrelated compliance program. Integrating security throughout development follows the direction of [NIST SSDF, SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final).

**Performance, reliability, and observability:** Establish measurable expectations before architectural choices depend on them. For each selected measure, identify its requirement or outcome, target status, collection method, test workload, reviewer, and review point. Design correlation identifiers, useful events/metrics, error visibility, and redaction with the slice. Measure during verification and compare after deployment; this demo does not need production-scale traffic or an enterprise monitoring stack.

**Backlog, dependencies, and risk:** [Work Items](WORK_ITEMS.md) is the persistent project backlog, including investigation, documentation, implementation, defect, release, and improvement work. Entries should identify the outcome, status, responsible role, next action, dependencies, canonical requirement/decision links, and evidence. Delivery risks identify impact, mitigation, owner, reassessment trigger, and linked work. A product-policy question remains an OQ entry; the backlog links to it rather than making a second decision register.

**Proportionate documentation:** Produce the minimum artifact that enables the next decision or verification. Expand it when risk, ambiguity, or repeated coordination warrants the detail. Planned paths listed here are destinations, not existing deliverables or evidence of completed phases. `05-delivery/` and `06-verification/` are planned and are created when substantive delivery or verification work begins.

## Ownership and decision changes

Project owner, analyst, UX designer, engineer/architect, quality reviewer, and release/operations contributor are responsibilities, not mandatory staffing positions. One person may hold several roles; a work item names its accountable role/person. An assistant may investigate, draft, implement, and report within the authorized task, but must not fabricate owner confirmation or test results.

Apply the status and identifier rules in [Documentation Architecture](DOCUMENTATION_ARCHITECTURE.md). Baseline, Draft, Proposed, and Accepted have distinct meanings. Accepted product choices need explicit owner confirmation or a decision within recorded delegated authority, with applicable provenance. Routine authorized analysis and preparation can continue without changing a proposal to Accepted.

Keep decisions in the artifact that owns their meaning:

- **Phases 1–2, product choices:** Goals and boundary live in product documents; rule/state/NFR definitions live in requirements. [Open Decisions](02-requirements/OPEN_DECISIONS.md) remains the canonical register for product-policy questions, including those discovered later.
- **Phase 3, domain analysis:** Model conclusions live in domain/process/event artifacts and link their requirement sources. A conclusion that introduces new product behavior returns to Open Decisions and the owning requirement.
- **Phase 4, solution choices:** Architectural trade-offs live in `04-solution-design/ADR/`; UX, API, data, and detailed design live in their solution artifacts. Do not disguise a business-policy change as an implementation choice.
- **Phases 5–6, delivery and operational choices:** Iteration/release sequencing lives in delivery plans and Work Items; verification, release outcomes, incidents, and lessons live in verification records. Changes to an architectural choice update or supersede its ADR; changes to product behavior follow the product-decision path.

When scope or a decision changes:

1. Identify the canonical choice, source, affected slice/release, and whether the change is a proposal or confirmed decision.
2. Assess affected scope, use cases, rules, states, NFRs, domain/design, code, tests, release procedures, and already deployed behavior. Scope changes do not silently erase previous identifiers.
3. Update the owning artifact and register, then [Traceability](02-requirements/TRACEABILITY.md). Show missing downstream work as pending.
4. Add the necessary update/reverification work and dependencies to Work Items; supersede obsolete decisions explicitly.
5. Update Project Context and append a dated Work Log entry. Reopen only the readiness checks invalidated by the change.

## Iteration and session continuity

Use this cycle for each connected journey or vertical slice:

1. **Choose an outcome.** Read current context, select the next work item, identify relevant requirements and open decisions, and state the scope of this iteration.
2. **Reduce uncertainty.** Clarify behavior, explore UX/domain/design as needed, and run a provisional spike if it answers an important question. Record evidence and assumptions.
3. **Check the affected prerequisites.** Resolve blocking dependencies for the selected implementation; unrelated scope can remain draft.
4. **Deliver and verify.** Implement an increment when appropriate, apply acceptance criteria and Definition of Done, then evaluate a release gate when preparing deployment.
5. **Review and learn.** Inspect findings with the owner, compare metrics when evidence exists, update decisions and backlog, and choose the next action. A fixed sprint cadence is optional for this learning project.
6. **Persist the handoff.** Update the current snapshot and dated log before stopping or switching context.

At the beginning of a session, read [Project Context](PROJECT_CONTEXT.md), the latest relevant entries in [Work Log](WORK_LOG.md), [Work Items](WORK_ITEMS.md), and the applicable phase index. Follow their links to canonical decisions and specifications rather than relying on chat history alone.

At the end of substantive work, update these distinct records:

- **Project Context:** Current phase/activity, active journey, established constraints, accepted/proposed decision references, readiness status, blockers, and next action. It is a navigational snapshot, not a second business-rule document.
- **Work Log:** A dated account of what changed, the affected files/work items, checks actually run and their results, remaining questions, and the next handoff. Use the project timezone, Asia/Ho_Chi_Minh, when recording dates/times.
- **Work Items:** Persistent tasks and delivery risks with current status, dependencies, ownership, and evidence links. Keep its ordering aligned with the current outcome.
- **Open Decisions and Traceability:** Canonical product questions/confirmation provenance and requirement-to-design/test/implementation coverage. Keep these existing registers authoritative.

If a summary conflicts with a canonical specification or confirmation record, inspect the provenance, correct the stale summary, and make the discrepancy visible. Never invent missing completion or approval evidence to make a session look consistent.

## Sources and limits

These sources inform specific practices; the eleven activities, six-phase mapping, artifact paths, and local gates are this project's choices:

- [Nielsen Norman Group — Discovery: Definition](https://www.nngroup.com/articles/discovery-phase/): discovery investigates the problem, users, constraints, and desired outcomes. The lightweight demo adaptation and early provisional experiments are project choices.
- [The 2020 Scrum Guide](https://scrumguides.org/scrum-guide.html): iterative increments, an evolving backlog, inspection/adaptation, and a shared Definition of Done inform the delivery cycle. This handbook does not require or claim full Scrum implementation.
- [NIST SP 800-218 — Secure Software Development Framework, Version 1.1](https://csrc.nist.gov/pubs/sp/800/218/final): secure-development practices can be integrated into a lifecycle. This project uses that principle without claiming SSDF compliance or certification.

The existing requirements and documentation conventions also draw on requirements engineering, domain analysis, C4, and ADR practices as described in [Documentation Architecture](DOCUMENTATION_ARCHITECTURE.md). No cited source mandates this repository's exact workflow or directory tree.
