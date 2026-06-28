# Chargers — applications

DeviceNIQ Chargers is **not one monolithic app**. It is a **product suite** of separate deployable applications that share Cognito auth, the `chargers` MySQL schema on `prod-ams`, and the existing OCPP platform runtime.

| Application class | Who uses it | Network | ALB | Status |
|-------------------|-------------|---------|-----|--------|
| **DeviceNIQ Chargers Onboarding** | Internal DeviceNIQ ops (`chargers-admin`) | **VPN required** | ALB 1 | **MVP** — [deviceniq-chargers-onboarding](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding) |
| **Operational** | Fleet owners, operators, viewers (customers) | Public internet | ALB 3 | **Shipped** |
| **Platform tools** | Engineering / QA | Mixed | Platform | Partial |

**Handoff rule:** onboarding **commissions** a charger (`lifecycle_status → commissioned`); only then does it appear in the **operational** fleet UI and APIs.

Engineering plans: [013 internal onboarding](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/013-chargers-internal-onboarding-portal.md) · [014 landscape](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/014-chargers-onboarding-to-operational-landscape.md)

---

## 1. DeviceNIQ Chargers Onboarding (internal)

ChargePoint-class **customer, site, and charger provisioning** before anything is visible on the public internet.

### Access

| Item | Value |
|------|-------|
| **UI** | `https://onboarding.chargers.deviceniq.com` |
| **API** | `https://onboarding.api.chargers.deviceniq.com` |
| **Organizations API** | `https://organizations.api.chargers.deviceniq.com` *(or nested under onboarding)* |
| **Edge** | ALB 1 — VPN-restricted (`prod-shared-alb`) |
| **Auth** | Cognito JWT — **`chargers-admin`** + `onboarding:*` permissions |
| **Repo paths** | `000-microapps/onboarding/`, `001-microservices/onboarding/` in **deviceniq-chargers-onboarding** |

### Critical features (vs ChargePoint)

| Feature | Description |
|---------|-------------|
| **Unified audit timeline** | `onboarding_events` — every org/site/charger change with actor + timestamp; shown in UI sidebar |
| **Approval queues + KPIs** | Dashboard counts for pending customers/chargers; one-click submit/approve |
| **Commission readiness gate** | Checklist blocks commission until org approved, site, serial, connector, charger approval |


Connect **Nord VPN** (DeviceNIQ static IP) before opening the UI. Do **not** publish onboarding hostnames on ALB 3.

### Modules (target)

| Module | Type | Purpose |
|--------|------|---------|
| **onboarding** | Microapp | Queues, wizards, entity detail, audit timeline |
| **onboarding** | Microservice | Submit / approve / commission; `onboarding_events` |
| **organizations** | Microservice | CRUD organizations, sites, membership |

### Capabilities

| Capability | Phase | Notes |
|------------|-------|-------|
| Customer (organization) create / approve | MVP | `organizations` table |
| Sites under org | MVP | `sites` |
| Charger register (draft) | MVP | `chargers` + org/site FK |
| Approval queues & wizards | MVP | UI + `onboarding_events` |
| Contacts (billing, ops, legal) | Phase 2 | `contacts` |
| Charger model catalog | Phase 2 | `charger_models` |
| Connectors (ports) | Phase 2 | `connectors` |
| Installation jobs | Phase 2 | `installation_jobs` |
| Documents (permits, photos) | Phase 2 | `onboarding_documents` → S3 |
| Network / OCPP profile (metadata) | Phase 2 | `network_profiles` — links to existing OCPP stack |
| Commissioning checklist | Phase 2 | `commissioning_checklist_items` |
| **Commission gate** | MVP | Sets `commissioned` — unlocks operational app |
| Tariffs | Phase 3 | `tariff_plans`, `organization_tariffs` |
| Driver / RFID IdTags | Phase 3 | `id_tags` |
| Cognito org user invites | Phase 4 | `organization_users` |
| Onboarding SLA analytics | Phase 4 | DataOps pipeline |

**Not in onboarding UI:** session cards, live charging control, ML predictions, driver self-signup, payments.

### Roles

See [roles.md](roles.md) §2 — `onboarding:read`, `onboarding:write`, `onboarding:approve`, `organizations:*`.

---

## 2. Operational application (end user)

Public **fleet operations** for customers and household users — the product most people mean by “Chargers app.”

### Access

| Item | Value |
|------|-------|
| **Primary UI** | [chargers.deviceniq.com](https://chargers.deviceniq.com) |
| **Edge** | ALB 3 + CloudFront (`prod-public-alb`) |
| **Auth** | Cognito JWT via [auth.api.chargers.deviceniq.com](https://auth.api.chargers.deviceniq.com) |
| **Gate** | Only **`commissioned+`** chargers for the user’s org (org filter — Phase 2 of plan 013) |

No VPN required.

### End-user microapps

Each microapp is a separate SPA under `000-microapps/` with its own hostname and `deploy-microapps` CI job.

| Microapp | Hostname | Audience | Purpose | Status |
|----------|----------|----------|---------|--------|
| **chargers** | [chargers.deviceniq.com](https://chargers.deviceniq.com) | admin, operator, viewer | Fleet list, charger detail, status | **Shipped** |
| **sessions** | [sessions.app.chargers.deviceniq.com](https://sessions.app.chargers.deviceniq.com) | admin, operator, viewer | Session history, **duration & energy ML cards** | **Shipped** |
| **chat** | `chat.app.chargers.deviceniq.com` | admin, operator, viewer | Role-aware **AI assistant** | **Shipped** |
| **support** | `support.app.chargers.deviceniq.com` | All authenticated | Privacy, legal, static help | **Shipped** |

Shared shell: `000-microapps/00-libraries/shared/` (`MicroAppShell`, nav, auth panel).

### Backend APIs (operational)

| Service | URL | Purpose |
|---------|-----|---------|
| **auth** | `https://auth.api.chargers.deviceniq.com` | Login, `/auth/me`, JWT refresh |
| **chargers** | `https://chargers.api.chargers.deviceniq.com` | Fleet CRUD, charger detail |
| **sessions** | `https://sessions.api.chargers.deviceniq.com` | Session history |
| **session-predict** | `https://session-predict.api.chargers.deviceniq.com` | Duration ML + baseline |
| **energy-predict** | `https://energy-predict.api.chargers.deviceniq.com` | kWh delivered ML |
| **chat-assistant** | `https://chat-assistant.api.chargers.deviceniq.com` | LLM Q&A with RBAC tools |

### End-user roles (Cognito)

| Role | Segment | Typical user | Can use |
|------|---------|--------------|---------|
| **`chargers-admin`** | Fleet / platform owner | RamaKrishna | All operational microapps + user admin; future link to onboarding (VPN) |
| **`chargers-operator`** | Operations staff | Saritha | Fleet + sessions + chat (read/write) |
| **`chargers-viewer`** | Read-only stakeholder | Raman, Veda | Fleet + sessions (read-only) |
| **`manchana-family`** | Household org label | All family accounts | Grouping only — pair with a `chargers-*` role |

Details: [roles.md](roles.md) · [users.md](users.md) · [assignments.md](assignments.md)

### Operational capabilities

| Capability | Status | Notes |
|------------|--------|-------|
| Fleet list & charger detail | Shipped | Today: global demo fleet |
| Session history & ML cards | Shipped | Baseline + MLflow models |
| Chat assistant | Shipped | Onboarding tools blocked for public roles |
| DataOps / lake export | Shipped | Session analytics pipeline |
| Org-scoped fleet | Planned | Post-commission filter per org |
| Live status from OCPP | Partial | Legacy stack; bridge in progress |
| Remote start/stop | Not yet | Needs OCPP command path |
| Billing / payments | Out of scope | Phase 5+ |

**Not in operational UI:** org approval queues, install jobs, document upload, commission workflow (those stay in **onboarding**).

---

## 3. Platform & engineering (supporting)

Not customer-facing product applications — used to run and test the stack.

| Tool | URL | Purpose |
|------|-----|---------|
| **OCPP simulator** | [simulator.deviceniq.com](https://simulator.deviceniq.com) | QA / demo charge points |
| **OCPP runtime** | EKS (`Prod2-OCPP`, `ng-ocpp-adapter`, DynamoDB) | Live WebSocket sessions — **existing platform, do not rebuild** |
| **MLflow** | VPN tool host | Model registry for session/energy predict |

---

## 4. Journey: onboarding → operational

```text
ONBOARDING (VPN · internal ops)
  Org:    draft → submitted → approved → active
  Site:   draft → active
  Charger: draft → submitted → approved → COMMISSIONED
                              │
                              ▼
OPERATIONAL (internet · end users)
  available → charging / offline → sessions → ML → chat
                              │
                              ▼
OCPP RUNTIME (existing platform)
  WebSocket · live transactions · fault processing
```

| Charger status | Visible in operational app? |
|----------------|----------------------------|
| `draft`, `submitted`, `approved` | **No** |
| **`commissioned`** and above | **Yes** |
| `decommissioned` | **No** |

Full state machines: [lifecycles.md](lifecycles.md)

---

## 5. Comparison at a glance

| Dimension | Onboarding | Operational |
|-----------|------------|-------------|
| **Users** | Internal DeviceNIQ ops | Customers, operators, viewers |
| **Network** | VPN + ALB 1 | Internet + ALB 3 |
| **Primary goal** | Provision & commission fleet | Run fleet, sessions, ML, chat |
| **Microapps** | `onboarding` | `chargers`, `sessions`, `chat`, `support` |
| **ML / AI** | Optional SLA analytics (later) | Session/energy predict + chat (**shipped**) |
| **ChargePoint equivalent** | Admin / staging / activation | Fleet dashboard + driver ops |

---

## Related docs

| Doc | Purpose |
|-----|---------|
| [product-information.md](product-information.md) | vs ChargePoint, roadmap |
| [roles.md](roles.md) | Cognito + onboarding permissions |
| [platform-and-vendors.md](platform-and-vendors.md) | AWS, Cognito, OCPP vendors |
| [diagrams/system-landscape.md](diagrams/system-landscape.md) | Architecture diagram |
