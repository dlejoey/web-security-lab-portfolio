# Web Application Security | Technical Portfolio

**Junior Web Application Security Tester**

Hands-on experience investigating web application vulnerabilities, examining HTTP requests and responses, testing access controls, and documenting findings with actionable remediation recommendations.

This repository presents reproducible security assessments completed in **authorized, intentionally vulnerable environments**. Each report separates endpoint discovery from vulnerability verification and documents the observed security impact.

## Security assessment reports

| ID | Assessment | Vulnerability class | Discovery technique | Report |
| --- | --- | --- | --- | --- |
| 001 | Unprotected admin functionality | Broken Access Control | `robots.txt` endpoint disclosure | [View assessment](reports/001-unprotected-admin-functionality.md) |
| 002 | Unprotected admin functionality with unpredictable URL | Broken Access Control | Client-side JavaScript source inspection | [View assessment](reports/002-unpredictable-admin-url.md) |

**Results:** Both PortSwigger Web Security Academy labs were completed successfully.

## Demonstrated technical capabilities

- **Reconnaissance and endpoint discovery:** Inspecting publicly accessible application files and client-side source code to identify relevant paths
- **Authorization testing:** Verifying whether privileged functions enforce server-side access controls
- **HTTP analysis:** Inspecting application requests and responses using Burp Suite and browser tools
- **Security reporting:** Documenting reproduction steps, observed impact, and remediation recommendations

## Tools and technologies

Burp Suite (Proxy and Repeater) · HTTP/HTTPS · HTML and JavaScript inspection · Browser tools · PortSwigger Web Security Academy

## Assessment evidence

The reports include reproduction steps and observed outcomes. Supporting screenshots will be linked after they are uploaded to the repository; no screenshots are represented as currently published.

## Scope and authorization

These assessments were performed against **PortSwigger Web Security Academy training labs**, not live client systems. The applications are intentionally vulnerable, and the described actions were part of the lab objectives. Testing is limited to owned or explicitly authorized environments.

## Technical focus

Web application and API security, with an emphasis on authorization testing, vulnerability verification, and clear, reproducible technical reporting.
