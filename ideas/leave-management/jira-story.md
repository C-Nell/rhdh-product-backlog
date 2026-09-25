# Build Leave & Coverage Plugin for DevHub

## Goal

Create a new RHDH leave-management system of record that replaces the existing SharePoint OOO list. Eligible SASS/XBAGS users can submit leave requests; configured supervisors can approve requests and quickly see team coverage.

## Frontend

- Build a **My Leave** view for eligible users.
- Prefill employee name, email, and section from the authenticated RHDH user profile.
- Do not let users edit their identity, section, or approver.
- Support leave type, start/end dates, full/partial day, hours, notice, telework details, comments, and attachments.
- Show personal request history and allow cancellation of pending requests.
- Build a **Team Approvals** view visible only to configured supervisors and leave admins.
- Show pending approval queue, review panel, approve/reject actions, manager comments, SETR validation, and section coverage calendar.
- Supervisors must see both My Leave and Team Approvals tabs.

## Backend

- Create `leave-management-backend` using PostgreSQL as the system of record.
- Validate RHDH/Keycloak identity and eligible group membership for every endpoint.
- Derive section and normal supervisor routing from `app-config.yaml`.
- Route a supervisor's own request to their configured delegate approver.
- Block self-approval.
- Persist requests, attachments metadata, and immutable audit events.
- Send configured email and RHDH notifications on submit, approval, rejection, and cancellation.

## Configuration

- Implement the settings shown in `app-config-example.yaml`.
- Allow configuration of eligible groups, sections, supervisors, delegate approvers, leave types, notice types, telework options, display settings, and notifications.
- Keep credentials in OpenShift secrets only.

## Acceptance Criteria

- A user outside the eligible SASS/XBAGS groups cannot access the request form.
- Eligible users submit requests with prefilled RHDH identity and section.
- Supervisors can approve/reject requests assigned to their section.
- Supervisors can submit their own leave request, which routes to their delegate approver.
- No user can approve their own request.
- Approved requests appear on the section coverage calendar.
- All request decisions and cancellations are auditable.
- All settings are read from `app-config.yaml`.

## Definition of Done

- Frontend and backend plugins build and register in DevHub.
- PostgreSQL schema/migration and attachment-storage integration are implemented.
- Unit tests cover authorization, approval routing, self-approval prevention, and state transitions.
- Manual tests confirm employee, supervisor, delegated-approver, and leave-admin workflows.
- README documents configuration, required Keycloak groups, and deployment steps.
