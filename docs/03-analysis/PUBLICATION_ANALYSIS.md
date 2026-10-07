# Part 1 — Publication domain and concurrency analysis

- Status: Draft, provisional Phase 3 analysis; no implementation evidence.
- Owner: Project owner as learner/architect; assistant prepared this Draft for critique.
- Scope: UC-004/005/006; minimal UC-008/009 and audit UC-024 per [learning plan](../01-product/LEARNING_PLAN.md).
- Sources: [BR-PROD-002/003](../02-requirements/BUSINESS_RULES.md#br-prod-002), [Product lifecycle](../02-requirements/STATE_MACHINES.md#sm-product-001), and OQ-002/005/006 in [decisions](../02-requirements/OPEN_DECISIONS.md).

## Domain ownership

Product owns current editable content, its revision, review-related visibility and SKU references. Submission captures one immutable validated content revision, its identity and pending/final outcome. A Moderator decides against that exact submission. User/Shop/Category are seeded references for this part; their administration is deferred. AuditEntry explains one effective command; transport retries are not additional business changes.

## Invariants

- A successful actual risky save hides Product immediately. Failed saves and opening an editor do not trigger hiding.
- Pending submitted content is immutable. Withdrawal must succeed before further risky editing of that submission; new submission receives new identity and validation.
- For one pending submission, the first accepted withdrawal/approval/rejection wins. Opposite later actions are refused; exact repeats return the existing result without extra audit or publication effects.
- Approval publishes only the reviewed revision subject to current guards. Rejection keeps Product hidden; old approved content is not restored automatically.
- Operational SKU changes cannot bypass review hiding. Read behavior must not expose unapproved content.

## Process and consistency boundaries

| Command | Required consistent outcome | Failure/conflict behavior |
|---|---|---|
| Save risky change | New content revision, hidden Product and attributable audit | Refuse invalid/stale input without partial change |
| Submit | Validated immutable revision and unique pending submission | Repeat returns same pending submission; invalid content stays outside review |
| Withdraw | Pending submission becomes withdrawn, Product remains hidden, audit once | Already decided submission refuses a new withdrawal |
| Approve/reject | Submission outcome, Product visibility/content effect and audit agree | Compare target identity/current state; competing loser cannot overwrite winner |
| Read listing/detail | Evaluate current approved content and eligibility | Hidden Product unavailable; no pending content leakage |

These are business consistency requirements. Database locks, optimistic versions and outbox design are Phase 4 choices, not selected technologies here.

## Design questions and next output

The next action is the owner's model and race walkthrough from the Learning Plan. This document is a starting reference, not learner completion evidence. Review that attempt before preparing a full Phase 4 solution. Canonical OQ-002/005/006 records have Accepted policy provenance; implementation details and state-code choices not covered by those resolutions remain explicit assumptions.

Phase 4 should explain how conditional state changes prevent two winners, how operation identity detects retries with changed instructions, how committed audit survives interruption, and how public reads observe hiding without stale-cache leakage. Compare a single transactional application/database first; add distributed components only when a concrete requirement justifies them. Validation, seeded permission and SKU assumptions link to OQ-002/005/006 and remain explicit where canonical policy is incomplete.

Verification planning: simultaneous approve/withdraw; repeated approve after lost response; successful risky save followed by immediate public read; rejection followed by operational edit; stale submission decision after resubmission. These scenarios extend the existing AC IDs, not claim passed tests.
