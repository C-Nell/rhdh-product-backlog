# Leave & Coverage — Supervisor Screen

## Purpose

Give section supervisors one clear workspace to answer two questions:

1. Which leave requests need my decision?
2. Who is out today or this week?

## Screen areas

### Pending approvals

Shows only requests assigned to the logged-in supervisor's section.

Each row displays employee, dates, leave type, notice, submitted time, and a **Review** action.

### Coverage calendar

Shows approved leave and telework for the supervisor's section. The header highlights the number of people out today and this week.

### Request review panel

Selecting a request opens the review panel. It shows request details, comments, attachments, and manager actions:

- Approve
- Reject
- Manager comments
- Validated in SETR (manager only)

## Access rules

- Employees can only submit and view their own requests.
- Supervisors can only review requests for sections assigned to their Keycloak/RHDH group.
- Leave administrators can view all sections.
- All employee and section data is derived from the authenticated RHDH user profile; users cannot manually change it.

## Mockups

- `employee-request-mockup.png` — employee request form and section coverage view.
- `manager-dashboard-mockup.png` — supervisor approval and coverage workspace.
