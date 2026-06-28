# DeviceNIQ Chargers (custom application)

**Folder:** `products/chargers/`  
**Type:** Custom-built EV charging platform (monorepo + platform OCPP stack)  
**GitHub:** [DeviceNIQ/deviceniq-chargers](https://github.com/DeviceNIQ/deviceniq-chargers)  
**My Apps tile (target):** DeviceNIQ Chargers — homepage → this README

## One-line pitch

ChargePoint-class **fleet onboarding, commissioning, and operations** — with **session/energy ML**, **DataOps lake**, and **role-aware AI chat** that most CSMS vendors treat as afterthoughts.

## Surfaces

| Application | URL | Edge | Status |
|-------------|-----|------|--------|
| **Operational** (public) | [chargers.deviceniq.com](https://chargers.deviceniq.com) | ALB 3 + CloudFront | **Shipped** |
| Sessions UI | [sessions.app.chargers.deviceniq.com](https://sessions.app.chargers.deviceniq.com) | ALB 3 | **Shipped** |
| **Onboarding** (internal) | `onboarding.app.chargers.deviceniq.com` | ALB 1 (VPN) | **Planned** ([013](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/013-chargers-internal-onboarding-portal.md)) |
| OCPP simulator | [simulator.deviceniq.com](https://simulator.deviceniq.com) | Platform | **Exists** |

## APIs

| Service | URL |
|---------|-----|
| Auth | `https://auth.api.chargers.deviceniq.com` |
| Chargers | `https://chargers.api.chargers.deviceniq.com` |
| Sessions | `https://sessions.api.chargers.deviceniq.com` |

## Documentation map

| Doc | Purpose |
|-----|---------|
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
| [009 personas MCP](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/009-chargers-product-personas-mcp.md) | Cursor MCP for RBAC planning |

## MCP (Cursor)

`chargers-product` — `powershell -File scripts\setup-chargers-product-mcp.ps1` in `deviceniq-cursor-workspace`.

## Owners

| Role | Person |
|------|--------|
| Product / platform | RamaKrishna Manchana |
| Engineering | DeviceNIQ chargers team |
