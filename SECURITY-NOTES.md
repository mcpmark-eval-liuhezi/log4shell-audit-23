# Log4Shell (CVE-2021-44228) Security Notes

## Summary

**What it is:** Log4Shell is a critical remote code execution (RCE) vulnerability in Apache Log4j 2, an ultra-fast and widely-used Java logging framework. An attacker who can control log messages or log message parameters can exploit JNDI features to execute arbitrary code loaded from LDAP (and other JNDI) servers. The vulnerability is triggered by self-referential lookups such as `${jndi:ldap://...}`.

## CVE Identifier

- **CVE-2021-44228** — Apache Log4j2 JNDI features do not protect against attacker controlled LDAP and other JNDI related endpoints

## Affected Versions

- **Affected:** Log4j 2.0-beta9 through 2.14.1 (excluding security releases 2.3.1, 2.12.2). Per the Apache Security page: `[2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0)`.
- **Fixed:** 2.15.0 (Java 8+), 2.12.2 (Java 7), 2.3.1 (Java 6).
- **Fully mitigated (functionality removed):** 2.16.0 removes JNDI lookup functionality entirely; 2.17.0 fixes CVE-2021-45105; 2.17.1 is the current recommended version for Java 8+.
- Not impacted: applications using only `log4j-api` without `log4j-core`.

## Severity

- **CVSS 3.1 Base Score: 10.0 — CRITICAL**
- Vector: `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H`
- Listed in the CISA Known Exploited Vulnerabilities (KEV) catalogue; EPSS ~100% (utilization probability).

## Remediation Actions

1. **Upgrade path:** Upgrade to 2.17.1+ (Java 8+), 2.12.4+ (Java 7), or 2.3.2+ (Java 6).
2. **Detecting vulnerable deployments:** Audit build manifests and running JARs for affected log4j-core versions; monitor logs for JNDI lookup patterns (`${jndi:ldap://...}`, `${jndi:rmi://...}`, `${jndi:dns://...}`).
3. **Hardening:** Set `log4j2.formatMsgNoLookups=true` and/or remove the `JndiLookup` class from the log4j-core JAR. Note: `formatMsgNoLookups=true` does NOT mitigate CVE-2021-45046.
4. **Automated dependency scanning:** Enable GitHub Dependabot alerts/security updates, OWASP Dependency-Check, Snyk, Trivy, or Grype in CI to catch vulnerable versions continuously.

## Sources

- Apache Logging Services Security Page: https://logging.apache.org/log4j/2.x/security.html
- NVD CVE-2021-44228: https://nvd.nist.gov/vuln/detail/CVE-2021-44228
- CISA KEV Catalogue: https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- Ubuntu Security CVE-2021-44228: https://ubuntu.com/security/CVE-2021-44228
- Rapid7 Vulnerability Database: https://www.rapid7.com/db/vulnerabilities/cve-2021-44228/
- SANS ISC Log4j2 Security Vulnerabilities Update Guide: https://isc.sans.edu/diary/28188
