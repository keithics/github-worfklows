# GitHub Workflows

This repository hosts reusable GitHub Actions workflows for frontend and backend services. Each workflow is designed to be called with `workflow_call` inputs from application repositories so teams can share consistent linting and deployment pipelines.

## Available workflows

- `.github/workflows/be-lint.yml` runs Java-oriented linting for Spring Boot projects, automatically detecting Maven or Gradle builds before executing Checkstyle.
- `.github/workflows/fe-lint.yml` runs `npm ci` and `npm run lint` to validate JavaScript or TypeScript projects.
- `.github/workflows/be-deploy.yml` performs backend deployment to Azure using OIDC authentication, optional branch normalization helpers, and secret loading from Key Vault.
- `.github/workflows/fe-deploy.yml` builds and deploys React frontends to Azure, supporting branch-aware environment derivation and Cloudflare credentials.
- `.github/workflows/gateway.yml` routes `workflow_call` inputs to the appropriate backend or frontend deployment workflow by resolving secret names based on the branch.

## Calling a workflow

Reference the workflow by path and pin a specific ref in your application repository. Provide required inputs and secrets that match the workflow you need.

```yaml
name: Deploy via reusable workflow

on:
  push:
    branches: ["main"]

jobs:
  deploy:
    uses: your-org/github-worfklows/.github/workflows/gateway.yml@main
    with:
      project: my-project
      branch_name: main
```

Secrets referenced in the reusable workflows must be present in the calling repository or organization. Review the corresponding workflow file to identify required secrets before adoption.
