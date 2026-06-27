# Microsoft 365 licenses

Tenant: `bdf0104c-0abf-45b1-bfb3-ca27784a1b34`

## Export (admin required)

Mail MCP delegated token cannot read `/subscribedSkus` or user `assignedLicenses` without **Organization.Read.All** / **Directory.Read.All**.

### Recommended export

```powershell
Connect-MgGraph -Scopes Organization.Read.All, Directory.Read.All

# Tenant SKUs
Get-MgSubscribedSku | Select-Object SkuPartNumber, SkuId, ConsumedUnits,
  @{N='Enabled';E={$_.PrepaidUnits.Enabled}} | Format-Table

# Per user
Get-MgUser -All -Property DisplayName, UserPrincipalName, AssignedLicenses |
  Where-Object { $_.AssignedLicenses } |
  Export-Csv deviceniq-org-apps/apps/office365/identity/licenses-export.csv -NoTypeInformation
```

Save JSON snapshot:

```powershell
az rest --method GET --url "https://graph.microsoft.com/v1.0/subscribedSkus"
```

## Placeholder — fill after admin export

| SKU (SkuPartNumber) | Assigned | Consumed | Notes |
|---------------------|----------|----------|-------|
| *(run export)* | | | |

## Related users

See [users.md](users.md) for mailbox routing. License assignment should match UPNs listed there.
