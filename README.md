# Salesforce CI/CD

This repository contains the Salesforce metadata and GitHub Actions workflows for a Salesforce promotion pipeline.

## Branch Strategy

- `dev`: development sandbox deployment branch
- `qat`: QAT sandbox deployment branch
- `preprod`: pre-production sandbox deployment branch
- `main`: production-ready code and production deployments

## Current Flow

1. Build and test changes in the `DEV` sandbox.
2. Commit metadata changes and merge them into `dev`.
3. A push to `dev` deploys automatically to the `DEV` sandbox.
4. If `DEV` deployment succeeds, GitHub merges `dev` into `qat`.
5. The push to `qat` deploys automatically to the `QAT` sandbox.
6. If `QAT` deployment succeeds, GitHub merges `qat` into `preprod`.
7. The push to `preprod` deploys automatically to the `PREPROD` sandbox.
8. If `PREPROD` deployment succeeds, GitHub merges `preprod` into `main`.
9. The push to `main` deploys automatically to Production.

## GitHub Secrets

Add these repository secrets before running validations or deployments:

- `SALESFORCE_DEV_AUTH_URL`
- `SALESFORCE_QAT_AUTH_URL`
- `SALESFORCE_PREPROD_AUTH_URL`
- `SALESFORCE_PROD_AUTH_URL`

## GitHub Environments

Create these GitHub environments:

- `dev`
- `qat`
- `preprod`
- `production`

For full automation, keep these environments without manual approval gates.

## Workflows

- `.github/workflows/validate-salesforce.yml`: lightweight pull request check for repo changes
- `.github/workflows/deploy-salesforce.yml`: deploys to the target org after a push into `dev`, `qat`, `preprod`, or `main`, then promotes forward automatically
