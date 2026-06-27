# Inbox mail rules

Rules apply to **Inbox** only. Each rule moves matching mail and stops further rules (`stopProcessingRules: true`).

**Sequence:** lower number runs first. People rules (1–3) run before vendor rules.

## People & projects

### 000-deviceniq → `Inbox/000-deviceniq`

| Match type | Value |
|------------|-------|
| From (exact) | `manchana.ramakrishna@deviceniq.com` |
| From (exact) | `manchana.veda@deviceniq.com` |
| From (exact) | `rmanchana@proterra.com` (RamaKrishna legacy Proterra address) |

### 000-proterra → `Inbox/000-proterra`

| Match type | Value |
|------------|-------|
| From (exact) | `skarri@proterra.com` (Suresh Karri) |
| From (exact) | `stina.brock@gmail.com` (Stina Brock) |
| From (exact) | `suresh.k.karri@gmail.com` (Suresh personal) |
| Sender contains | `@proterra.com` (other Proterra colleagues) |

### 009-utility → `Inbox/009-utility`

| Match type | Value |
|------------|-------|
| From (exact) | `no-reply@verificationemail.com` |
| From (exact) | `mssecurity-noreply@microsoft.com` |
| Sender contains | `verificationemail.com`, `mssecurity-noreply@microsoft.com` |

## Vendors & services

| Rule | Sender contains | Target folder |
|------|-----------------|---------------|
| 009-slack | `slack.com` | `009-slack` (root) |
| 009-figma | `figma.com` | `009-figma` |
| 009-godaddy | `godaddy.com` | `009-godaddy` |
| 009-atlassian | `atlassian.net`, `atlassian.com` | `009-atlassian` |
| 009-aws | `amazonaws.com`, `aws.amazon`, `registrar.amazon`, … | **Inbox/009-aws** |
| 009-github | `github.com` | **Inbox/009-github-actions** |
| 009-google-cloud | `googlecloud.com`, `cloud.google.com`, `accounts.google.com`, … | `009-google-cloud` |
| 009-lucidchart | `lucidchart.com`, `lucid.co` | `009-lucidchart` |
| 009-azure | `azure.microsoft.com`, `o365mc@microsoft.com`, … | `009-azure` |
| 0003-dlms | `dlms`, `innominds.com` | **Inbox/0003-dlms** |
| 0009-insurance | `insurance`, `fmr.com`, `vsp.com` | **Inbox/0009-insurance** |

## Bulk move (one-time / re-run)

`setup-o365-mail-org.ps1` also searches the **entire mailbox** and moves historical mail into these folders (not only Inbox).

Last run summary (2026-06-27): 542 messages moved across all categories.

## Export

Machine-readable: [rules-export.json](rules-export.json)

Regenerate:

```powershell
powershell -File scripts\setup-o365-mail-org.ps1
```

## Outlook UI

Settings → Mail → Rules — rules appear as `000-deviceniq`, `000-proterra`, `009-*`, etc.
