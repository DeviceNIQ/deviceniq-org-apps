# DeviceNIQ — Chargers internal onboarding

**GitHub:** [DeviceNIQ/deviceniq-chargers-onboarding](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding)

Internal VPN portal for customer/site/charger commissioning. Separate repo from operational app; **shared Cognito** credentials.

| Surface | URL |
|---------|-----|
| UI | `https://onboarding.chargers.deviceniq.com` (ALB 1, VPN) |
| API | `https://onboarding.api.chargers.deviceniq.com` |
| Auth (shared) | `https://auth.api.chargers.deviceniq.com` |

## vs ChargePoint — three shipped differentiators

1. **Unified audit timeline** — every state change on org/site/charger in one `onboarding_events` stream (compliance-friendly; ChargePoint-class UIs scatter history).
2. **Smart approval queues** — KPI dashboard + Customers \| Chargers tabs with pending counts and one-click approve/commission.
3. **Commission readiness gate** — checklist blocks go-live until customer approved, site, serial, connector, and charger approval are complete.

## Users

Only **`chargers-admin`** (e.g. RamaKrishna) — same password as operational app. Operators/viewers use [chargers.deviceniq.com](https://chargers.deviceniq.com) only. See [USERS-AND-ROLES.md](https://github.com/DeviceNIQ/deviceniq-chargers-onboarding/blob/main/docs/USERS-AND-ROLES.md).

## Related

- [applications.md](applications.md) — full app landscape
- [013 implementation plan](https://github.com/DeviceNIQ/deviceniq-cursor-workspace/blob/main/implementationPlans/013-chargers-internal-onboarding-portal.md)
