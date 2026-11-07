# Assessment 003 — User Role Controlled by Request Parameter

**Platform:** PortSwigger Web Security Academy  
**Lab:** User role controlled by request parameter  
**Vulnerability class:** Broken Access Control — Vertical Privilege Escalation  
**Test environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved

## 1. Assessment overview

**Objective:** Determine whether a normal authenticated user can obtain administrative access by modifying client-supplied role information.

**Result:** Changing the `Admin` cookie from `false` to `true` caused the application to expose the admin link and accept a request to `/admin`. The administrative interface displayed user-management actions. A request to delete the lab-designated user `carlos` returned an HTTP 302 redirect to `/admin`, and the lab displayed its solved confirmation.

## 2. Testing methodology

1. Logged in with the standard user account supplied by the lab.
2. Used Burp Suite Proxy / HTTP history to inspect requests and cookies associated with the authenticated session.
3. Identified a client-supplied `Admin=false` role indicator.
4. Sent an authenticated request to Burp Repeater and modified the role indicator to `Admin=true`.
5. Observed an administrator-panel link in the resulting HTML.
6. Requested `GET /admin` with the modified cookie and observed the administrative interface, including user-management controls.
7. Requested the lab-authorized deletion endpoint for `carlos` and observed a `302 Found` response with `Location: /admin`.
8. Confirmed completion from the PortSwigger Academy “Congratulations, you solved the lab!” banner.

## 3. Vulnerability identified

**Finding:** The application relied on a client-modifiable role cookie for administrative authorization.

Illustrative request header (session token redacted):

```http
Cookie: session=<redacted>; Admin=true
```

A regular authenticated user could change the administrator flag and reach server-side privileged functionality. The finding is not merely that an admin link became visible: the `/admin` endpoint itself exposed protected administrative features.

## 4. Proof of concept and evidence

- **Role manipulation:** `Admin=false` changed to `Admin=true` in Burp Repeater.
- **Privileged access:** `GET /admin` returned the administrative interface listing `wiener` and `carlos`, including delete links.
- **Administrative action:** `GET /admin/delete?username=carlos` returned `HTTP/2 302 Found` with `Location: /admin`.
- **Verified outcome:** PortSwigger lab displayed its solved confirmation.

**Evidence status:** Screenshots of the Burp requests/responses and solved lab were captured during the assessment but have not yet been uploaded to this repository. The 302 response alone is not treated as proof of deletion; the lab's solved confirmation corroborates the outcome.

## 5. Security impact

An ordinary user could elevate access to administrator-only functionality by modifying data under their own control. This permits unauthorized access to protected user information and administrative actions such as deleting accounts, compromising confidentiality, integrity, and potentially availability.

## 6. Remediation recommendations

- Determine roles and privileges from trusted server-side account/session state, not an editable client cookie.
- Enforce authorization on every administrative endpoint and each state-changing operation.
- Reject unauthorized requests even when the client displays or submits administrator-specific links or parameters.
- Add regression tests that replay privileged requests with standard-user sessions and modified client-controlled role values.

## 7. Technical analysis

The session cookie identifies the authenticated user; the separate `Admin` flag purported to define that user's authorization level. Modifying the flag did not authenticate the user as a different account. Instead, it caused the application to treat the existing ordinary-user session as privileged — a vertical privilege-escalation failure.

**Scope note:** This report documents an intentionally vulnerable PortSwigger training application, not a commercial client assessment.
