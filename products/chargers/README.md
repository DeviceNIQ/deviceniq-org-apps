# DeviceNIQ Chargers (custom application)

**Folder:** `products/chargers/`  
**Type:** Custom-built EV charging platform (monorepo + platform OCPP stack)  
**GitHub:** [DeviceNIQ/deviceniq-chargers](https://github.com/DeviceNIQ/deviceniq-chargers) (operational) · [DeviceNIQ/deviceniq-chargers-onboarding](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding) (internal onboarding)  
**My Apps tile (target):** DeviceNIQ Chargers — homepage → this README

## One-line pitch

ChargePoint-class **fleet onboarding, commissioning, and operations** — with **session/energy ML**, **DataOps lake**, and **role-aware AI chat** that most CSMS vendors treat as afterthoughts.

## Applications

Chargers is a **suite** of applications — not a single UI:

| Class | Apps | Doc |
|-------|------|-----|
| **DeviceNIQ Chargers Onboarding** (internal, VPN) | `onboarding` microapp + API | [applications.md](applications.md) §1 |
| **Operational** (public, end user) | `chargers`, `sessions`, `chat`, `support` | [applications.md](applications.md) §2 |
| **Platform** | OCPP simulator, runtime stack | [applications.md](applications.md) §3 |

## Surfaces (quick reference)

| Application | URL | Edge | Status |
|-------------|-----|------|--------|
| **Operational — fleet** | [chargers.deviceniq.com](https://chargers.deviceniq.com) | ALB 3 + CloudFront | **Shipped** |
| **Operational — sessions** | [sessions.app.chargers.deviceniq.com](https://sessions.app.chargers.deviceniq.com) | ALB 3 | **Shipped** |
| **Operational — chat** | `chat.app.chargers.deviceniq.com` | ALB 3 | **Shipped** |
| **Operational — support** | `support.app.chargers.deviceniq.com` | ALB 3 | **Shipped** |
| **DeviceNIQ Chargers Onboarding** (internal) | `onboarding.chargers.deviceniq.com` | ALB 1 (VPN) | **MVP** — [deviceniq-chargers-onboarding](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding) |
| OCPP simulator | [simulator.chargers.deviceniq.com](https://simulator.chargers.deviceniq.com) | Platform | **Exists** |
| SteVe CSMS | [steve.chargers.deviceniq.com](https://steve.chargers.deviceniq.com) | Platform | **Exists** |

Full breakdown: **[applications.md](applications.md)**

## APIs

| Service | URL |
|---------|-----|
| Auth | `https://auth.api.chargers.deviceniq.com` |
| Chargers | `https://chargers.api.chargers.deviceniq.com` |
| Sessions | `https://sessions.api.chargers.deviceniq.com` |

## Documentation map

| Doc | Purpose |
|-----|---------|
| [applications.md](applications.md) | **Onboarding vs operational apps** — URLs, modules, roles, handoff |
| [onboarding-repo.md](onboarding-repo.md) | Internal onboarding repo, features, user matrix |
| [product-information.md](product-information.md) | Capabilities vs ChargePoint, roadmap, differentiation |
| [actors-and-entities.md](actors-and-entities.md) | Actors, domain entities, associations |
| [users.md](users.md) | Cognito users, types, lifecycle |
| [roles.md](roles.md) | Cognito + org RBAC |
| [assignments.md](assignments.md) | User → group → permission matrix |
| [lifecycles.md](lifecycles.md) | State machines per entity |
| [platform-and-vendors.md](platform-and-vendors.md) | Purchased cloud services + platform stack we build on |
| [diagrams/](diagrams/README.md) | Architecture and ER diagrams |

## Engineering references (cursor-workspace)

| Plan | Topic |
|------|-------|
| [014 onboarding → operational](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/014-chargers-onboarding-to-operational-landscape.md) | Two apps + OCPP landscape |
| [013 internal onboarding](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/013-chargers-internal-onboarding-portal.md) | ChargePoint-class entities + schema |
| [chargepoint-parity.md](chargepoint-parity.md) | **ChargePoint parity** — onboarding + operational + CSMS |
| [019 CP roadmap](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/019-operational-chargepoint-roadmap.md) | RC-001…RC-005 operational ↔ charge point |
| [chargepoint-parity](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/docs/chargepoint-parity.md) | **Master** ChargePoint scorecard (onboarding + operational) |
| [019 CP roadmap](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/019-operational-chargepoint-roadmap.md) | RC-001…RC-005 vs ChargePoint |
| [009 personas MCP](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/009-chargers-product-personas-mcp.md) | Cursor MCP for RBAC planning |

## MCP (Cursor)

`chargers-product` — `powershell -File scripts\setup-chargers-product-mcp.ps1` in `deviceniq-cursor-workspace`.

## Owners

| Role | Person |
|------|--------|
| Product / platform | RamaKrishna Manchana |
| Engineering | DeviceNIQ chargers team |
