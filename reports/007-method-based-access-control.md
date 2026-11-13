# Assessment 007 — Method-Based Access Control Can Be Circumvented

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Broken Access Control / HTTP method-based authorization bypass  
**Environment:** Authorized, intentionally vulnerable training lab  
**Status:** Solved

## 1. Assessment overview

**Objective:** Determine whether a standard user could perform an administrator-only role change by changing the HTTP method.

**Result:** A privileged role-change request made with Wiener's non-admin session was rejected when submitted as POST, but a GET request with the same intended operation was processed. Changing Wiener's role through GET resulted in the lab's confirmed solved state.

## 2. Testing methodology

1. Logged into the training application as the standard user `wiener`.
2. Inspected account and administrative requests in Burp Suite Proxy and Repeater.
3. Tested `POST /admin-roles` using Wiener's session and observed `401 Unauthorized`.
4. Changed the method to GET. The initial request returned `400 Bad Request` with `Missing parameter 'username'`, indicating the endpoint expected parameters in a different location for GET.
5. Placed the username and action in the query string. A GET request to upgrade `carlos` returned `302 Found` redirecting to `/admin`; this alone was not taken as proof of the requested role change.
6. Submitted the GET role-change request targeting `wiener`, using Wiener's session cookie.
7. Observed `302 Found` and verified the PortSwigger **Congratulations, you solved the lab!** banner.

## 3. Vulnerability identified

**Finding:** Inconsistent server-side authorization checks across HTTP methods for a privileged role-management endpoint.

The application denied the role-management operation via POST for the standard-user session but accepted a corresponding GET request that changed the user's role. The issue is not that GET exists; it is that the server failed to enforce administrator authorization for the sensitive action on that method.

## 4. Proof of concept and evidence

**Rejected baseline:**
```http
POST /admin-roles HTTP/2
Cookie: session=<standard-user-session>
Content-Type: application/x-www-form-urlencoded

username=wiener&action=upgrade
```
**Observed:** `401 Unauthorized`.

**Method-bypass request:**
```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Cookie: session=<standard-user-session>
```
**Observed:** `302 Found`, `Location: /admin`; PortSwigger subsequently displayed the **solved** banner.

The cookie shown is a placeholder; no active session secrets are published. The GET request shown isolates the material parameters; unrelated browser headers are omitted.

**Evidence status:** Request/response and solved-state screenshots were observed during testing but have not yet been uploaded to the repository.

## 5. Security impact

A normal authenticated user could invoke a privileged account-role modification without administrator authorization. In a real application, this could allow unauthorized privilege escalation and compromise administrative functions.

## 6. Remediation recommendations

- Enforce server-side authorization for every privileged operation, independently of the HTTP method.
- Restrict state-changing role-management actions to intended methods and reject unsupported methods.
- Do not use GET for operations that modify privileges or other server state.
- Test role-management endpoints across methods using accounts with different privilege levels.
- Validate both the requested target account and the caller's authority to change its role.

## 7. Technical analysis

The comparison between `401` on POST and the successful GET-based lab completion demonstrated a method-dependent authorization gap. A `302` response alone would not establish a privilege change; the solved state provided outcome confirmation. The test highlights the need to authorize the **operation**, not merely a particular request shape.
