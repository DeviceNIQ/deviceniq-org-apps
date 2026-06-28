# ChargePoint parity — DeviceNIQ Chargers

**Canonical scorecard (full tables):**  
[DeviceNIQ/deviceniq-cursor-workspace — `docs/chargepoint-parity.md`](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/docs/chargepoint-parity.md)

**Applications:** [applications.md](applications.md) · **Roles:** [roles.md](roles.md) · **Product summary:** [product-information.md](product-information.md)

---

## Onboarding app vs ChargePoint

| Capability | ChargePoint | DeviceNIQ | Status |
|------------|-------------|-----------|--------|
| Account / org | ✓ | `organizations` | MVP shipped |
| Sites | ✓ | `sites` | MVP shipped |
| Station registry | ✓ | `chargers` | MVP shipped |
| Connectors, models, contacts | ✓ | Ph 2 tables | Planned |
| Install jobs + documents | ✓ | Ph 2 | Planned |
| Commission → OCPP | ✓ built-in | IoT + simulator bridge | Shipped |
| Tariffs, IdTags | ✓ | Ph 3 | Planned |
| Payments | ✓ | — | N/A v1 |

Repo: [deviceniq-chargers-onboarding](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding) · VPN: `onboarding.chargers.deviceniq.com`

---

## Operational app vs ChargePoint

| Capability | ChargePoint | DeviceNIQ | Status |
|------------|-------------|-----------|--------|
| Fleet list | ✓ | chargers microapp | **Shipped** |
| Sessions | ✓ | sessions microapp | **Shipped** |
| AI assistant | Rare | chat microapp | **Differentiator** |
| Session / energy ML | Add-on | predict services | **Differentiator** |
| Live CP connection UI | ✓ | Backend only | RC-002 backlog |
| Remote Start/Stop | ✓ | — | RC-001 backlog |
| IdTag in UI | ✓ | API only | RC-003 backlog |
| Firmware / diagnostics UI | ✓ | Telemetry only | RC-004 backlog |
| Reports console | ✓ | DataOps batch | Partial |
| Roaming / payments | ✓ | — | N/A v1 |

URLs: [chargers.deviceniq.com](https://chargers.deviceniq.com) · [sessions.app.chargers.deviceniq.com](https://sessions.app.chargers.deviceniq.com)

---

## CSMS vs ChargePoint cloud

| Layer | ChargePoint | DeviceNIQ |
|-------|-------------|-----------|
| OCPP WSS | CP cloud | `wss://ocpp.chargers.deviceniq.com/{CP-*}` |
| Ops DB sync | Built-in | EventBridge → chargers API |
| Remote commands | Built-in | RC-001 (planned) |

Detail: [018 OCPP sync landscape](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/docs/018-ocpp-sync-landscape.md)

---

## Positioning

| Match ChargePoint | Beat ChargePoint |
|-----------------|------------------|
| Entity model, commissioning, fleet visibility (019 RC features) | ML on sessions, DataOps lake, AI assistant, card-first UX |
