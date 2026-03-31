# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
This project uses [Semantic Versioning](https://semver.org/).

---

## [v1.0.0-beta] — 2026

> ⚠️ Beta — not yet fully tested in production. API may change.

### Added

**Reusable Workflows**
- `owasp-baseline-reusable.yml` — ZAP Baseline passive DAST scan with preflight connectivity check
- `owasp-active-reusable.yml` — ZAP Full Scan and ZAP API Scan (active DAST) with approval gate
- `create-deployment.yml` — Creates GitHub Deployment record for automatic scan triggering
- `update-deployment.yml` — Reports deployment success/failure with environment URL for ZAP targeting

**Examples**
- `basic-web-app` — Minimal baseline scan setup for a public web app
- `api-app` — Baseline + active scan for a web app with REST API
- `deploy-integration` — Full GitHub Actions deploy pipeline with automatic ZAP triggering
- `ado-integration` — Azure DevOps pipeline with GitHub deployment reporting for automatic ZAP triggering

**ADO Templates**
- `create-deployment.yml` — ADO reusable template to create GitHub Deployment record
- `update-deployment.yml` — ADO reusable template to report deployment success/failure

**Documentation**
- README with quick start, architecture diagrams, and workflow reference
- SETUP.md with step-by-step instructions for all three integration paths
- CONTRIBUTING.md
- SECURITY.md

### Notes
- Passive baseline scan is safe for all environments — no attack payloads sent
- Active scans require a `security-approved` GitHub Environment with required reviewers
- Works with both GitHub Actions and Azure DevOps deployment pipelines
- Self-hosted runners supported for internal/intranet web apps
- Scan results appear in GitHub (Issues, Artifacts, Security tab) — ADO results
  integration (work items, pipeline artifacts) is planned for v1.1. See [ROADMAP.md](ROADMAP.md).
