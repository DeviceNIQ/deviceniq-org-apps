# Mail folder structure

Mailbox: `manchana.ramakrishna@deviceniq.com`

## Naming convention

| Prefix | Purpose | Location |
|--------|---------|----------|
| `000-*` | People, projects, personal admin | **Under Inbox** |
| `009-*` | SaaS / vendor / automated services | Root **or** under Inbox (legacy) |

## Under Inbox (active)

| Folder | Purpose |
|--------|---------|
| **000-self** | Mail **from RamaKrishna’s personal email IDs** (Gmail, Outlook personal, etc.) |
| **000-deviceniq** | Mail from RamaKrishna Manchana & Veda Manchana (DeviceNIQ **work** addresses) |
| **000-proterra** | Mail from Suresh Karri, Stina Brock, and `@proterra.com` |
| **0003-dlms** | DLMS project (Innominds, DLMS threads) |
| **0009-insurance** | Insurance / benefits (Fidelity `fmr.com`, VSP, etc.) |
| **000-perm** | PERM / immigration (manual) |
| **009-utility** | System utility mail (verification codes, security noreply) |
| **009-github-actions** | GitHub Actions / CI notifications (rule target) |
| **009-aws** | Legacy AWS subfolder (some mail; rule also uses this) |

## At mailbox root (vendor archives)

Bulk-moved vendor mail lives here: `009-slack`, `009-figma`, `009-godaddy`, `009-atlassian`, `009-aws`, `009-github`, `009-google-cloud`, `009-lucidchart`, `009-azure`.

New **inbox rules** prefer Inbox subfolders when both exist (e.g. GitHub → `Inbox/009-github-actions`).

## Cleanup candidates (empty / typo)

| Folder | Notes |
|--------|-------|
| `009-godadddy` | Typo — delete when convenient |
| `009-attlassian-confluence` | Superseded by `009-atlassian` rule |

## Related scripts

- `scripts/setup-o365-mail-org.ps1` — create folders, rules, bulk move, export JSON
- `scripts/discover-mailbox.ps1` — audit folder counts
