# UC-024 — Record and inspect auditable business actions

- Implementation status: Not implemented — publication/moderation audit only; full audit capability deferred.
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; the audit inventory, viewer permissions, redaction and retention are Proposed under [OQ-012](../OPEN_DECISIONS.md#oq-012) and [OQ-005](../OPEN_DECISIONS.md#oq-005).
- Owner: Project owner.
- Priority: P0 — Proposed cross-cutting requirement in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-022, supports SC-023; quality basis [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004).

## Goal

Preserve a trustworthy, attributable record of every auditable business action and let authorized staff inspect it to explain business changes and diagnose simulated asynchronous flows. This is a cross-cutting capability; each detailed use case supplies its own audit criterion.

## Primary actor

Initiating actors (Buyer, Seller / Shop Operator, Internal Staff, simulated partners) produce records through their use cases. Inspection actors are authorized Internal Staff and, for their own Shop, Shop owners.

## Trigger

An auditable action is attempted or completed, or an authorized actor queries the audit history.

## Preconditions

- The auditable action inventory is defined in [BR-AUDIT-001](../BUSINESS_RULES.md#br-audit-001).
- Inspection requires the audit-view permission with an applicable scope.

## Main flow

1. An auditable action takes effect through its owning use case.
2. The system records, in the same atomic outcome as the business change: actor identity (or partner/system identity), action, affected resource and changed data (before/after where applicable), a correlation identifier and an authoritative timestamp.
3. An authorized actor searches the history by resource, actor, action type, correlation identifier and time range, within their permitted scope.
4. The system returns matching records with sensitive data redacted per policy and does not allow any record to be changed or deleted.

## Alternative and error flows

- **A1 — Business change cannot be recorded:** if the audit record cannot be established together with the change, the change is not applied.
- **A2 — Denied or failed attempts:** denied authorization and rejected requests in the inventory produce a distinguishable record that does not claim a business change occurred.
- **A3 — Partner evidence:** duplicate, late and conflicting partner events are recorded as delivery evidence that is distinguishable from a repeated effective business change.
- **A4 — Sensitive data:** credentials, verification codes, callback secrets and full payment/address details are excluded or redacted from audit content.
- **A5 — Unauthorized inspection:** an actor without the view permission or outside their scope receives no records, and a Shop owner cannot see another Shop's records.
- **A6 — Tampering attempt:** requests to edit or delete an audit record are refused and are themselves recorded.
- **A7 — Repeated request:** an exact repeat that produces no new business effect produces no duplicate audit effect.

## Business rules

[BR-AUDIT-001](../BUSINESS_RULES.md#br-audit-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004).

## State/data changes

Appends immutable AuditRecord entries. Records are never updated or deleted by business actions.

## Postconditions

- Success: every inventory action has exactly one attributable record per effective change and records can be found and correlated.
- Failure: no unrecorded effective change and no record exposure to unauthorized actors.

## Acceptance criteria

- **AC-UC-024-01 — Required elements (Proposed):** each audit record contains actor/system identity, action, affected resource, changed data, correlation identifier and timestamp.
- **AC-UC-024-02 — Inventory coverage (Proposed):** for every action in the [BR-AUDIT-001](../BUSINESS_RULES.md#br-audit-001) inventory, performing it once yields exactly one attributable record for the effective change.
- **AC-UC-024-03 — Atomic with change (Proposed):** a business change and its audit record take effect together; if the record cannot be established, the change is not applied.
- **AC-UC-024-04 — Denied and failed attempts (Proposed):** denied or rejected attempts listed in the inventory produce a record marked as an unsuccessful attempt without implying a business change.
- **AC-UC-024-05 — Partner evidence (Proposed):** repeated, late or conflicting partner events are recorded as received evidence and are distinguishable from a repeated effective business change.
- **AC-UC-024-06 — Immutability (Proposed):** an audit record cannot be modified or deleted through any business action; an attempt is refused and recorded.
- **AC-UC-024-07 — Search and correlation (Proposed):** an authorized actor can find records by resource, actor, action type, correlation identifier and time range and can follow one purchase from checkout through payment, fulfillment and after-sales.
- **AC-UC-024-08 — View authorization (Proposed):** inspection is limited to authorized Internal Staff roles and, for Shop owners, to their own Shop's records; other actors see no records.
- **AC-UC-024-09 — Redaction (Proposed):** credentials, one-time codes, callback secrets and full payment/address data are never present in readable form in audit records or query results.
- **AC-UC-024-10 — Idempotent repeats (Proposed):** an exact repeat producing no new business effect produces no new audit effect.
- **AC-UC-024-11 — Rejection reasons (Proposed):** moderation, dispute and Seller rejection records include the reason supplied, consistent with BR-PROD-001.
- **AC-UC-024-12 — Retention (Proposed, depends on OQ-012):** records remain retrievable for at least the confirmed demo retention period; the period is not assumed before confirmation.

## Traceability

- SC-022 (supports SC-023) → UC-024 → [BR-AUDIT-001](../BUSINESS_RULES.md#br-audit-001), [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-024-01 through AC-UC-024-12. Per-use-case audit criteria remain in UC-001 through UC-017.

## Open questions

[OQ-012](../OPEN_DECISIONS.md#oq-012) retention, redaction and failed-attempt scope; [OQ-005](../OPEN_DECISIONS.md#oq-005) viewer permissions.
