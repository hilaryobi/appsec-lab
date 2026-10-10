# AppSec Lab: Securing OWASP Juice Shop

A hands-on application security project built to support the CompTIA Security+ certification. The project takes one web application through a complete security lifecycle: threat modeling, manual exploitation, remediation, automated CI/CD security testing, and incident detection.

**Target application:** a fork of OWASP Juice Shop (a deliberately vulnerable training app)
**Code repo:** https://github.com/hilaryobi/juice-shop
**This repo:** documentation, findings, and reports

---

## Project status

| Step | Description | Status |
|---|---|---|
| 1 | Setup | ✅ Done |
| 2 | Threat modeling | ✅ Done |
| 3 | Attack baseline | ✅ Done |
| 4 | Fix and verify | ✅ Done |
| 5 | CI/CD security pipeline | ✅ Done |
| 6 | Logging and detection | 🔲 In progress |
| 7 | Re-test and measure | 🔲 Pending |
| 8 | Final write-up | 🔲 Pending |
| 9 | Publish | 🔲 Pending |

---

## 1. Setup

Ran OWASP Juice Shop locally in Docker, then forked the project to a personal GitHub repo to allow code changes.

## 2. Threat model

Built a data flow diagram (Browser → App → Database → Outside services) and applied STRIDE to each trust boundary. Risks ranked by Likelihood × Impact.

📄 [`docs/threat-model.md`](docs/threat-model.md)

## 3. Attack baseline

Used Burp Suite Community Edition to manually test the top-ranked risks against the running app.

| # | Vulnerability | Severity |
|---|---|---|
| 1 | Login bypass via SQL injection | Critical |
| 2 | Broken access control on basket endpoint (IDOR) | High |
| 3 | SQL injection via search bar (full user data dump) | Critical |

📄 [`reports/before.md`](reports/before.md)

## 4. Fixes applied

Each vulnerability was fixed at the code level (parameterized queries, ownership checks) and re-tested against the same attack to confirm remediation.

| Fix | Commit |
|---|---|
| Parameterized login query | [link] |
| Parameterized search query | [link] |
| Basket ownership check (IDOR fix) | [link] |

📄 [`reports/after.md`](reports/after.md)

## 5. CI/CD security pipeline

Every push to the target repo now runs four automated security checks:

| Tool | Purpose |
|---|---|
| [Gitleaks](https://github.com/gitleaks/gitleaks) | Secret scanning |
| [Semgrep](https://semgrep.dev/) | Static code analysis (OWASP Top 10) |
| [OSV-Scanner](https://osv.dev/) | Dependency vulnerability scanning |
| [Trivy](https://trivy.dev/) | Container image scanning |

📄 [Pipeline definition](https://github.com/hilaryobi/juice-shop/blob/master/.github/workflows/security.yml)
📄 [Live results](https://github.com/hilaryobi/juice-shop/actions/workflows/security.yml)

**Note:** this step also surfaced a real supply-chain security incident affecting one of the chosen tools (`trivy-action`), requiring the pipeline to be pinned to a confirmed-safe version. Documented in `reports/after.md`.

## 6. Logging and detection

*In progress.* Adding structured logging for failed login attempts, plus a short incident runbook describing the expected response.

📄 `docs/runbook.md` (pending)

## 7. Re-test and measure

*Pending.* Final re-run of all three attacks against the fully patched, pipeline-protected application, with a before/after comparison table.

## 8. Final write-up

*Pending.* Summary report covering the
