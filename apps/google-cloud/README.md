# Google Cloud

**My Apps:** Google Cloud / G Suite Connector by Microsoft  
**Mail folder:** `009-google-cloud`

## Org status

DeviceNIQ **production runs on AWS** (EKS, RDS, etc.). There is **no org GCP project** for `@deviceniq.com` users today — the Entra connector tile is SSO to Google services, not GCP IAM access for `manchana.ramakrishna@deviceniq.com`.

Use this app folder for **vendor mail**, **My Apps catalog**, and future GCP assignments if the org adopts Google Cloud.

## Links

| Item | URL |
|------|-----|
| Google Cloud Console | https://console.cloud.google.com |
| Google Admin | https://admin.google.com |

## Cursor MCP (opt-in)

GCP MCP in Cursor requires a **Google account with a GCP project** (personal Gmail + your own project, or future org access). It is **not enabled by default**.

When you have access, setup from `deviceniq-cursor-workspace`:

```powershell
powershell -File scripts\setup-google-cloud-mcp.ps1
```

See [docs/google-cloud-mcp.md](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/docs/google-cloud-mcp.md) in the rules repo.

See [roles.md](roles.md) | [assignments.md](assignments.md)
