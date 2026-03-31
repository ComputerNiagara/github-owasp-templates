# Roadmap

Planned enhancements for future releases. Not all items are committed —
priority and timeline depend on community feedback and contributions.

Have a feature request? [Open an issue](../../issues/new?template=feature_request.md).

---

## v1.1 — ADO Results Integration

> Push ZAP scan results back into Azure DevOps so teams don't need to
> check GitHub for security findings.

### ADO Work Items from ZAP Findings
- Parse ZAP scan output after each run
- Automatically create ADO bugs for each finding
- Include severity, description, affected URL, and link to full report
- Close work items automatically when findings are resolved in a subsequent scan
- Configurable: choose which severity levels create work items

### ADO Pipeline Artifact Publishing
- Upload ZAP HTML report as an ADO pipeline artifact
- Report accessible directly from the ADO pipeline run
- No GitHub access required to view results

### Implementation notes
- Requires an ADO PAT stored as a GitHub Actions secret
- Will be opt-in via new inputs on the reusable workflow
- ADO org URL and project name will be configurable per repo

---

## v1.2 — Enhanced Scan Coverage

### ZAP Authenticated Scanning (simplified)
- Streamlined setup for form-based login authentication
- Currently only Bearer token auth is supported via `ZAP_AUTH_HEADER_VALUE`
- v1.2 will add support for session-based auth flows

### OWASP ZAP Custom Rules
- Allow teams to supply a custom ZAP rules file to tune alert severity
- Override specific rule thresholds without forking the reusable workflow

### Scan History and Trending
- Track findings over time across scan runs
- Flag new findings introduced since the last scan
- Useful for measuring security posture improvement over time

---

## Backlog (no version assigned)

- Support for ZAP authenticated scanning via Playwright/browser automation
- GitHub Actions Marketplace listing for easier discovery
- Automated test suite for the reusable workflows
- Support for scanning multiple URLs per repo in a single run
- Integration with Defender for DevOps / Microsoft Security Exposure Management

---

## Completed

### v1.0.0
- ✅ ZAP Baseline passive DAST reusable workflow
- ✅ ZAP Full Scan active DAST reusable workflow
- ✅ ZAP API Scan active DAST reusable workflow
- ✅ Deployment integration (GitHub Actions)
- ✅ Deployment integration (Azure DevOps)
- ✅ Preflight connectivity check for all scans
- ✅ Approval gate for active scans
- ✅ Self-hosted runner support for internal apps
- ✅ Examples: basic web app, API app, deploy integration, ADO integration
