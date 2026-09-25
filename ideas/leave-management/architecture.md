# Leave & Coverage — V1 Architecture

## System of record

The `leave-management-backend` plugin owns leave-request data in PostgreSQL. RHDH is the new system of record; SharePoint is not part of the runtime design.

## Components

| Component | Responsibility |
| --- | --- |
| `leave-management` frontend plugin | My Leave and Team Approvals user interfaces. |
| `leave-management-backend` plugin | Authorization, business rules, request workflow, audit events, and notifications. |
| PostgreSQL | Leave requests, approval decisions, and audit history. |
| Keycloak/RHDH catalog | Authenticated identity, display name, email, and group membership. |
| Approved object storage | Attachment content; PostgreSQL stores attachment metadata only. |
| Email / RHDH notifications | Request, decision, and cancellation notifications. |

## Approval routing

1. Backend reads the authenticated RHDH user identity.
2. It verifies that the user belongs to an eligible configured group.
3. It derives the employee section and normal approver from configuration.
4. If the requester is a supervisor, it routes their request to that supervisor's `delegateApproverUserRef`.
5. Backend rejects any decision where the approver and requester are the same user.

## Core states

`Pending` → `Approved` or `Rejected`

An employee can cancel a `Pending` request. Every state change creates an immutable audit event.

## Security

- Frontend never selects its own employee identity, section, or approver.
- Backend verifies RHDH/Keycloak identity and group membership on every request.
- Supervisors view only requests assigned to them or their configured section.
- Credentials are stored in OpenShift secrets, never `app-config.yaml`.
