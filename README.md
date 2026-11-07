# Web Application Security | Technical Portfolio

**Junior Web Application Security Tester**

Documented web application security assessments covering endpoint discovery, authorization verification, security impact, and actionable remediation. All reported testing was performed in explicitly authorized environments.

## Completed security assessments

| ID | Assessment | Verified issue | Discovery method | Report |
| --- | --- | --- | --- | --- |
| 001 | Unprotected admin functionality | Unauthorized access to administrative user-management functionality | `robots.txt` | [Assessment 001](reports/001-unprotected-admin-functionality.md) |
| 002 | Unprotected admin functionality with unpredictable URL | Unauthorized access to administrative functionality despite a hidden navigation link | Client-side JavaScript inspection | [Assessment 002](reports/002-unpredictable-admin-url.md) |

**Outcome:** Both assessments completed successfully in PortSwigger Web Security Academy.

## Demonstrated capabilities

- Identified administrative endpoints through publicly accessible application files and client-side source code
- Tested whether server-side authorization prevented non-admin access to privileged functions
- Analyzed web requests, endpoints, and application behavior using Burp Suite and browser inspection tools
- Documented findings with reproduction steps, impact, and remediation recommendations

## Tools

Burp Suite (Proxy and Repeater) · HTTP/HTTPS · HTML/JavaScript source inspection · Browser tools

## Evidence

Reproduction steps and verified outcomes are documented in each assessment. Supporting screenshots are pending upload.

## Assessment scope

These are **PortSwigger Web Security Academy training assessments**, not commercial client engagements. All activity took place in intentionally vulnerable, authorized lab environments.
