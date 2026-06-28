# Actor map — who touches what

Maps **human and system actors** to applications, entities, and lifecycle transitions.

## Human actors by application

```mermaid
flowchart LR
  subgraph onboarding ["Onboarding app — VPN"]
    io[internal_ops]
  end

  subgraph operational ["Operational app — internet"]
    adm[platform_admin]
    op[field_operator]
    vw[viewer]
    drv[driver]
  end

  subgraph entities ["Entities"]
    O[Organization]
    S[Site]
    C[Charger]
    X[Session]
    T[IdTag]
  end

  io --> O
  io --> S
  io --> C
  adm --> C
  adm --> X
  adm --> T
  op --> C
  op --> X
  vw --> C
  vw --> X
  drv --> T
  drv --> X
```

## Actor responsibility matrix

| Actor | Organization | Site | Charger onboard | Charger ops | Session | IdTag | Install job |
|-------|:------------:|:----:|:---------------:|:-----------:|:-------:|:-----:|:-----------:|
| **internal_ops** | CRUD, approve | CRUD | CRUD, commission | read | — | CRUD Ph3 | CRUD |
| **platform_admin** | read | read | read | CRUD | CRUD | assign | read |
| **field_operator** | — | read | read | CRUD | CRUD | read | — |
| **viewer** | — | read | read | read | read | — | — |
| **driver** | — | — | — | use CP | start/stop | present | — |
| **installer_partner** | — | — | — | — | — | — | update job |

## System actors

```mermaid
flowchart TB
  CP[charge_point] -->|OCPP| OS[ocpp_server]
  OS --> AD[ocpp_adapter]
  AD -->|sync status| CH[chargers API / DB]
  AD --> LS[live-charging-session]
  ML[ml_pipeline] -->|predictions| SE[sessions API]
  DO[dataops_exporter] -->|parquet| LAKE[S3 datalake]
  AI[chat_assistant] -->|tools| CH
  AI --> ON[onboarding API\nPhase 2]
```

| System actor | Triggers | Entities affected |
|--------------|----------|-------------------|
| **charge_point** | Plug-in, meter | Connector status, session |
| **ocpp_server** | WebSocket events | DynamoDB CP state |
| **ocpp_adapter** | Bridge jobs | `lifecycle_status`, sessions |
| **ml_pipeline** | Post-session / batch | Prediction fields on session |
| **dataops_exporter** | Scheduled | Lake tables |
| **chat_assistant** | User prompt | Read-only tools per RBAC |

## Cognito group → actor mapping

| Cognito group | Maps to actor | Application |
|---------------|---------------|-------------|
| `chargers-admin` + onboarding perms | internal_ops + platform_admin | Both |
| `chargers-operator` | field_operator | Operational |
| `chargers-viewer` | viewer | Operational |
| `manchana-family` | org grouping only | Pair with chargers-* role |
| `organization_users.role` | org_admin / org_operator / org_viewer | Tenant scope (Ph 4) |

See [roles.md](../roles.md) | [assignments.md](../assignments.md)
