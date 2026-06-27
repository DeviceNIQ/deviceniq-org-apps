# AWS — roles

## Entra enterprise app: AWS IAM Identity Center

| App role ID | Name (typical) | Description |
|-------------|----------------|-------------|
| `8774f594-1d59-4279-b9d9-59ef09a23530` | *(AWS-defined)* | Assigned to RamaKrishna Manchana |

AWS permission sets are managed **inside AWS IAM Identity Center**, not in Entra app roles. Entra assignment grants SSO access only.

## AWS IAM (post-SSO)

| Permission set | Accounts | Target users |
|----------------|----------|--------------|
| `production-admin` | 964680330571 | RamaKrishna |
| `sandbox-admin` | sandbox account | Engineers |
| `root-readonly` | org management | Auditors |

Document actual permission sets in AWS console export.
