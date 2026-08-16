# GitHub Setup

## 1. Repository Settings

In GitHub, configure the repository with these branches:

- `main`
- `dev`
- `qat`
- `preprod`

## 2. Branch Protection

For `dev`, `qat`, `preprod`, and `main`, enable:

- Require a pull request before merging
- Require status checks to pass before merging
- Restrict direct pushes if possible

Recommended required check for each branch:

- `Validate Salesforce`

For full automation, do not require approvals on these branches.

## 3. Environment

Create these GitHub environments:

- `dev`
- `qat`
- `preprod`
- `production`

Do not require reviewers on these environments if you want hands-off promotion.

## 4. Secrets

- `SALESFORCE_DEV_AUTH_URL`
- `SALESFORCE_QAT_AUTH_URL`
- `SALESFORCE_PREPROD_AUTH_URL`
- `SALESFORCE_PROD_AUTH_URL`

## 5. Authentication Secret Format

Generate the auth URL from a machine where you are logged into each org:

```bash
sf org display --target-org <your-org-alias> --verbose --json | jq -r '.result.sfdxAuthUrl'
```

Copy the one-line result into the matching GitHub secret.

## 6. Promotion Flow

- feature branches -> `dev`
- push to `dev` deploys to `DEV`, then auto-merges to `qat`
- push to `qat` deploys to `QAT`, then auto-merges to `preprod`
- push to `preprod` deploys to `PREPROD`, then auto-merges to `main`
- push to `main` deploys to Production
