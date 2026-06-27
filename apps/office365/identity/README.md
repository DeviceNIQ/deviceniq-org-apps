# Identity documentation

Entra ID tenant: **DeviceNIQ** — `bdf0104c-0abf-45b1-bfb3-ca27784a1b34`

| Doc | Status |
|-----|--------|
| [users.md](users.md) | Populated from mailbox + known routing |
| [app-registrations.md](app-registrations.md) | Partial — MCP apps documented; full list needs admin export |
| [licenses.md](licenses.md) | Template — needs admin Graph export |

## Why some exports failed

`scripts/export-o365-org-data.ps1` uses the **O365 Mail MCP** token (Mail.ReadWrite, User.Read). It cannot call:

- `GET /users` (needs Directory.Read.All)
- `GET /applications` (needs Application.Read.All)
- `GET /subscribedSkus` (needs Organization.Read.All)

Use **Global Reader**, **Connect-MgGraph**, or **azure-entra** MCP with admin consent for full org inventory.

## Git repo

Documentation lives in **`DeviceNIQ/deviceniq-org-apps`** under `apps/office365/`:

```
deviceniq-org-apps/
  apps/office365/
    mail/
    identity/
  catalog/
  exports/          # gitignored raw Graph dumps
```

Scripts remain in `deviceniq-cursor-workspace/scripts/`.
