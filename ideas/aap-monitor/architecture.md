# AAP Monitor — V1 Architecture

## Purpose

Let DevHub users monitor approved AAP automation runs without navigating to the AAP user interface.

## Components

| Component | Responsibility |
| --- | --- |
| DevHub frontend plugin | Displays the AAP Monitor page, run list, and run-detail drawer. |
| DevHub backend plugin | Reads configuration, calls the AAP API, normalizes responses, and exposes safe DevHub APIs. |
| AAP | Runs automation jobs and owns job status, workflow nodes, stdout, and job events. |
| `app-config.yaml` | Defines the approved AAP templates and refresh/display settings. |
| OpenShift secret | Supplies the backend-only AAP service-account token. |

## Request Flow

1. The user opens **AAP Monitor** in DevHub.
2. The frontend calls the DevHub backend `/api/aap-monitor/runs` endpoint.
3. The backend reads the approved template list from `app-config.yaml`.
4. The backend queries AAP for matching Job Template and Workflow Job Template runs.
5. The backend returns normalized run data to the frontend.
6. The user selects a run; the frontend requests its detail and latest AAP output.

## Refresh Modes

- **Polling mode:** The frontend refreshes the run list at `refresh.intervalSeconds`.
- **Streaming mode:** The frontend connects to the DevHub backend SSE endpoint. The backend checks AAP for updates at `refresh.aapPollIntervalSeconds` and forwards changes to connected browsers.
- **Fallback:** If SSE disconnects, the frontend returns to polling automatically.

## Security

- The browser never communicates with AAP directly.
- The AAP token is available only to the DevHub backend through an OpenShift secret.
- The AAP service account should have read-only access limited to monitored templates.
- Existing DevHub RBAC protects access to the monitor page and backend routes.

## V1 Design Principle

AAP remains the source of truth. DevHub reads and displays job information; it does not own, change, or launch AAP jobs in this plugin.
