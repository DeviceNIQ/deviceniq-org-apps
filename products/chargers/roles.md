# Chargers — roles

Identity: **AWS Cognito** user pool + groups. JWT claim: `cognito:groups`.  
Effective permissions = **union** of all groups on the token.

Source of truth: `deviceniq-chargers/005-config/app.config.yaml`, `chargers_common/roles.py`.

---

## 1. Global Cognito roles (shipped)

| Role ID | Label | Permissions | UI capabilities | Cannot |
|---------|-------|-------------|-----------------|--------|
| **`chargers-admin`** | Administrator | `chargers:read`, `chargers:write`, `sessions:read`, `sessions:write`, `auth:manage` | Full fleet CRUD, sessions, user/role management, all exports | — |
| **`chargers-operator`** | Operator | `chargers:read`, `chargers:write`, `sessions:read`, `sessions:write` | Manage chargers, start/stop sessions, view history | `auth:manage`, Cognito admin |
| **`chargers-viewer`** | Viewer | `chargers:read`, `sessions:read` | View fleet and sessions (read-only) | Write chargers/sessions, user admin |
| **`manchana-family`** | Household org | *(none)* | Org grouping label on JWT | **Must** pair with `chargers-admin` / `operator` / `viewer` |

### Permission reference

| Permission | Resource | admin | operator | viewer |
|------------|----------|:-----:|:--------:|:------:|
| `chargers:read` | Charger fleet | ✓ | ✓ | ✓ |
| `chargers:write` | Charger fleet | ✓ | ✓ | |
| `sessions:read` | Sessions | ✓ | ✓ | ✓ |
| `sessions:write` | Sessions | ✓ | ✓ | |
| `auth:manage` | Users / invites | ✓ | | |

---

## 2. Onboarding roles (planned — plan 013)

Granted to **`chargers-admin`** (internal ops) via additional permission strings:

| Permission | admin | operator | viewer | Description |
|------------|:-----:|:--------:|:------:|-------------|
| `onboarding:read` | ✓ | | | View queues, audit |
| `onboarding:write` | ✓ | | | Create/edit draft entities |
| `onboarding:approve` | ✓ | | | Approve org/charger, commission |
| `organizations:read` | ✓ | ✓* | | Read org/site (*scoped) |
| `organizations:write` | ✓ | | | CRUD organizations |

Requires **VPN + ALB 1** for onboarding UI.

---

## 3. Organization-scoped roles (planned — `organization_users`)

Per-customer tenant role (stored in RDS, reflected in JWT claims after Ph 4):

| Role ID | Label | Permissions (target) | ChargePoint equivalent |
|---------|-------|----------------------|------------------------|
| `org_admin` | Organization admin | Full org fleet + invite users | Account admin |
| `org_operator` | Organization operator | Operate chargers/sessions in org | Station manager |
| `org_viewer` | Organization viewer | Read org fleet | Reports only |

Scoped by `organization_id` on every API query.

---

## 4. Future roles (roadmap)

| Role | Phase | Purpose |
|------|-------|---------|
| `driver` | 3 | IdTag holder; start/stop own sessions |
| `installer` | 3+ | Update `installation_jobs` only |
| `billing_admin` | 5+ | Tariffs, invoices |

---

## 5. Entra directory roles (engineering only)

Chargers **end users are not in Entra**. Engineering staff:

| Entra / AWS role | Used for |
|------------------|----------|
| Global Administrator | Tenant admin (minimal count) |
| AWS `production-admin` | EKS, RDS deploy |
| GitHub org Owner | `deviceniq-chargers` repo |

See [office365 identity](../../apps/office365/identity/users.md) for tenant admins.

---

## 6. ChargePoint mapping

| ChargePoint | DeviceNIQ |
|-------------|-----------|
| Network manager | `chargers-admin` + `onboarding:*` |
| Station manager | `chargers-operator` or `org_operator` |
| Reporting user | `chargers-viewer` or `org_viewer` |
| Driver | `driver` + IdTag (Ph 3) |
| Installer | `installer` + partner record |

---

## 7. Role assignment rules

1. Every human app user has **≥1** `chargers-*` group.
2. `manchana-family` is optional org label — never assign alone for API access.
3. Onboarding UI: require `chargers-admin` **and** `onboarding:approve` for commission actions.
4. Public API: filter chargers by `lifecycle_status ≥ commissioned` **and** user's org scope.

Assignment detail: [assignments.md](assignments.md) | User list: [users.md](users.md)
