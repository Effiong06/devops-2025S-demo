@'
# Security Findings Register

**Last reviewed:** 2026-11-10
**Next review:** 2026-12-10
**Owner:** DevOps team / repository maintainer

## 1. Remediated findings

| Scanner | Finding | Resolution | Status |
|---|---|---|---|
| Bandit | B105 â€“ hardcoded password string (Low) | Replaced the hardcoded value with environment-based configuration. Latest scan reports no issues. | Remediated |
| Gitleaks | Previously exposed Django SECRET_KEY | Moved the secret to environment-based configuration and removed the exposed secret from repository history. Latest scan passed. | Remediated |
| Trivy | Django/Python dependency vulnerabilities, including CVE-2025-64459 | Upgraded Django from 5.2.7 to 5.2.19. Latest scan reports no Python package vulnerabilities. | Remediated |
| Checkov | CKV_AWS_136 â€“ ECR repository encryption | Added KMS encryption configuration to the ECR repository. Latest scan: 4 passed, 0 failed. | Remediated |

## 2. Accepted risks â€“ Trivy image scan

The latest image scan reports **47 HIGH and 0 CRITICAL vulnerabilities**. The remaining findings are in OS packages from the Debian 13 base image.

| CVE | Affected package(s) | Severity | Decision |
|---|---|---|---|
| CVE-2025-69720 | libncursesw6, libtinfo6, ncurses-base, ncurses-bin | HIGH | Accept pending Debian stable security update |
| CVE-2026-119373 | SQLite packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-119374 | SQLite packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-119379 | SQLite packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-16742 | libsystemd0, libudev1 | HIGH | Accept pending Debian stable security update |
| CVE-2026-54369 | libacl1 | HIGH | Accept pending Debian stable security update |
| CVE-2026-76642 | util-linux packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-78408 | util-linux packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-78409 | util-linux packages | HIGH | Accept pending applicability verification and package update |
| CVE-2026-78410 | util-linux packages | HIGH | Accept pending Debian stable security update |
| CVE-2026-9538 | perl-base / Archive-Tar | HIGH | Accept pending package fix |

**Risk justification:** These findings remain in the current Debian 13 image after applying available package upgrades. Moving to Debian unstable or suppressing findings without verified justification would introduce unnecessary risk. The findings are therefore documented for monitoring rather than silently ignored.

**Mitigation:** The Trivy CI gate blocks CRITICAL vulnerabilities. HIGH findings remain visible in scan reports and are documented above.

**Review action:** Recheck Debian security updates and package fixes by 2026-12-10. Rebuild and rescan the image when applicable fixes become available.

## 3. Current CI enforcement policy

| Scanner | Policy | Blocking |
|---|---|---|
| Bandit | Warning-only; `continue-on-error: true` | No |
| Gitleaks | Secret detection | Yes |
| Trivy | CRITICAL only; `exit-code: "1"` | Yes |
| Checkov | `soft_fail: false` | Yes |
'@ | Set-Content -Encoding utf8 .\SECURITY-FINDINGS-REGISTER.md
