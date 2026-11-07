# Assessment 002 — Unprotected Admin Functionality with Unpredictable URL

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Broken Access Control  
**Test environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved

## 1. Assessment overview

**Objective:** Evaluate whether an administrative endpoint is protected by server-side authorization when its URL is not easily guessed.

**Result:** Client-side JavaScript disclosed the administrator path. Direct navigation to that path exposed administrative functions, and the lab-authorized user deletion succeeded.

## 2. Testing methodology

1. Accessed the lab's online shop and inspected the HTML page source.
2. Located JavaScript that conditionally displayed an administrator link based on an `isAdmin` flag.
3. Extracted the admin path from the client-delivered source.
4. Navigated to that path on the **same active lab instance**.
5. Confirmed that the administrator interface was accessible despite the visitor not being an administrator.
6. Deleted the lab's designated training user, `carlos`, and observed the solved state.

## 3. Vulnerability identified

**Finding:** Missing server-side authorization on an administrative endpoint, with its path exposed in client-side JavaScript.

A relevant source-code pattern was:

```javascript
var isAdmin = false;
// The page conditionally creates an admin link.
// Its href contains the admin endpoint path.
```

An example path observed in the active lab instance was `/admin-fp2shv`. Lab instances can use different paths.

Hiding a navigation link when `isAdmin` is false affects only the browser interface; it does not enforce server-side access control.

## 4. Proof of concept and evidence

**Entry point:** Admin path discovered in client-side JavaScript.

**Observed behavior:** Requesting the discovered path directly opened an administrator panel with user-management functions. The lab's designated deletion action succeeded.

**Evidence status:** The solved state was observed during testing. Screenshots and raw HTTP captures have not yet been uploaded to this repository.

## 5. Security impact

An unauthorized visitor who examines client-side source code can discover the administrative route and invoke privileged operations in the training application. An unpredictable endpoint name does not compensate for missing authorization.

## 6. Remediation recommendations

- Check authentication and administrative privileges on the server for each privileged request.
- Independently authorize state-changing operations, not just access to the admin page.
- Avoid treating JavaScript flags or hard-to-guess paths as security boundaries.
- Test direct endpoint access using non-admin and unauthenticated sessions.

## 7. Technical analysis

Client-side code is delivered to the visitor and can be inspected regardless of whether an interface element is displayed. Authorization must be enforced at the server boundary. This assessment demonstrated that knowing the admin path was sufficient to reach and use privileged functionality, confirming a broken access-control condition.
