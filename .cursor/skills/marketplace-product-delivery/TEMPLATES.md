# Templates

## Document metadata

Identify Status and Owner beneath each canonical document title. Use Scope and Source for specifications and Priority where prioritization applies; shared fields may be inherited by entries. Link to canonical artifacts and identify entry-level status when a document mixes baseline facts and proposals.

```markdown
- Status: [Baseline | Draft | Proposed | Accepted]
- Owner: [accountable role or person]
- Scope: [SC-### links, entity, or document boundary]
- Source: [existing artifact / explicit owner confirmation or applicable recorded delegation and date]
- Priority: [not assigned | Proposed priority | confirmed priority and source]
```

Baseline preserves existing content without claiming recorded sign-off. Draft means specification in progress. Proposed marks a new policy, priority, or target. Accepted requires explicit project-owner confirmation or applicable recorded delegation of decision authority; general authorization to draft does not imply acceptance. Local Open questions sections link to the central `docs/02-requirements/OPEN_DECISIONS.md` entry instead of duplicating the unresolved decision. These statuses describe artifacts or decisions; task progress and readiness outcomes use their own fields.

## Product scope item

```markdown
## SC-### — [Capability]

- Status: [Baseline | Proposed | Accepted]
- Actor:
- Capability and boundary:
- Source:
- Related use cases: [UC-### links or pending]
```

## Use Case Catalog entry

```markdown
- [UC-### — Name](USE_CASES/UC-###-short-name.md)
  - Actor:
  - Scope: [SC-### link]
  - Status: [Draft | Proposed | Accepted]
  - Priority: [not assigned | Proposed MVP/later | confirmed priority and source]
  - Rules/states/NFRs: [canonical links or pending]
  - Open questions: [OQ-### links in OPEN_DECISIONS.md or none]
```

## Use Case

```markdown
# UC-### — [Name]

- Status: Draft
- Owner:
- Scope: [SC-### links]
- Source: [baseline artifact / explicit owner decision]
- Priority: [not assigned | Proposed priority | confirmed priority and source]

## Goal
## Primary actor
## Supporting actors
## Trigger
## Preconditions
## Main flow
## Alternative and error flows
## Business rules
[BR-{DOMAIN}-### links to BUSINESS_RULES.md]
## State/data changes
[SM-{ENTITY}-### links to STATE_MACHINES.md]
## Postconditions
## Acceptance criteria
- AC-UC-###-01: Given [context], when [action], then [observable result].
  - Related rules/states/NFRs: [canonical links]
  - Verification: [scenario or test reference; pending before implementation]
## Traceability
[SC-### → UC-### → BR/SM/NFR → AC links in TRACEABILITY.md]
## Open questions
[OQ-### links in OPEN_DECISIONS.md or none]
```

## Business Rule

```markdown
## BR-{DOMAIN}-### — [Name]

- Status: [Baseline | Draft | Proposed | Accepted]
- Owner:
- Scope: [SC-### / business entity]
- Source:
- Priority: [not assigned | inherited UC priority]

### Rule
[Observable business constraint; the canonical rule definition lives here.]
### Rationale
### Applies to
### Exceptions
### Related use cases and states
[UC-### and SM-{ENTITY}-### links]
### Acceptance criteria
[AC-UC-###-## links to the owning use cases]
### Open questions
[OQ-### links in OPEN_DECISIONS.md or none]
```

Keep `BR-PROD-001` for the existing product-moderation rule. Add further rules with stable domain codes; do not rename existing IDs when moving sections or refining their content.

## State Machine

```markdown
# SM-{ENTITY}-### — [Entity] State Model

- Status: [Baseline | Draft | Proposed | Accepted]
- Owner:
- Scope: [SC-### / entity]
- Source: [business rule / use case / owner confirmation]
- Priority: [not assigned | inherited UC priority]

## States
## Allowed transitions
## Transition guards
## Transition side effects/events
## Invalid transitions
## Related use cases and acceptance criteria
[UC-###, BR-{DOMAIN}-### and AC-UC-###-## links]
## Open questions
[OQ-### links in OPEN_DECISIONS.md or none]
```

Use `SM-PRODUCT-001` for the product lifecycle. Label new state or transition policy Proposed until confirmed. Order, payment, shipment, and review states belong to their respective lifecycle definitions.

## Non-Functional Requirement

```markdown
## NFR-### — [Name]

- Status: [Draft | Proposed | Accepted]
- Owner:
- Scope: [SC-### / UC-### / quality boundary]
- Source:
- Priority:
- Requirement:
- Measurement/acceptance threshold:
- Rationale:
- Verification method:
- Related use cases/acceptance criteria:
- Open questions: [OQ-### links in OPEN_DECISIONS.md or none]
```

## Architecture Decision Record

```markdown
# ADR-### — [Decision]

- Status: [Draft | Proposed | Accepted]
- Owner:
- Scope:
- Source: [requirement links / explicit owner confirmation or applicable recorded delegation]
- Priority:

## Status
[Decision status and confirmation/delegation provenance; record later supersession here.]
## Context
## Decision
## Alternatives considered
## Consequences
## Links to requirements
## Open questions
[OQ-### links in OPEN_DECISIONS.md or none]
```

## Open Decision

Create the canonical entry in `docs/02-requirements/OPEN_DECISIONS.md`.

```markdown
## OQ-### — [Decision needed]

- Status: [Draft | Proposed | Accepted]
- Decision state: [Open | Resolved]
- Owner:
- Related artifacts: [SC / UC / BR / SM / NFR / domain / design / ADR links]
- Question:
- Proposal: [explicitly Proposed; may be absent]
- Impact if unresolved:
- Resolution: [pending or accepted choice with explicit confirmation/delegation source/date]
```

## Traceability entry

```markdown
- SC-### → UC-### → BR-{DOMAIN}-###, SM-{ENTITY}-###, NFR-###
  → AC-UC-###-## → [domain/design/ADR links or pending] → [test reference or pending]
- Coverage: [complete | draft/incomplete]
- Open decisions: [OQ-### links or none]
```

Store canonical traceability entries in `docs/02-requirements/TRACEABILITY.md`. Links are relative to the document containing them. Do not invent downstream evidence to fill a traceability entry. Provisional design or spike findings may be linked with their status and open dependencies visible.

## Work item

Maintain actionable work in `docs/WORK_ITEMS.md`. Work states describe execution only; Done does not mean every referenced proposal is Accepted. Use stable `WI-###` identifiers and preserve completed entries.

```markdown
## WI-### — [Action and result]

- Work state: [Planned | In progress | Blocked | Done]
- Owner: [accountable role/person; unassigned if unknown]
- Phase and selected slice: [bounded journey or deliverable]
- Goal and completion criteria: [observable outcome]
- References: [canonical SC / UC / BR / SM / NFR / OQ / ADR links]
- Evidence: [changed artifacts, check result and date, or pending]
- Next action: [concrete action; none when complete]
- Blockers/dependencies: [OQ / WI / RISK links or none]
```

## Readiness or gate record

Place a record with the relevant work item or phase/release artifact and link it from context. Use the readiness criteria in `docs/PRODUCT_DEVELOPMENT_PROCESS.md`; record actual outcomes rather than declaring every check passed. A narrowly scoped pass does not pass a wider phase or release.

```markdown
## Readiness — [Phase / slice / release]

- Scope: [named capabilities, use cases, artifact versions or commit]
- Result: [Not assessed | Not passed | Passed]
- Accountable owner: [role/person; unassigned if unknown]
- Check date: [date; pending before evaluation]
- Criteria and actual evidence:
  - [criterion]: [result and artifact/test/reference; pending if unverified]
- Open dependencies and risks: [OQ / WI / RISK links and impact]
- Decision provenance: [explicit confirmation or applicable delegation source/date where required; pending otherwise]
- Next action or recheck trigger: [action or changed assumption]
```

## Handoff

Append concise dated entries to `docs/WORK_LOG.md` after actual changes. Keep the current snapshot in `docs/PROJECT_CONTEXT.md` and execution state in `docs/WORK_ITEMS.md`; link to canonical facts rather than restating policy.

```markdown
## [YYYY-MM-DD] — [Completed work or checkpoint]

- Work items and scope: [WI-### links; current phase and selected slice]
- Changes and evidence: [canonical artifact links; what changed and why]
- Verification: [checks and outcomes; explicitly identify checks not run]
- Decisions/dependencies: [accepted decision provenance or pending OQ links]
- Resume at: [next concrete action and required artifact]
- Context/work items updated: [links; remaining blocker if any]
```

## Risk

Keep actionable risks in a section of `docs/WORK_ITEMS.md` using stable `RISK-###` identifiers. A risk records uncertainty and its handling; product choices still belong in `OPEN_DECISIONS.md`. Use qualitative estimates unless measured evidence supports numbers.

```markdown
## RISK-### — [Uncertain event and impact]

- Risk state: [Open | Mitigating | Closed]
- Owner: [role/person; unassigned if unknown]
- Affected scope and references: [WI / SC / UC / NFR / OQ / ADR links]
- Cause or evidence: [observed fact or clearly labelled assumption]
- Likelihood and impact: [qualitative estimate with rationale]
- Response and trigger: [action and condition requiring it]
- Verification/closure evidence: [reference or pending]
```
