# Lab 001 — Unprotected admin functionality

**Platform:** PortSwigger Web Security Academy  
**Category:** Broken Access Control  
**Environment:** Authorized, intentionally vulnerable training lab  
**Outcome:** Solved

## Objective
Investigate whether administrative functionality is protected from unauthenticated or non-administrator visitors.

## Investigation
1. Opened the lab's online shop.
2. Requested `/robots.txt` on the lab host.
3. Found the disclosed path `Disallow: /administrator-panel`.
4. Navigated to `/administrator-panel` on the same lab host.
5. Observed an accessible administrator interface with user-management actions.
6. Used the lab's **Delete** action for the designated training user `carlos`, completing the exercise.

## Finding
**Missing server-side authorization on an administrative endpoint.** The admin interface and its user-management action were available without an administrator privilege check sufficient to block the lab visitor.

The entry in `robots.txt` helped **discover** the path; publishing a path there is not, by itself, the access-control failure.

## Impact
An unauthorized visitor could reach administrative functionality and delete user accounts in the training application. In a production application, analogous flaws could permit unauthorized account changes or other privileged operations.

## Recommended remediation
- Enforce server-side authorization for every administrative route and action.
- Apply deny-by-default access policies and check roles or permissions on each request.
- Avoid relying on hidden links, `robots.txt`, or client-side controls as security boundaries.
- Add automated tests ensuring non-admin users cannot access privileged functions.

## Evidence
The lab was completed during the hands-on session. Completion and request screenshots may be added to the repository separately.

## What I learned
Endpoint discovery and vulnerability verification are different: the issue is not knowing an admin URL, but being able to perform privileged actions without the required authorization.
