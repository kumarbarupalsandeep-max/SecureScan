# SecureScan — Deep Product & Market Research

**Owner / Content Creator:** Sandeep Barupal  
**Copyright:** © 2026 Sandeep Barupal. All rights reserved, except third-party resources under their own terms.  
**Research status:** Deep-research baseline v1.0 — desk research; scanner coverage and security claims require validation.

## 1. Product thesis
SecureScan should give engineering teams a prioritized, explainable view of application risk across source code, dependencies, secrets, containers, infrastructure-as-code and running web apps. It should help developers fix issues in their normal workflow, not just generate a large undifferentiated vulnerability list.

## 2. Security category landscape
Security testing is a collection of complementary methods: SAST analyzes code patterns; SCA identifies vulnerable or risky dependencies; secret scanning finds exposed credentials; container/IaC scanners inspect build and deployment artifacts; DAST tests running applications; manual review remains important for business logic and access-control flaws. OWASP WSTG discusses code-level review, SAST, secret scanning, SCA and dynamic testing. OWASP ASVS provides testable security requirements. Sources: [OWASP WSTG](https://wstg.owasp.org/latest/3-The_OWASP_Testing_Framework/0-The_Web_Security_Testing_Framework/), [OWASP ASVS](https://asvs.owasp.org/), [Snyk docs](https://docs.snyk.io/).

## 3. Competitor landscape
| Category | References | Research questions |
|---|---|---|
| Developer security platform | Snyk | Code/dependency/container/IaC coverage, developer workflow, prioritization |
| Git-hosted application security | GitHub Advanced Security | Repository-native alerts, secret protection, code scanning and permissions |
| SAST / rules engine | Semgrep and similar | Language coverage, rule customization, false positives, CI integration |
| Dependency scanning | OWASP Dependency-Check | Ecosystem coverage, vulnerability database mapping and false positives |
| Dynamic testing | OWASP ZAP | Crawl/authenticated testing, scan safety and reporting |

Do not imply equivalent detection quality without benchmark datasets, versioned rules and measured false-positive/negative rates.

## 4. User jobs
Developer: understand and fix a finding. AppSec: triage risk and enforce policy. Security leader: see exposure trends. Release manager: know whether a release violates policy. Auditor: obtain evidence and remediation history.

## 5. Feature specification
### P0
- Repository onboarding with least-privilege access and scan scope preview.
- SAST/SCA/secret-scan adapters; findings normalized with source, rule, severity, confidence, evidence, location and timestamp.
- Deduplication across scans; suppressions with reason, owner, expiry and review.
- Triage states, assignee, remediation guidance and links to code/commit.
- Policy gates, scan history, export and audit log.
- Clear distinction between scanner output and manually verified finding.
### P1
- Container and IaC scans; SBOM ingestion; reachability/context where available.
- PR annotations, CI checks, ticket creation and remediation SLA.
- Risk prioritization using exploitability, asset criticality, exposure and known exploited status; explain score factors.
### P2
- DAST orchestration in authorized test environments; secrets rotation workflows; organization-wide risk graph.

## 6. Page map
Security Overview; Projects/Repositories; Scan Setup; Findings; Finding Detail; Vulnerability Trends; Dependency/SBOM; Secrets; Containers; IaC; Policies; Exceptions; Remediation Queue; CI Integrations; Reports; Audit; Roles; Settings.

## 7. Architecture
Use isolated scanner workers, queue-based orchestration, signed/versioned scanner images, result normalization, evidence storage, deduplication and policy evaluation. Scans must run only against authorized targets. Sandbox untrusted repository content; avoid executing build scripts with privileged credentials. Store secrets only in a secrets manager and pass short-lived scoped tokens to workers.

## 8. Security and trust
Tenant isolation, RBAC, SSO/MFA, encryption, audit logs, data minimization, secure deletion, scanner supply-chain verification, rate limits and safe failure modes. Avoid storing raw secrets in findings. Provide confidence and known coverage limitations. Suppression must never erase historical evidence. Require explicit authorization and scope confirmation before DAST or active testing.

## 9. Metrics and acceptance criteria
- Finding has reproducible source/evidence, scanner/rule version and first/last seen timestamps.
- Repeated scans deduplicate without losing history.
- Suppression includes owner, reason, expiry and audit event.
- Policy gate explains blocking rule and remediation path.
- Metrics: mean time to remediate, actionable finding rate, false-positive rate, recurring findings, coverage by repo and overdue critical findings.

## 10. Risks and validation
Risks: false confidence, scanner false positives, unsafe dynamic scans, secret leakage, supply-chain compromise and score opacity. Validate using intentionally vulnerable test fixtures and known findings; measure false positives and scan reproducibility. Never market a clean scan as proof that software is secure.

## 11. References
- [OWASP WSTG](https://wstg.owasp.org/latest/3-The_OWASP_Testing_Framework/0-The_Web_Security_Testing_Framework/)
- [OWASP ASVS 5.0](https://asvs.owasp.org/)
- [Snyk documentation](https://docs.snyk.io/)
- [OWASP Dependency-Check](https://owasp.org/projects/Dependency-Check)
- [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)

## 12. Next work
Build a feature matrix by scanner type and language, validate API/licensing terms, test result normalization with sample SARIF/JSON, and document authorized-scan safeguards.