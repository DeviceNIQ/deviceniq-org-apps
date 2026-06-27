# Office 365 — app roles

## Entra / M365 directory roles (tenant)

| Directory role | Assigned to (target) | Purpose |
|----------------|------------------------|---------|
| Global Administrator | RamaKrishna Manchana | Full tenant (minimize count) |
| Exchange Administrator | *(optional delegate)* | Mail rules, shared mailboxes |
| User Administrator | *(optional)* | User lifecycle |
| Global Reader | *(recommended for exports)* | Read-only Graph exports for this repo |

> Verify in Entra → **Roles and administrators**. Document actual assignments after admin export.

## Custom app roles (DeviceNIQ documentation app)

If registering **DeviceNIQ Office365 Docs** as a custom enterprise app for My Apps:

| App role | Value | Description |
|----------|-------|-------------|
| Admin | `Office365.Admin` | Run mail org scripts, edit rules, manage exports |
| Mail Operator | `Office365.MailOperator` | View mail docs; request rule changes |
| Identity Reader | `Office365.IdentityReader` | Read users/licenses exports |
| User | `Office365.User` | Standard M365 user; My Apps tile only |

## Microsoft 365 license SKUs

Document per-user SKUs in [identity/licenses.md](identity/licenses.md) after running:

```powershell
Connect-MgGraph -Scopes Organization.Read.All
Get-MgSubscribedSku | ft SkuPartNumber, ConsumedUnits
```

## Exchange inbox rules (not Entra roles)

Operational mail routing uses **Outlook inbox rules** — see [mail/rules.md](mail/rules.md). These are separate from Entra app role assignments.
