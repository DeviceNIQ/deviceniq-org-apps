# Users — mail routing & known identities

> Full Entra user export requires **Directory.Read.All** (Global Reader or admin). Mail-scoped token can only infer from mailbox history.

## DeviceNIQ (`000-deviceniq` folder)

| Display name | Primary email | Notes |
|--------------|---------------|-------|
| RamaKrishna Manchana | `manchana.ramakrishna@deviceniq.com` | Mailbox owner |
| Veda Manchana | `manchana.veda@deviceniq.com` | Family / DeviceNIQ |

Also routed to **000-deviceniq:** `rmanchana@proterra.com` (same person, legacy Proterra SMTP).

## Proterra (`000-proterra` folder)

| Display name | Email | Notes |
|--------------|-------|-------|
| Suresh Karri | `skarri@proterra.com` | Primary work |
| Suresh Karri | `suresh.k.karri@gmail.com` | Personal |
| Stina Brock | `stina.brock@gmail.com` | Contractor |
| *(any)* | `*@proterra.com` | Other Proterra colleagues (Erika Coplen, Giovanna Malhotra, …) |

## Refresh user list from Entra (admin)

```powershell
# Azure CLI + Graph (Global Reader)
az login --tenant bdf0104c-0abf-45b1-bfb3-ca27784a1b34
az rest --method GET --url "https://graph.microsoft.com/v1.0/users?\`$select=displayName,mail,userPrincipalName,jobTitle,assignedLicenses"
```

Save output to `identity/users-export.json` and update this doc.

## Mail-derived senders (sample)

From mailbox search (`export-o365-org-data.ps1`):

- `manchana.ramakrishna@deviceniq.com`, `manchana.veda@deviceniq.com`
- `skarri@proterra.com`, `stina.brock@gmail.com`, `ecoplen@proterra.com`, `gmalhotra@proterra.com`
- `vtatiparthi@innominds.com` → routed to **0003-dlms**
