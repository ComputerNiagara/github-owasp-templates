# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
This project uses [Semantic Versioning](https://semver.org/).

---

## [Unreleased] — main

### Changed
- `create-deployment` and `update-deployment` refactored from reusable workflows
  to composite actions — resolves `github.repository` context issue when called
  cross-repo, eliminates extra runner spin-up, simpler pipeline graph
- All `@v1` references updated to `@main` — stable tag not yet created

### Fixed
- ZAP workspace `PermissionError` on `/zap/wrk/zap.yaml` — added `chmod 777` step
- Artifact upload 400 error — updated to `zaproxy/action-baseline@v0.15.0`,
  `action-full-scan@v0.13.0`, `action-api-scan@v0.10.0`
- `actions/checkout@v4` Node.js 20 deprecation — updated to `actions/checkout@v6`
- `github.repository` in deployment workflows resolving to called repo instead of
  caller repo — fixed by moving to composite actions

---

## [v1.0.0-beta] — 2026

> ⚠️ POC — not yet fully tested in production.

### Added

**Reusable Workflows** (`.github/workflows/`)
- `owasp-baseline-reusable.yml` — ZAP Baseline passive DAST with preflight connectivity check
- `owasp-active-reusable.yml` — ZAP Full Scan + ZAP API Scan (active DAST) with approval gate

**Composite Actions** (`actions/`)
- `create-deployment/` — creates GitHub Deployment record in the caller's repo context
- `update-deployment/` — reports deployment success/failure, sets environment URL for ZAP

**ADO Templates** (`ado-templates/`)
- `create-deployment.yml` — ADO reusable template to create GitHub Deployment record
- `update-deployment.yml` — ADO reusable template to report deployment success/failure

**Examples**
- `basic-web-app` — minimal baseline scan setup for a public web app
- `api-app` — baseline + active scan for a web app with REST API
- `deploy-integration` — GitHub Actions deploy pipeline with automatic ZAP triggering
- `ado-integration` — Azure DevOps pipeline with GitHub deployment reporting

**Documentation**
- README with quick start, architecture diagrams, and full reference
- SETUP.md with step-by-step instructions for all integration paths
- CONTRIBUTING.md, SECURITY.md, ROADMAP.md

### Architecture

| Component | Type | Purpose |
|-----------|------|---------|
| `owasp-baseline-reusable.yml` | Reusable workflow | ZAP scan — needs Docker, own runner |
| `owasp-active-reusable.yml` | Reusable workflow | ZAP scan — needs Docker, own runner |
| `create-deployment` | Composite action | curl command — runs in caller's job context |
| `update-deployment` | Composite action | curl command — runs in caller's job context |

### Notes
- Passive baseline scan is safe for all environments — no attack payloads sent
- Active scans require a `security-approved` GitHub Environment with required reviewers
- Works with both GitHub Actions and Azure DevOps deployment pipelines
- Self-hosted runners supported for internal/intranet web apps
- Scan results appear in GitHub (Issues, Artifacts) — ADO results integration planned for v1.1
- See [ROADMAP.md](ROADMAP.md) for planned enhancements
