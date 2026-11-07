# Web Application Security Lab Portfolio

**Junior Web Application Security Tester** | Hands-on learning and security research

This repository documents practical web application security exercises, including how I investigated vulnerabilities, verified their impact in authorized training environments, and recommended mitigations.

## Completed PortSwigger Web Security Academy labs

| Lab | Category | Key learning | Report |
| --- | --- | --- | --- |
| 001 — Unprotected admin functionality | Broken Access Control | Discovered an admin endpoint via `robots.txt` and verified missing authorization | [Read report](reports/001-unprotected-admin-functionality.md) |
| 002 — Unprotected admin functionality with unpredictable URL | Broken Access Control | Identified an admin endpoint in client-side JavaScript and verified missing authorization | [Read report](reports/002-unpredictable-admin-url.md) |

## Tools and concepts

- Burp Suite (Proxy HTTP history and Repeater)
- Browser developer tools and HTML/JavaScript source inspection
- HTTP requests, responses, and endpoints
- Authentication versus authorization
- Access-control verification and remediation

## Scope and ethics

All findings documented here come from **intentionally vulnerable PortSwigger Web Security Academy labs**. They are training exercises, not findings from a client engagement. I test only systems I own or am explicitly authorized to assess.

## Evidence

Completion screenshots and supporting request/response captures can be added under `evidence/`. The reports describe observed behavior; no screenshots are claimed as uploaded until they appear in the repository.

## Ongoing development

I am continuing to build practical skills in web application and API security, with an emphasis on clear technical reasoning and reproducible reporting.
