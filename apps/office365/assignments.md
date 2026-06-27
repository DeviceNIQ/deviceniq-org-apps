# Office 365 — role assignments

## DeviceNIQ users

| User | UPN | Directory / license roles | Mail folder | Notes |
|------|-----|---------------------------|-------------|-------|
| RamaKrishna Manchana | `manchana.ramakrishna@deviceniq.com` | Global Admin *(expected)*; all license SKUs TBD | — (mailbox owner) | Primary tenant admin |
| Veda Manchana | `manchana.veda@deviceniq.com` | **User** — standard M365 license | `Inbox/000-deviceniq` (mail from Veda) | Family / DeviceNIQ |

## External collaborators (mail only unless B2B guest)

| User | Email | Entra guest? | Mail folder |
|------|-------|--------------|-------------|
| Suresh Karri | `skarri@proterra.com` | No *(Proterra)* | `Inbox/000-proterra` |
| Stina Brock | `stina.brock@gmail.com` | No *(unless invited)* | `Inbox/000-proterra` |

## Custom app role assignments (target state)

| User | Office365.Admin | Office365.MailOperator | Office365.User |
|------|-----------------|------------------------|----------------|
| RamaKrishna Manchana | ✓ | ✓ | ✓ |
| Veda Manchana | | | ✓ |

## My Apps — related enterprise apps (same tenant)

From Entra `/me/appRoleAssignments` (2026-06-27):

| Enterprise app | User | Role |
|----------------|------|------|
| MS 365 MCP Server | RamaKrishna Manchana | Default |
| Graph Explorer | RamaKrishna Manchana | Default |

See sibling apps: [aws](../aws/assignments.md), [atlassian](../atlassian/assignments.md), [google-cloud](../google-cloud/assignments.md).

## Refresh

```powershell
powershell -File scripts\export-entra-catalog.ps1 deviceniq-org-apps\exports
# Requires Global Reader for all users; /me/appRoleAssignments works for signed-in user
```
