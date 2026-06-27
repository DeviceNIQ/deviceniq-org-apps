# Entra ID — app registrations

Tenant: `bdf0104c-0abf-45b1-bfb3-ca27784a1b34` (DeviceNIQ)

## Known apps (documented elsewhere)

| App / integration | Client ID | Purpose | Doc |
|-------------------|-----------|---------|-----|
| MS 365 MCP Server (built-in) | `084a3e9f-a9f4-43f7-89f9-d229cf97853e` | Cursor O365 mail MCP | [docs/o365-mail-mcp.md](../../docs/o365-mail-mcp.md) |
| Azure Entra MCP | `d64caf0e-6244-43e1-8973-050c99717547` | Cursor enterprise MCP | `.cursor/mcp.json.example` |
| GitHub | *(PAT)* | Cursor GitHub MCP | `docs/SETUP.md` |
| Atlassian MCP | *(OAuth)* | Jira/Confluence MCP | `scripts/atlassian-mcp-auth.ps1` |

## Full inventory export (admin required)

Mail MCP token does **not** have `Application.Read.All`. Use one of:

### Azure portal

Entra ID → App registrations → Export list (or PowerShell below).

### PowerShell

```powershell
Connect-MgGraph -Scopes Application.Read.All
Get-MgApplication -All | Select-Object DisplayName, AppId, CreatedDateTime, SignInAudience, PublisherDomain |
  ConvertTo-Json -Depth 3 | Set-Content deviceniq-org-apps/apps/office365/identity/apps-export.json
```

### Azure CLI

```powershell
az rest --method GET --url "https://graph.microsoft.com/v1.0/applications?\`$select=displayName,appId,createdDateTime,signInAudience,publisherDomain&\`$top=999"
```

## SaaS apps (no Entra registration)

These are vendor accounts tied to mail folders, not necessarily Entra apps:

| Service | Mail folder | Domain |
|---------|-------------|--------|
| Slack | 009-slack | slack.com |
| Figma | 009-figma | figma.com |
| GoDaddy | 009-godaddy | godaddy.com |
| Atlassian | 009-atlassian | atlassian.net |
| AWS | Inbox/009-aws | amazonaws.com |
| GitHub | Inbox/009-github-actions | github.com |
| Google Cloud / Workspace | 009-google-cloud | google.com |
| Lucidchart | 009-lucidchart | lucid.co |
| Microsoft Azure / M365 | 009-azure | microsoft.com |

Add rows here when new SaaS is onboarded.
