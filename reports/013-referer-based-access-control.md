# Assessment 013 — Referer-based Access Control

**Platform:** PortSwigger Web Security Academy  
**Lab:** Referer-based access control (Practitioner)  
**Vulnerability:** Broken access control / privilege escalation through reliance on the HTTP Referer header  
**Status:** Solved — confirmed by the PortSwigger lab banner

## 1. Overview

I tested whether the role-management endpoint enforced server-side authorization for a normal user, or instead relied on the HTTP `Referer` header as evidence that a request came from the administrator interface.

The lab was solved by replaying a role-upgrade request with the standard user's session cookie and changing the target username to `wiener`, while retaining an admin-page `Referer`.

## 2. Assessment procedure

1. Signed in as `administrator` and used the admin interface to upgrade `carlos`.
2. Observed the resulting role-change request in Burp Suite: `GET /admin-roles?username=carlos&action=upgrade`.
3. Signed in as the normal user `wiener` and captured Wiener's session cookie.
4. In Burp Repeater, replaced the administrator session cookie with Wiener's session and changed `username=carlos` to `username=wiener`.
5. Sent the modified request. Burp returned `HTTP/2 302 Found` with `Location: /admin`.
6. Verified the PortSwigger application displayed **“Congratulations, you solved the lab!”**

The `Referer` was observed as an admin-page URL in the working request. The user also noticed that the original and replayed requests could carry the same `Referer` despite having different session cookies.

## 3. Proof of concept

Representative request, with the session token and instance hostname deliberately replaced by placeholders:

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: <authorized-lab-host>
Cookie: session=<redacted-wiener-session>
Referer: https://<authorized-lab-host>/admin
```

Observed response:

```http
HTTP/2 302 Found
Location: /admin
```

**Verification:** The PortSwigger lab displayed its Solved status and congratulations banner after the role change.

### Additional control test

After completing the lab, I changed the `Referer` value to `/my-account` and observed `HTTP/2 401 Unauthorized`. This result is consistent with Referer-based gating, but it is **not conclusive by itself**, because the subsequent test was performed after the account had already been upgraded and the lab may have had a different session or state. A controlled same-session comparison would be needed to isolate the header's effect.

## 4. Security impact

A normal authenticated user could perform a privileged role-management action and elevate their own account to administrator. Trusting a client-supplied navigation header instead of validating the user's privileges allows unauthorized administrative changes.

## 5. Remediation

- Enforce role-based authorization on the server for every privileged role-change request.
- Determine privileges from the authenticated user's server-side identity, not the `Referer` header.
- Reject unauthorized role changes regardless of navigation path, request method, or client-controlled headers.
- Add regression tests for role-management actions from standard-user sessions.

## 6. Technical analysis

The session cookie identifies the caller; the `Referer` describes a claimed navigation source and is not an authorization credential. Two different users may send the same `Referer` value. The security boundary must be enforced against the caller's actual privileges.

**Scope:** Intentionally vulnerable, authorized PortSwigger Web Security Academy training environment; not a commercial client assessment.

**Evidence screenshots:** Captured during the lab and provided in the conversation. Image files have not yet been committed to this repository; screenshots should be redacted before public upload.
