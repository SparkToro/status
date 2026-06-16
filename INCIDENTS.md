# Incident Runbook

This status page runs on [Upptime](https://upptime.js.org). **Incidents are GitHub Issues in this repo** — there's no separate dashboard. Opening, commenting on, and closing issues is how you drive the public status page at https://status.sparktoro.com.

The page rebuilds automatically a couple of minutes after any issue change.

## Site slugs

Each monitored site has a label named after its slug. Apply the matching label so the incident shows under the right component:

| Component | Label |
|-----------|-------|
| Website | `website` |
| Public API | `public-api` |
| MCP Server | `mcp-server` |
| API Docs | `api-docs` |

## Report an incident

Open an issue, title it with the customer-facing summary, label it with the affected site(s), and write the first update in the body.

```sh
gh issue create --repo SparkToro/status \
  --title "Public API elevated error rate" \
  --label public-api \
  --body "Investigating elevated 5xx responses on the public API. Updates to follow."
```

Affecting more than one component? Add multiple labels (`--label public-api --label mcp-server`).

## Post an update

Comment on the issue. Each comment becomes a timeline entry on the incident.

```sh
gh issue comment <number> --repo SparkToro/status \
  --body "Identified — bad deploy to the API origin. Rolling back now."
```

Conventional update labels people use in the body or as a prefix: **Investigating → Identified → Monitoring → Resolved.**

## Resolve

Close the issue. The page marks the incident resolved and the component returns to operational.

```sh
gh issue close <number> --repo SparkToro/status \
  --comment "Resolved — rollback complete, error rates back to normal."
```

## Auto-incidents

When a monitored check fails, Upptime opens one of these issues automatically (with the correct site label) and closes it on recovery. Manual issues are for what the checks can't see: degraded performance, partial outages, or a heads-up before customers notice.

## Scheduled maintenance

Use the **Maintenance Event** issue template. The HTML comment block at the top is what drives it:

```
<!--
start: 2026-06-20T13:00:00.000Z
end: 2026-06-20T14:00:00.000Z
expectedDown: public-api, mcp-server
-->
```

- Times are UTC, ISO 8601.
- `expectedDown` is a comma-separated list of site slugs.

During the window the page shows a maintenance banner and the downtime is excluded from uptime numbers.

## Configuration

Monitored sites, branding, and intro text live in [`.upptimerc.yml`](./.upptimerc.yml). The public API is checked at `api.sparktoro.com/health` and the MCP server at `mcp.sparktoro.com/health`.
