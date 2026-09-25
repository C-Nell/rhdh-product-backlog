# Leave & Coverage

An RHDH plugin for submitting, approving, and viewing team leave and telework coverage.

## Two views

| View | Who sees it | Purpose |
| --- | --- | --- |
| **My Leave** | Every eligible SASS/XBAGS user | Submit leave, view personal requests, and cancel pending requests. |
| **Team Approvals** | Configured supervisors and leave administrators | Review requests, approve/reject, validate in SETR, and view team coverage. |

A supervisor sees both tabs. Their own leave request automatically routes to their configured delegated approver, so self-approval is never possible.

## Files

- `employee-request-mockup.png` — My Leave view.
- `supervisor-dashboard-mockup.png` — Team Approvals view.
- `jira-story.md` — implementation-ready Jira story.
- `architecture.md` — V1 architecture and data ownership.
- `app-config-example.yaml` — access, routing, display, notification, and field settings.
