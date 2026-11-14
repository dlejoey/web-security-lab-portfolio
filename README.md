# Web Application Security | Technical Portfolio

**Junior Web Application Security Tester**

Documented web application security assessments covering endpoint discovery, authorization verification, security impact, and actionable remediation. All reported testing was performed in explicitly authorized environments.

## Completed security assessments

| ID | Assessment | Verified issue | Discovery method | Report |
| --- | --- | --- | --- | --- |
| 001 | Unprotected admin functionality | Unauthorized access to administrative user-management functionality | `robots.txt` | [Assessment 001](reports/001-unprotected-admin-functionality.md) |
| 002 | Unprotected admin functionality with unpredictable URL | Unauthorized access to administrative functionality despite a hidden navigation link | Client-side JavaScript inspection | [Assessment 002](reports/002-unpredictable-admin-url.md) |
| 003 | User role controlled by request parameter | Admin access through an editable cookie | Cookie testing in Burp Repeater | [Assessment 003](reports/003-user-role-controlled-by-request-parameter.md) |
| 004 | User role can be modified in user profile | Admin access through an email-change request | JSON request editing in Burp Repeater | [Assessment 004](reports/004-user-role-can-be-modified-in-user-profile.md) |
| 005 | Insecure direct object references (IDOR) | Another user's private chat transcript exposed through filename change | Transcript download request tested in Burp Repeater | [Assessment 005](reports/005-insecure-direct-object-references.md) |
| 006 | URL-based access control can be circumvented | Admin access through X-Original-URL header | Request header testing in Burp Repeater | [Assessment 006](reports/006-url-based-access-control-bypass.md) |
| 007 | Method-based access control can be circumvented | Privilege escalation through inconsistent GET/POST authorization | HTTP method and query-parameter testing in Burp Repeater | [Assessment 007](reports/007-method-based-access-control.md) |
| 008 | User ID controlled by request parameter | Cross-account disclosure of Carlos's API key | User ID query-parameter testing in Burp Repeater | [Assessment 008](reports/008-user-id-controlled-by-request-parameter.md) |

| 009 | User ID controlled by request parameter, with unpredictable user IDs | Cross-account API key disclosure despite unpredictable IDs | Public blog-author identifier discovery and account ID substitution | [Assessment 009](reports/009-user-id-unpredictable-ids.md) |
| 010 | User ID controlled by request parameter with data leakage in redirect | Carlos's API key exposed in the HTTP response despite redirect behavior | Burp Repeater response-body inspection | [Assessment 010](reports/010-user-id-data-leakage-in-redirect.md) |
| 011 | User ID controlled by request parameter with password disclosure | Administrator password disclosed, enabling account takeover | Account ID manipulation and HTML response inspection | [Assessment 011](reports/011-user-id-password-disclosure.md) |

**Outcome:** All eleven assessments completed successfully in PortSwigger Web Security Academy.

## Demonstrated capabilities

- Identified administrative endpoints through publicly accessible application files and client-side source code
- Tested whether server-side authorization prevented non-admin access to privileged functions
- Changed role cookies and profile request fields to test for unauthorized admin access
- Tested transcript download URLs and confirmed unauthorized access to another user's chat
- Confirmed cross-account API key exposure through a user-controlled account ID parameter
- Identified cross-account API key exposure despite unpredictable user IDs and redirect behavior
- Demonstrated administrator credential disclosure and account takeover through missing object-level authorization
- Bypassed URL-based admin restrictions using the X-Original-URL header
- Verified method-dependent authorization failure by comparing rejected POST with successful GET role-change requests
- Analyzed web requests, endpoints, and application behavior using Burp Suite and browser inspection tools
- Documented findings with reproduction steps, impact, and remediation recommendations

## Tools

Burp Suite (Proxy and Repeater) · HTTP/HTTPS · HTML/JavaScript source inspection · Browser tools

## Evidence

Reproduction steps and verified outcomes are documented in each assessment. Supporting screenshots are pending upload.

## Assessment scope

These are **PortSwigger Web Security Academy training assessments**, not commercial client engagements. All activity took place in intentionally vulnerable, authorized lab environments.
