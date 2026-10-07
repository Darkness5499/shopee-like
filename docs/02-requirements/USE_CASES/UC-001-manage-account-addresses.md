# UC-001 — Manage account, profile, and delivery addresses

- Implementation status: Not implemented — deferred; closed for current work. Resume only when the owner selects a later part.
- Planning authority: [Learning plan](../../01-product/LEARNING_PLAN.md); document maturity below is separate from implementation status.

## Metadata

- Status: Draft; all account, verification, session and address policy is Proposed under [OQ-020](../OPEN_DECISIONS.md#oq-020) and [OQ-005](../OPEN_DECISIONS.md#oq-005).
- Owner: Project owner.
- Priority: P0 — Proposed in the [catalog](../USE_CASE_CATALOG.md).
- Scope/source: SC-001, [Functional Scope](../../01-product/FUNCTIONAL_SCOPE.md).

## Goal

Let a person register one User account, prove control of a contact channel, authenticate, and maintain the profile and delivery addresses that other journeys use. One verified User may act as Buyer and as Seller / Shop Operator.

## Primary actor

User (Buyer or prospective Seller).

## Supporting actors

Simulated verification channel (a demo notification that delivers a one-time code). Admin may handle internal staff accounts, which are outside this use case ([UC-002](UC-002-register-manage-shop.md) and [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001) cover the internal permission model).

## Trigger

A person registers, signs in, verifies an account, edits a profile, or adds/changes/removes an address.

## Preconditions

- Registration needs a unique login identifier (proposed: email) and a credential that satisfies the credential policy. Exact rules remain OQ-020.
- Address and profile changes require an authenticated session for the owning User.

## Main flow

1. A person submits registration data. The system validates it and creates a User in `UNVERIFIED` ([SM-ACCOUNT-001](../STATE_MACHINES.md#sm-account-001)).
2. The system issues a one-time verification code through the simulated channel with a bounded lifetime.
3. The User submits the code. A correct, unexpired, unused code moves the User to `ACTIVE` and consumes the code.
4. The User signs in. The system authenticates, starts a session for that User only, and records the attempt outcome.
5. The User maintains profile fields (display name, contact phone) and delivery addresses. The first address becomes the default; one default exists at any time.
6. The system records attributable audit entries for registration, verification, credential changes, profile and address changes ([NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004)).

## Alternative and error flows

- **A1 — Duplicate identifier:** registration with an identifier already in use is refused without revealing credential data and without creating a second account.
- **A2 — Invalid, expired or reused code:** verification is refused and the User stays `UNVERIFIED`; a new code may be requested subject to a resend limit, and issuing a new code invalidates earlier codes.
- **A3 — Repeated failed sign-in:** after the proposed consecutive-failure threshold, sign-in for that account is temporarily locked for a bounded period; the lock expires without staff action.
- **A4 — Unverified User attempts a protected action:** checkout, Shop registration and payment are refused until verified; browsing remains available. Cart use by unverified Users is OQ-020.
- **A5 — Restricted or suspended User:** sign-in/protected actions follow the state's guards. Staff-imposed restrictions are [UC-021](../USE_CASE_CATALOG.md#uc-021--handle-user-and-shop-violations) and OQ-004; this use case only honors the state.
- **A6 — Address validation failure:** an address missing a required field or exceeding limits is refused without partial change.
- **A7 — Address used by purchases:** editing or deleting an address never changes the delivery-address snapshot of an accepted Order ([BR-ORDER-001](../BUSINESS_RULES.md#br-order-001)).
- **A8 — Cross-User access:** reading or changing another User's profile or address is refused with no data change.
- **A9 — Repeated request:** an identical verification or registration retry returns the existing outcome without a second account, a second consumed code or a duplicate audit effect.

## Business rules

[BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001) (accounts, sessions, addresses and the permission model), [SM-ACCOUNT-001](../STATE_MACHINES.md#sm-account-001), [BR-ORDER-001](../BUSINESS_RULES.md#br-order-001) for address snapshots.

## State/data changes

Creates and updates the User, verification code records, sessions, profile and addresses. Credentials are never returned or logged in readable form. Deleting an address removes it from future selection only.

## Postconditions

- Success: one `ACTIVE` User per identifier with attributable records; addresses belong only to that User with a single default.
- Failure: no account, state, session or address change beyond the recorded failed-attempt evidence.

## Acceptance criteria

- **AC-UC-001-01 — Registration (Proposed):** given a unique identifier and a credential meeting the policy, registration creates exactly one `UNVERIFIED` User and issues one verification code; the credential is never retrievable in readable form.
- **AC-UC-001-02 — Duplicate identifier (Proposed):** registering an identifier already in use creates no account and reveals no credential or profile data of the existing account.
- **AC-UC-001-03 — Verification success (Proposed):** a correct, unexpired, unused code moves the User to `ACTIVE` and cannot be used again.
- **AC-UC-001-04 — Verification failure (Proposed):** an incorrect, expired or already-used code leaves the User `UNVERIFIED`; only the most recently issued code is valid and resend attempts beyond the limit are refused.
- **AC-UC-001-05 — Sign-in (Proposed):** valid credentials for an `ACTIVE` User create a session scoped to that User; an `UNVERIFIED` User can sign in only for permitted browsing actions.
- **AC-UC-001-06 — Temporary lock (Proposed):** reaching the consecutive-failure threshold refuses further sign-in for the bounded period without disclosing whether the identifier exists; a correct credential during the lock does not succeed.
- **AC-UC-001-07 — Protected actions need verification (Proposed):** an `UNVERIFIED` User cannot checkout, pay or register a Shop; each refusal leaves no Order, hold, payment or Shop record.
- **AC-UC-001-08 — Profile update (Proposed):** an authenticated User can update permitted profile fields; invalid values are refused without partial change.
- **AC-UC-001-09 — Address maintenance (Proposed):** an authenticated User can add, edit, delete and select a default address within the limits; exactly one default exists whenever at least one address exists.
- **AC-UC-001-10 — Address snapshot isolation (Proposed):** editing or deleting an address does not change the delivery-address snapshot of any accepted Order or Shipment.
- **AC-UC-001-11 — Cross-User denial (Proposed):** a User cannot read or change another User's profile or addresses; protected data stays unchanged.
- **AC-UC-001-12 — Restricted states honored (Proposed):** a `RESTRICTED` or `SUSPENDED` User cannot perform actions disallowed by that state, while previously accepted Orders remain visible to the User per [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001).
- **AC-UC-001-13 — Idempotent repeats (Proposed):** an identical registration or verification retry returns the existing outcome with no second account, no extra audit effect and no re-consumed code.
- **AC-UC-001-14 — Audit (Proposed):** registration, verification, sign-in outcome, credential change, profile and address changes produce attributable records without credentials or one-time codes in readable form.

## Traceability

- SC-001 → UC-001 → [BR-ACCESS-001](../BUSINESS_RULES.md#br-access-001), [SM-ACCOUNT-001](../STATE_MACHINES.md#sm-account-001), [BR-ORDER-001](../BUSINESS_RULES.md#br-order-001), [NFR-001](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-001), [NFR-004](../NON_FUNCTIONAL_REQUIREMENTS.md#nfr-004) → AC-UC-001-01 through AC-UC-001-14.

## Open questions

[OQ-020](../OPEN_DECISIONS.md#oq-020) account, verification, credential and address policy; [OQ-005](../OPEN_DECISIONS.md#oq-005) permission model; [OQ-004](../OPEN_DECISIONS.md#oq-004) restricted states; [OQ-012](../OPEN_DECISIONS.md#oq-012) audit retention.
