# Chargers — entity lifecycles

Each entity has a **lifecycle status** (and optional workflow fields). Transitions emit **`onboarding_events`** for onboarding entities. Operational states for chargers are driven by **OCPP** after commission.

---

## 1. Organization lifecycle

**Actor:** `internal_ops` (create/approve), `platform_admin` (suspend)

```mermaid
stateDiagram-v2
  [*] --> draft : create
  draft --> submitted : submit
  submitted --> approved : approve
  submitted --> draft : reject
  approved --> active : activate
  active --> suspended : suspend
  suspended --> active : reinstate
  active --> [*]
```

| Status | Public app | ChargePoint equivalent |
|--------|------------|------------------------|
| `draft` | Hidden | Prospect / staging account |
| `submitted` | Hidden | Pending approval |
| `approved` | Hidden | Approved, not billing-active |
| `active` | Tenant visible | Active account |
| `suspended` | Hidden | Suspended account |

**Associations active when `active`:** sites, commissioned chargers, org users, tariffs.

---

## 2. Site lifecycle

**Actor:** `internal_ops`

```mermaid
stateDiagram-v2
  [*] --> draft : create under org
  draft --> active : activate
  active --> inactive : deactivate
  inactive --> active : reactivate
```

Sites must belong to an **`active`** (or approving) organization. Chargers reference `site_id`.

---

## 3. Charger lifecycle (onboarding + operations)

**Actors:** `internal_ops` (through `commissioned`), `ocpp_server` / `ocpp_adapter` (runtime states)

```mermaid
stateDiagram-v2
  [*] --> draft : register
  draft --> submitted : submit
  submitted --> approved : approve
  submitted --> draft : reject
  approved --> commissioned : commission\n(checklist + network)
  commissioned --> available : OCPP online
  available --> charging : session start
  charging --> available : session stop
  available --> offline : fault / disconnect
  offline --> available : recover
  commissioned --> decommissioned : retire
  available --> decommissioned : retire
  offline --> decommissioned : retire
  decommissioned --> [*]
```

| Status | Actor | Public UI | OCPP |
|--------|-------|-----------|------|
| `draft` … `approved` | internal_ops | ✗ | Not registered |
| **`commissioned`** | internal_ops | **✓ appears** | Register CP (Ph 5–7) |
| `available` | System | ✓ | Connected, idle |
| `charging` | System | ✓ | Transaction active |
| `offline` | System | ✓ | Unreachable / fault |
| `decommissioned` | internal_ops | ✗ | Removed |

**Commission prerequisites (target):** org `active`, site `active`, checklist complete, `network_profiles` set, optional install job `completed`.

---

## 4. Connector lifecycle

**Actor:** `internal_ops` (onboarding), `charge_point` (runtime)

```mermaid
stateDiagram-v2
  [*] --> draft : add port
  draft --> available : charger commissioned
  available --> faulted : OCPP fault
  faulted --> available : clear fault
  available --> unavailable : maintenance
  unavailable --> available : restore
```

Each **connector** maps to OCPP `connectorId` via `ocpp_connector_id`.

---

## 5. Installation job lifecycle

**Actors:** `internal_ops`, `installer_partner`

```mermaid
stateDiagram-v2
  [*] --> scheduled : create
  scheduled --> in_progress : start
  in_progress --> completed : complete
  in_progress --> failed : fail
  failed --> scheduled : reschedule
  completed --> [*]
```

Linked: `charger_id`, `organization_id`, `site_id`, optional `partner_id`.

---

## 6. Contact, document, network profile

| Entity | Lifecycle | Notes |
|--------|-----------|-------|
| **Contact** | Created → updated (no enum) | Typed: primary, billing, operations, legal |
| **Onboarding document** | Upload → immutable | S3 URI; tied to org/site/charger/job |
| **Network profile** | Draft at create → bound at commission | Drives OCPP CP identity |
| **Commissioning checklist item** | `completed` 0/1 per item | All required → commission allowed |
| **Charger model** | Catalog (no workflow) | Seed + admin CRUD |
| **Partner** | Active registry | Installer companies |
| **Tariff plan** | `draft` → `active` → `retired` | Assigned via `organization_tariffs` |
| **IdTag** | Issued → revoked | Phase 3; OCPP Authorize |

---

## 7. Session lifecycle (operational)

**Actors:** `field_operator`, `driver`, `charge_point`

```mermaid
stateDiagram-v2
  [*] --> pending : remote start / plug-in
  pending --> active : StartTransaction
  active --> completed : StopTransaction
  active --> faulted : error
  faulted --> completed : force stop
  completed --> [*]
```

Post-session: **session-predict** and **energy-predict** attach ML outputs; **dataops_exporter** lands rows in lake.

---

## 8. End-to-end journey (actors + entities)

```mermaid
sequenceDiagram
  participant Ops as internal_ops
  participant Onb as onboarding API
  participant DB as prod-ams.chargers
  participant OCPP as ocpp_adapter
  participant Pub as operational API
  participant Op as field_operator

  Ops->>Onb: Create organization (draft)
  Onb->>DB: organizations + events
  Ops->>Onb: Approve org → active
  Ops->>Onb: Create site + charger (draft)
  Ops->>Onb: Add connectors, network, install job
  Ops->>Onb: Commission charger
  Onb->>DB: lifecycle_status = commissioned
  Onb->>OCPP: Register CP (Phase 5+)
  OCPP->>DB: status available / charging
  Op->>Pub: List fleet (commissioned only)
  Op->>Pub: View sessions + ML predictions
```

---

## 9. Lifecycle vs ChargePoint station states

| DeviceNIQ `lifecycle_status` | ChargePoint station state (approx.) |
|-------------------------------|-------------------------------------|
| draft … approved | Unpublished / pending |
| commissioned | Activated, not yet online |
| available | Available / reachable |
| charging | In use |
| offline | Unreachable / fault |
| decommissioned | Deactivated / removed |

DeviceNIQ splits **business commission** from **telemetry-driven** states — clearer audit than a single “active” flag.
