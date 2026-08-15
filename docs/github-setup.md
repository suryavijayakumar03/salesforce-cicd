# GitHub Setup

## 1. Repository Settings

In GitHub, configure the repository with these branches:

- `main`
- `dev`
- `qat`
- `preprod`

Only `main` is required right now.

## 2. Branch Protection

For `main`, enable:

- Require a pull request before merging
- Require at least one approval
- Require status checks to pass before merging
- Restrict direct pushes if possible

Recommended required check:

- `Validate Production`

## 3. Environment

Create a GitHub environment named `production` and enable required reviewers for deployment approval.

## 4. Secrets

Add this repository secret now:

- `SALESFORCE_PROD_AUTH_URL`

Later add:

- `SALESFORCE_DEV_AUTH_URL`
- `SALESFORCE_QAT_AUTH_URL`
- `SALESFORCE_PREPROD_AUTH_URL`

## 5. Authentication Secret Format

Generate the auth URL from a machine where you are logged into the production org:

```bash
sf org display --target-org <your-production-alias> --verbose
```

Copy the `Sfdx Auth Url` value into the `SALESFORCE_PROD_AUTH_URL` secret.

## 6. Promotion Plan Later

When the sandboxes are ready, we can add:

- PR validation on `dev`, `qat`, and `preprod`
- deployment workflows for each sandbox
- promotion rules from `dev` -> `qat` -> `preprod` -> `main`
