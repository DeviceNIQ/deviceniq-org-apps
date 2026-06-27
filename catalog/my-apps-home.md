# My Apps home page (Entra ID)

## URLs

| Portal | URL |
|--------|-----|
| **My Apps (users)** | https://myapplications.microsoft.com/ |
| **My Apps (tenant-scoped)** | https://myapplications.microsoft.com/?tenantid=bdf0104c-0abf-45b1-bfb3-ca27784a1b34 |
| **Entra admin — Enterprise apps** | https://entra.microsoft.com/#view/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/~/AppAppsPreview |
| **App launchers settings** | Entra → Identity → **Applications** → **App launchers** |

## Collections (recommended layout)

Group tiles on My Apps to match `deviceniq-org-apps`:

| Collection | Apps |
|------------|------|
| **DeviceNIQ — Core** | Microsoft 365, Microsoft Azure, GitHub |
| **DeviceNIQ — Engineering** | AWS, Atlassian, Google Cloud, Lucid, diagrams.net |
| **DeviceNIQ — Operations** | GoDaddy, Slack, Figma |

Configure: Entra → **Identity** → **Applications** → **App launchers** → **Collections**.

## Currently assigned (sample user)

From `/me/appRoleAssignments` export (RamaKrishna Manchana, 2026-06-27):

| Enterprise app | Default / app role |
|----------------|-------------------|
| AWS IAM Identity Center | App role `8774f594-1d59-4279-b9d9-59ef09a23530` |
| Atlassian Cloud | Default access |
| Atlassian | Default access |
| Google Cloud / G Suite Connector | App role `9e79c739-f165-49e8-8745-fe08f1634b3b` |
| Lucid | Default access |
| diagrams.net | Default access |
| Graph Explorer | Default access |
| MS 365 MCP Server | Default access |

Full export: [../exports/entra-catalog-export.json](../exports/entra-catalog-export.json)

## Homepage customization

Per enterprise application in Entra:

1. **Enterprise application** → **Properties**
   - **Home page URL** — link to app doc `README.md` on GitHub or internal wiki
   - **Logo** — upload 280×280 PNG
   - **Notes** — visible to admins; use for runbook link

2. **Users and groups** — assign users to **app roles** (see each app's `roles.md`)

3. **Single sign-on** — SAML/OIDC for SaaS; Office 365 uses native M365 sign-in

## DeviceNIQ custom app pages

Each folder under `apps/<name>/README.md` is the **canonical app page** content. Mirror key fields into Entra enterprise app **Properties** homepage URL pointing to:

`https://github.com/DeviceNIQ/deviceniq-org-apps/blob/main/apps/<name>/README.md`
