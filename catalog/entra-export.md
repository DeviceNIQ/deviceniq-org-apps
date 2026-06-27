# Entra catalog export

Refresh tenant inventory into `exports/` for syncing `assignments.md` files.

## Quick export (signed-in user scopes)

Works with O365 mail MCP token — limited to `/me`, `/organization`:

```powershell
powershell -File scripts\export-entra-catalog.ps1 deviceniq-org-apps\exports
```

Output: `exports/entra-catalog-export.json`

## Full export (recommended)

Requires **Global Reader** or **Application Administrator** + **User.Read.All**:

```powershell
# Install once
Install-Module Microsoft.Graph -Scope CurrentUser

Connect-MgGraph -Scopes `
  Organization.Read.All, `
  Directory.Read.All, `
  Application.Read.All, `
  AppRoleAssignment.ReadWrite.All, `
  User.Read.All

# Users
Get-MgUser -All -Property Id,DisplayName,UserPrincipalName,AssignedLicenses |
  ConvertTo-Json -Depth 5 | Set-Content deviceniq-org-apps\exports\users.json

# Enterprise apps + assignments
Get-MgServicePrincipal -Filter "servicePrincipalType eq 'Application'" -All |
  Select-Object Id,DisplayName,AppId,AppRoles |
  ConvertTo-Json -Depth 6 | Set-Content deviceniq-org-apps\exports\service-principals.json

# Per-app user assignments (sample loop)
$sp = Get-MgServicePrincipal -Filter "displayName eq 'AWS IAM Identity Center'"
Get-MgServicePrincipalAppRoleAssignedTo -ServicePrincipalId $sp.Id -All
```

## Sync into app docs

After export, update each `apps/<slug>/assignments.md` from:

| Export field | Doc field |
|--------------|-----------|
| `appRoleAssignments` | User → app role table |
| `users` | DeviceNIQ user list in office365 |
| `subscribedSkus` | office365/identity/licenses.md |

## Azure Entra MCP

Cursor **azure-entra** MCP can query users and apps when authenticated with admin scopes — use for ad-hoc lookups; commit structured results via this export workflow.
