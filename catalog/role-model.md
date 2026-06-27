# Role model (all apps)

## Role tiers

DeviceNIQ uses a consistent pattern across documented apps:

| Role | Typical Entra app role value | Permissions |
|------|------------------------------|-------------|
| **Admin** | `App.Admin` | Configure SSO, assign roles, full app admin |
| **Operator** | `App.Operator` | Day-to-day operations (deploy, manage resources) |
| **User** | `App.User` | Standard access |
| **ReadOnly** | `App.ReadOnly` | View-only / audit |

SaaS apps from the gallery (Atlassian, AWS IAM IC, Lucid) ship **vendor-defined app roles** — map them in each app's `roles.md`.

## Assignment rules

| Person | Tenant UPN | Typical assignments |
|--------|------------|---------------------|
| RamaKrishna Manchana | `manchana.ramakrishna@deviceniq.com` | Admin on core apps (Office 365, AWS, Azure, GitHub org owner) |
| Veda Manchana | `manchana.veda@deviceniq.com` | User on Office 365; no infra admin by default |
| Suresh Karri | `skarri@proterra.com` | **External** — Proterra mail only; not DeviceNIQ Entra unless guest B2B |
| Stina Brock | `stina.brock@gmail.com` | **External** — contractor; guest B2B if Entra access needed |

## Guest (B2B) access

For `@proterra.com` or `@gmail.com` collaborators:

1. Entra → **Users** → **New guest user**
2. Assign to enterprise app → **User** role only
3. Document in `apps/<app>/assignments.md`

## App role ID convention

Custom DeviceNIQ app registrations should define roles in manifest:

```json
"appRoles": [
  {
    "allowedMemberTypes": ["User"],
    "description": "Full administrative access",
    "displayName": "Admin",
    "id": "<generated-guid>",
    "isEnabled": true,
    "value": "Admin"
  }
]
```

Assign via Entra → Enterprise app → **Users and groups** → **Add user/group** → select **role**.

## Mail vs Entra

Mail folders (`000-deviceniq`, `000-proterra`) organize **email** only. Entra roles control **application access**. See [apps/office365/mail/rules.md](../apps/office365/mail/rules.md).
