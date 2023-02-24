# run-playwright-on-vercel-deployment-action

Examples to use:


On successful Vercel deployment

```yml
name: 🎭 Playwright e2e
on:
  deployment_status:
jobs:
  e2e-test:
    runs-on: ubuntu-latest
    timeout-minutes: 60
    if: ${{ github.event.deployment_status.state == 'success' }}
    steps:
      # branch name is not available as github.ref
      - name: Extract branch name
        id: extract_branch_name
        run: echo "::set-output name=branch_name::$(echo ${GITHUB_REF#refs/heads/})"
      - name: Run Playwright tests on master
        if: steps.extract_branch_name.outputs.branch_name == 'main'
        uses: gorazdo/run-playwright-on-vercel-deployment-action@master
        with:
          baseUrl: ${{ PRODUCTION_DOMAIN_NAME }}
      - name: Run Playwright tests
        if: steps.extract_branch_name.outputs.branch_name != 'main'
        uses: gorazdo/run-playwright-on-vercel-deployment-action@master
        with:
          baseUrl: ${{ github.event.deployment_status.target_url }}
```

Manual run

```yml
name: 🎭 Playwright. Manual run ▶

on:
  workflow_dispatch:
    inputs:
      baseUrl:
        description: 'Website URL'
        default: ''

jobs:
  e2e-test:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
      - name: Run Playwright tests
        uses: gorazdo/run-playwright-on-vercel-deployment-action@master
        with:
          baseUrl: ${{ github.event.inputs.baseUrl }}

```