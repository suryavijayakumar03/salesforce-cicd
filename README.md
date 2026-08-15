# Salesforce CI/CD

This repository contains the Salesforce metadata for a Production-first CI/CD setup.

## Branch Strategy

- `main`: production-ready code and production deployments
- `dev`: development sandbox promotion branch
- `qat`: QAT sandbox promotion branch
- `preprod`: pre-production sandbox promotion branch

Until the non-production sandboxes are ready, only the `main` production flow is active.

## Current Flow

1. Create a feature branch from `main`.
2. Commit Salesforce metadata changes.
3. Open a pull request into `main`.
4. GitHub Actions validates the deployment against Production.
5. After approval, a production deployment can be triggered.

## GitHub Secrets

Add these repository secrets before running deployments:

- `SALESFORCE_PROD_AUTH_URL`

Later, when the sandboxes are ready, also add:

- `SALESFORCE_DEV_AUTH_URL`
- `SALESFORCE_QAT_AUTH_URL`
- `SALESFORCE_PREPROD_AUTH_URL`

## GitHub Environments

Create a GitHub environment named `production` and require manual approval for deployments.

## Workflows

- `.github/workflows/validate-production.yml`: validates pull requests targeting `main`
- `.github/workflows/deploy-production.yml`: deploys to Production after a push to `main` or a manual trigger
