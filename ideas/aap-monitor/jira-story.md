# Build Configurable AAP Monitor Plugin for DevHub

## Goal

Create a small DevHub plugin that shows AAP automation runs and live job output for a configured list of AAP templates.

## Frontend

Create an **AAP Monitor** page with:

- Template filter dropdown
- Running, Completed Today, and Failed Today cards
- Recent automation-runs list
- Run-detail drawer showing status, timing, job ID, requester, template, progress, and latest AAP output
- Manual refresh button and live-update indicator
- Loading, empty, and error states

## Backend

Create an `aap-monitor-backend` plugin that:

- Uses a backend-only read-only AAP service account/token.
- Queries AAP only for configured templates.
- Supports AAP Job Templates and Workflow Job Templates.
- Returns job ID, template, status, requester, start/finish time, duration, AAP URL, and latest stdout/events.
- Handles AAP API errors and timeouts gracefully.
- Never exposes AAP credentials to the browser.

Create endpoints:

```text
GET /api/aap-monitor/templates
GET /api/aap-monitor/runs
GET /api/aap-monitor/runs/:jobType/:jobId
GET /api/aap-monitor/events
```

## Configuration

Read all settings from `app-config.yaml`. See `app-config-example.yaml` in this folder.

Configuration behavior:

- `enabled: false` disables the plugin.
- `templates` is the Git-managed list of AAP templates to monitor.
- `streamingEnabled: false` uses frontend polling.
- `streamingEnabled: true` uses Server-Sent Events from DevHub to the browser.
- If streaming disconnects, the frontend falls back to polling.
- Do not hardcode AAP URLs, template IDs, refresh values, or log limits.

## Security

- Store `AAP_MONITOR_TOKEN` in an OpenShift/DevHub secret.
- Use a read-only AAP service account restricted to approved templates.
- Apply existing DevHub RBAC to the AAP Monitor route.
- The frontend communicates only with the DevHub backend.

## Acceptance Criteria

- Developers can add or remove monitored templates through `app-config.yaml`.
- The plugin displays runs from configured AAP Job and Workflow Templates.
- Users can filter by template and open run details.
- Users can view latest AAP output without opening AAP.
- Polling works using the configured refresh interval.
- Streaming can be enabled or disabled through configuration.
- Streaming failure falls back to polling.
- AAP errors and no-results conditions display useful UI messages.
- AAP credentials never appear in API responses or browser tools.
- Tested with one active, successful, and failed AAP run.

## Definition of Done

- Frontend and backend plugins build and register successfully in DevHub.
- AAP Monitor navigation entry is added.
- Configuration and secret setup are documented in the README.
- Unit tests cover configuration parsing, AAP response normalization, and backend error handling.
- Manual tests confirm polling, streaming, and fallback behavior.
- Code is reviewed and deployed to the DevHub development environment.
