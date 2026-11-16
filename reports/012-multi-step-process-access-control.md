# Assessment 012 — Multi-step process with no access control on one step

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Vulnerability:** Broken access control / privilege escalation in a multi-step workflow  
**Status:** Solved

## 1. Overview

I tested whether a user-role change workflow enforced administrator authorization at each step. The application used an administrator-only role-selection step followed by a confirmation request. The final confirmation request could be replayed with a standard user's session to upgrade that same user.

## 2. Testing methodology

1. Authenticated to the lab as the provided administrator and inspected the role-management workflow.
2. Initiated an upgrade for the lab account `carlos`, advanced to the confirmation page, and captured the resulting `POST /admin-roles` confirmation request in Burp Suite.
3. Authenticated as the standard user `wiener` and obtained that user's session cookie.
4. In Burp Repeater, reused the confirmation request with Wiener's session cookie and changed the target account in the form body from `carlos` to `wiener`.
5. Submitted the modified request and verified that the lab displayed **“Congratulations, you solved the lab!”** and marked the assessment **Solved**.

## 3. Vulnerability identified

The final confirmation step failed to enforce the administrator-only authorization requirement. A standard user could bypass the protected earlier step and directly submit a privileged role-change request.

**Affected endpoint:** `POST /admin-roles`

**Proof-of-concept form body:**

```http
POST /admin-roles HTTP/2
Cookie: session=[REDACTED]
Content-Type: application/x-www-form-urlencoded

action=upgrade&confirmed=true&username=wiener
```

**Observed response:**

```http
HTTP/2 302 Found
Location: /admin
```

The redirect alone was not considered proof of success; the PortSwigger **Solved** status and congratulations banner provided confirmation.

## 4. Security impact

A low-privileged authenticated user could escalate to administrator privileges and gain access to administrative functions. Multi-step workflows remain vulnerable when authorization is enforced only at the beginning rather than at every state-changing operation.

## 5. Remediation recommendations

- Enforce server-side administrator authorization on **every** role-change request, including final confirmation.
- Validate that the authenticated user is permitted to change the specified target user's role.
- Do not trust client-supplied `username`, `action`, or `confirmed` fields as proof of permission.
- Add negative tests for standard-user sessions against each step in privileged workflows.

## 6. Evidence

Three screenshots were captured: a Burp request with Wiener's session selected, the lab's solved confirmation, and the role-change request/302 response. Session-cookie values must be redacted before publishing screenshots.

- [Evidence 1 — Wiener session in Burp](../evidence/012-wiener-session.png)
- [Evidence 2 — Solved confirmation](../evidence/012-solved.png)
- [Evidence 3 — Role-change request and response](../evidence/012-role-change-request.png)

**Evidence image links will become active after the sanitized screenshots are uploaded.**

## 7. Technical analysis

The key weakness was not the existence of a confirmation page or the use of a POST request. It was the failure to recheck authorization at the sensitive final action. This is a workflow authorization flaw, distinct from the earlier method-based authorization bypass lab.

**Scope:** Authorized, intentionally vulnerable PortSwigger training environment; not a commercial client assessment.
