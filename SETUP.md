# Setup Guide

Step-by-step instructions to get scanning in under 10 minutes.

---

## Prerequisites

- A GitHub repo with a web app or API that has a running environment
- GitHub Actions enabled on your repo (on by default)
- Docker on your runner if using self-hosted (GitHub-hosted runners include Docker)

---

## Option 1 — Manual Trigger (Quickest)

Good for testing the workflow works before wiring up automation.

**1. Create the workflow file**

In your app repo, create `.github/workflows/owasp-baseline-scan.yml`:

```yaml
name: OWASP Baseline Scan

on:
  workflow_dispatch:
    inputs:
      target_url:
        description: 'URL to scan'
        required: true
        type: string
        default: 'http://testphp.vulnweb.com'
      environment:
        description: 'Environment'
        required: true
        type: choice
        options: [dev, staging, uat]

jobs:
  scan:
    uses: ComputerNiagara/github-owasp-templates/.github/workflows/owasp-baseline-reusable.yml@main
    with:
      target_url: ${{ github.event.inputs.target_url }}
      environment: ${{ github.event.inputs.environment }}
```

If using a self-hosted runner for internal apps, replace `ComputerNiagara/github-owasp-templates` with your fork path.

**2. Run it**

Actions → OWASP Baseline Scan → Run workflow → enter a URL → Run

**3. Check results**

- Issues tab → new issue with findings
- Workflow run → Artifacts → HTML report

---

## Option 2 — Automatic After Deployment

Scans fire automatically whenever your app deploys successfully.

**1. Add baseline scan workflow** (same as Option 1 above, with `deployment_status` trigger)

Use the [basic-web-app example](examples/basic-web-app/.github/workflows/owasp-baseline-scan.yml).

**2. Add deployment reporting to your deploy workflow**

```yaml
# In your existing deploy workflow
permissions:
  contents: read
  deployments: write

steps:
  - name: Create Deployment
    id: deployment
    uses: ComputerNiagara/github-owasp-templates/actions/create-deployment@main
    with:
      environment: dev
      token: ${{ secrets.GITHUB_TOKEN }}

  # --- your existing deploy steps ---

  - name: Report Success
    if: success()
    uses: ComputerNiagara/github-owasp-templates/actions/update-deployment@main
    with:
      deployment_id: ${{ steps.deployment.outputs.deployment_id }}
      environment: dev
      status: success
      environment_url: 'https://your-app-dev.yourcompany.com'
      token: ${{ secrets.GITHUB_TOKEN }}

  - name: Report Failure
    if: failure()
    uses: ComputerNiagara/github-owasp-templates/actions/update-deployment@main
    with:
      deployment_id: ${{ steps.deployment.outputs.deployment_id }}
      environment: dev
      status: failure
      token: ${{ secrets.GITHUB_TOKEN }}
```

See the [deploy-integration example](examples/deploy-integration/) for the full file.

**3. Deploy**

Push a change. The deploy runs, reports success, and the baseline scan fires automatically.

---

## Option 3 — Active Scan (Approval Gated)

For deep security testing with real attack payloads.

**1. Set up the approval environment**

In your app repo:
```
Settings → Environments → New environment → Name: security-approved
→ Required reviewers → add yourself or your security team
```

**2. Add the active scan workflow**

Use the [api-app example](examples/api-app/.github/workflows/owasp-active-scan.yml).
If using self-hosted runners, replace `ComputerNiagara/github-owasp-templates` with your fork path.

**3. Run it**

Actions → OWASP Active Scan → Run workflow → fill in the form → Run

The workflow pauses for approval. Go to the workflow run and click **Review deployments → Approve**.

⚠️ Only point this at an isolated environment with throwaway data.

---

## Enabling Reusable Workflow Access

For your app repo to call workflows in `github-owasp-templates`, the source repo must allow it:

```
github-owasp-templates repo → Settings → Actions → General → Access
→ Accessible from repositories owned by the user (or your org)
```

---

## Authenticated Scanning

If your app requires a token to access:

1. Add a secret to your app repo:
   ```
   Repo → Settings → Secrets → Actions → New secret
   Name: ZAP_AUTH_HEADER_VALUE
   Value: your-bearer-token
   ```

2. Pass it through in your workflow:
   ```yaml
   secrets:
     ZAP_AUTH_HEADER_VALUE: ${{ secrets.ZAP_AUTH_HEADER_VALUE }}
   ```

3. Uncomment the `cmd_options` block in `owasp-baseline-reusable.yml`

---

## Testing with a Safe Public Target

Don't have an environment ready? Use these intentionally vulnerable test sites:

| URL | What it is |
|-----|-----------|
| `http://testphp.vulnweb.com` | OWASP PHP test app — ZAP's default demo target |
| `http://testfire.net` | IBM demo banking app |
| `https://juice-shop.herokuapp.com` | OWASP Juice Shop |

These are designed for security scanner testing — safe to use for both baseline and active scans.
