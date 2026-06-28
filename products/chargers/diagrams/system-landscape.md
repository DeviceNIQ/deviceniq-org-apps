# System landscape — Chargers product

Three runtime bands: **onboarding (VPN)**, **operational (internet)**, **OCPP platform (existing)**.

```mermaid
flowchart TB
  subgraph users ["Users"]
    ops[Internal ops\nVPN]
    pub[Operators / viewers / drivers\nInternet]
    cp[Charge points\nOCPP WebSocket]
  end

  subgraph edge ["Edge"]
    cf[CloudFront\nchargers.deviceniq.com]
    alb1[ALB 1 prod-shared-alb\nonboarding.*]
    alb3[ALB 3 prod-public-alb\n*.api.chargers.*]
  end

  subgraph eks ["EKS namespace chargers"]
    ob_ui[onboarding microapp]
    ob_api[onboarding + organizations APIs]
    pub_ui[chargers · sessions · chat microapps]
    pub_api[chargers · sessions · predict · chat · auth]
    bridge[chargers-ocpp-bridge\nplanned]
  end

  subgraph data ["Data"]
    rds[(RDS prod-ams\nschema chargers)]
    lake[(S3 deviceniq-datalake)]
    cognito[AWS Cognito]
  end

  subgraph ocpp ["OCPP platform — existing"]
    ws[Prod2-OCPP / steve pipeline]
    ddb[(DynamoDB OCPP)]
    adapter[ng-ocpp-adapter]
    live[live-charging-session]
  end

  ops --> alb1 --> ob_ui --> ob_api --> rds
  pub --> cf --> pub_ui
  pub --> alb3 --> pub_api --> rds
  pub_api --> cognito
  ob_api --> cognito
  pub_api --> lake
  bridge --> adapter
  bridge --> rds
  cp --> ws --> ddb
  adapter --> ddb
  adapter --> live
  ob_api -.->|commission| bridge
  pub_api -.->|status sync| bridge
```

## Deployment matrix

| Component | Hostname pattern | ALB | Auth |
|-----------|------------------|-----|------|
| Onboarding UI | `onboarding.chargers.deviceniq.com` | 1 | Cognito + VPN |
| Onboarding API | `onboarding.api.chargers.deviceniq.com` | 1 | JWT + onboarding perms |
| Public UI | `chargers.deviceniq.com`, `*.app.chargers.*` | CF + 3 | Cognito |
| Public API | `*.api.chargers.deviceniq.com` | 3 | Cognito JWT |

## Repo

Single monorepo **`deviceniq-chargers`** — path-filtered CI deploys changed modules only.

See [product-information.md](../product-information.md) | [platform-and-vendors.md](../platform-and-vendors.md)
