# Project working instructions

Applies to the entire Shopee-like repository. User instructions take precedence.

## Start a session

1. Read `docs/PROJECT_CONTEXT.md` for the current state and next action.
2. Read `docs/WORK_ITEMS.md` for the relevant work item and dependencies.
3. Read `docs/PRODUCT_DEVELOPMENT_PROCESS.md` when choosing activities or evaluating readiness.
4. Read the canonical artifacts linked from those documents that the task affects; use `docs/WORK_LOG.md` when handoff history matters.
5. Inspect the working tree before edits. Preserve existing user changes.

## Work consistently

- Use the project skill at `.cursor/skills/marketplace-product-delivery/SKILL.md` for product, requirements, domain, design, and delivery work.
- Discuss work with the owner in Vietnamese; maintain repository documentation in English, following the existing baseline.
- Keep scope, rules, states, acceptance criteria, design, and tests linked through `docs/02-requirements/TRACEABILITY.md`; preserve stable identifiers.
- Keep product decisions canonical in `docs/02-requirements/OPEN_DECISIONS.md`; use ADRs for technical rationale. Link these rather than duplicating policy text.
- Distinguish Baseline, Draft, Proposed, and Accepted. Acceptance needs recorded owner confirmation or explicitly delegated decision authority; file existence is not approval.
- Apply readiness to the selected journey/slice. Provisional UX, domain sketches, and feasibility spikes may support discovery before all requirements are accepted; record assumptions and affected decisions.
- Proceed with authorized drafting, reviews, reversible fixes, and routine technical choices. Resolve material product ambiguity before treating a proposed behavior as accepted; continue independent work while a decision is pending.
- Keep this a learning demo with simulated payment/logistics unless the owner changes the product boundary.

## Finish a meaningful work session

- Update changed canonical artifacts and traceability together.
- Update the affected work item with actual results, checks, unresolved issues, and its next action.
- Refresh `docs/PROJECT_CONTEXT.md` when the current state, priorities, decisions, or next action changed.
- Append a dated handoff to `docs/WORK_LOG.md` for meaningful project changes, using the owner's timezone (Asia/Ho_Chi_Minh).
- Record gate evidence only when assessed; do not equate drafted acceptance criteria with passing tests or completed documentation with a passed product/release gate.

## Learner-led delivery

- The owner is learning system design and solution architecture, not requesting unattended delivery of every phase. Follow `docs/01-product/LEARNING_PLAN.md`.
- Begin the next learning exercise with the owner's model, reasoning or code attempt. Coach, ask focused questions, review counterexamples and explain trade-offs. Do not preemptively complete the next architecture or implementation unless the owner explicitly asks.
- Routine authorized documentation maintenance, reviews, fixes and Git handoffs may proceed autonomously. Distinguish assistant-written Draft material from learner-reviewed evidence.
- Keep only Part 1 publication/moderation active; deferred features are not automatic dependencies to implement. New scope needs an owner request.
