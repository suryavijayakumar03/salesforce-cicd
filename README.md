# Salesforce CI/CD

This repository contains the Salesforce metadata and GitHub Actions workflows for a Salesforce promotion pipeline.

## Branch Strategy

- `dev`: development sandbox deployment branch
- `qat`: QAT sandbox deployment branch
- `preprod`: pre-production sandbox deployment branch
- `main`: production-ready code and production deployments

## Current Flow

1. Build and test changes in the `DEV` sandbox.
2. Commit metadata changes and open a pull request into `dev`.
3. GitHub validates the pull request against the `DEV` sandbox.
4. After merge, GitHub deploys the same code to the `DEV` sandbox.
5. Promote with pull requests from `dev` -> `qat` -> `preprod` -> `main`.
6. Each pull request validates against its target org.
7. Each merge deploys to its matching target org.
8. `main` remains the production approval and deployment branch.

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

Recommended approvals:

- `dev`: no approval gate
- `qat`: optional approval gate
- `preprod`: manual approval recommended
- `production`: manual approval required

## Workflows

- `.github/workflows/validate-salesforce.yml`: validates pull requests targeting `dev`, `qat`, `preprod`, or `main`
- `.github/workflows/deploy-salesforce.yml`: deploys to the target org after a merge into `dev`, `qat`, `preprod`, or `main`
