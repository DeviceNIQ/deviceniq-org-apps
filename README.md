# DeviceNIQ — Organization apps catalog

Git remote: **`DeviceNIQ/deviceniq-org-apps`**

Central registry of **Entra ID / My Apps** applications and **DeviceNIQ products** for the tenant (`bdf0104c-0abf-45b1-bfb3-ca27784a1b34`, `deviceniq.com`).

- **`apps/`** — My Apps tiles, vendor SSO, mail folders (`009-*`): README, roles, assignments
- **`products/`** — DeviceNIQ-owned platforms (Cognito, product architecture, lifecycles)

## My Apps home

User portal: [https://myapplications.microsoft.com/](https://myapplications.microsoft.com/)  
Admin: Entra ID → **Enterprise applications** → **My Apps** settings / **Collections**

See [catalog/my-apps-home.md](catalog/my-apps-home.md) and [catalog/role-model.md](catalog/role-model.md).

## Apps

| App | Folder | My Apps / Entra name | Status |
|-----|--------|----------------------|--------|
| **Microsoft 365** | [apps/office365](apps/office365/) | Office 365 / Exchange / Entra core | Documented (mail rules, identity) |
| **AWS** | [apps/aws](apps/aws/) | AWS IAM Identity Center | SSO + app role assigned |
| **Atlassian** | [apps/atlassian](apps/atlassian/) | Atlassian Cloud, Atlassian | SSO |
| **Google Cloud** | [apps/google-cloud](apps/google-cloud/) | Google Cloud / G Suite Connector | SSO + app role |
| **GitHub** | [apps/github](apps/github/) | *(GitHub org — PAT/OAuth)* | Mail + DeviceNIQ org |
| **Azure** | [apps/azure](apps/azure/) | Microsoft Azure, Graph Explorer | Portal + MCP |
| **Lucid** | [apps/lucidchart](apps/lucidchart/) | Lucid | SSO |
| **Slack** | [apps/slack](apps/slack/) | Slack | Vendor account |
| **Figma** | [apps/figma](apps/figma/) | Figma | Vendor account |
| **GoDaddy** | [apps/godaddy](apps/godaddy/) | GoDaddy / domains | Vendor account |
| **diagrams.net** | [apps/diagrams-net](apps/diagrams-net/) | diagrams.net | SSO |

## Products

| Product | Folder | Auth | Status |
|---------|--------|------|--------|
| **DeviceNIQ Chargers** | [products/chargers](products/chargers/) | Cognito (prod) | Product docs + MCP |

## Tenant snapshot

| Field | Value |
|-------|-------|
| Organization | deviceniq.com |
| Default domain | `NETORG19372448.onmicrosoft.com` |
| Federated domain | `deviceniq.com` |
| Primary admin contact | RamaKrishna Manchana (`manchana.ramakrishna@deviceniq.com`) |

## Scripts (deviceniq-cursor-workspace)

```powershell
# Export Entra catalog (admin scopes for full inventory)
powershell -File scripts\export-entra-catalog.ps1 deviceniq-org-apps\exports

# Office 365 mail rules + export
powershell -File scripts\setup-o365-mail-org.ps1

# MCP setup for all org apps
powershell -File scripts\setup-org-apps-mcp.ps1
```

## MCP integration

See [DeviceNIQ/deviceniq-cursor-workspace/docs/org-apps-mcp.md](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/docs/org-apps-mcp.md) for server mapping per app.

## Exports

Raw Graph dumps: `exports/` (gitignored). Refresh after admin export — see [catalog/entra-export.md](catalog/entra-export.md).

## Adding a new app

1. Create `apps/<app-slug>/` from [catalog/app-template](catalog/app-template/)
2. Register in Entra (app registration + enterprise app) if SSO
3. Add My Apps collection tile / homepage URL
4. Define `roles.md` and `assignments.md`
5. Add row to this README
