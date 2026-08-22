# Origami Studio (family site)

**Folder:** `products/origami/`  
**Type:** Static site + serverless API (students, classes, tutors)  
**GitHub:** [DeviceNIQ/origami-manchana](https://github.com/DeviceNIQ/origami-manchana)  
**Production:** [origami.manchana.online](https://origami.manchana.online)  
**Staging:** [staging.origami.manchana.online](https://staging.origami.manchana.online)

## Branches

| Branch | Environment | Deploy |
|--------|-------------|--------|
| `prod` | production | S3 + CloudFront + Lambda API |
| `dev` | staging | same stack, staging buckets |

## AWS

- Account: `964680330571` (production-admin SSO)
- Site buckets: `origami.manchana.online`, `staging.origami.manchana.online`
- Lambda: `origami-api`
- DynamoDB: `origami-students`, `origami-enrollments`, `origami-tutors`, `origami-support`

## Local

```powershell
cd origami-manchana
npx serve 000-microapps/studio
```

Deploy: `004-infrastructure/frontend/sync-local.ps1 -Target production -IncludeApi`
