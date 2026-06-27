# Microsoft 365 (Office 365)

**App slug:** `office365`  
**Entra tenant:** `bdf0104c-0abf-45b1-bfb3-ca27784a1b34`  
**My Apps tile:** Microsoft 365 / Office / Outlook (native)

## Overview

DeviceNIQ productivity suite: Exchange Online, Entra ID, Teams (optional), SharePoint, and admin portals. This app folder documents **mail organization**, **licenses**, and **identity** for the tenant.

## Quick links

| Service | URL |
|---------|-----|
| Outlook web | https://outlook.office.com/mail/ |
| Microsoft 365 admin | https://admin.microsoft.com/ |
| Entra admin | https://entra.microsoft.com/ |
| My Apps | https://myapplications.microsoft.com/?tenantid=bdf0104c-0abf-45b1-bfb3-ca27784a1b34 |

## Documentation

| Topic | Path |
|-------|------|
| Mail folders | [mail/folder-structure.md](mail/folder-structure.md) |
| Inbox rules | [mail/rules.md](mail/rules.md) |
| Users | [identity/users.md](identity/users.md) |
| App registrations | [identity/app-registrations.md](identity/app-registrations.md) |
| Licenses | [identity/licenses.md](identity/licenses.md) |

## Related MCP (Cursor)

| Integration | Doc |
|-------------|-----|
| O365 mail MCP | [docs/o365-mail-mcp.md](../../docs/o365-mail-mcp.md) in cursor-workspace |
| Azure Entra MCP | `.cursor/mcp.json.example` |

## Owners

| Role | Person |
|------|--------|
| Primary admin | RamaKrishna Manchana |
| Backup | *(assign)* |

## Mail automation scripts

```powershell
powershell -File scripts\setup-o365-mail-org.ps1
powershell -File scripts\export-entra-catalog.ps1 deviceniq-org-apps\exports
```
