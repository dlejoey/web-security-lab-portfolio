# Lab 002 — Unprotected admin functionality with unpredictable URL

**Platform:** PortSwigger Web Security Academy  
**Category:** Broken Access Control  
**Environment:** Authorized, intentionally vulnerable training lab  
**Outcome:** Solved

## Objective
Determine whether an administrator endpoint remains protected when its URL is difficult to guess.

## Investigation
1. Opened the lab's online shop and inspected its HTML page source.
2. Identified JavaScript that used an `isAdmin` flag to conditionally display an admin link.
3. Found a hard-coded admin path in the client-delivered source, such as `/admin-fp2shv` for the active lab instance.
4. Requested the discovered path on the **same** lab instance.
5. Observed that the administrator panel was accessible even though the browser-side flag indicated a non-admin visitor.
6. Used the lab's **Delete** action for the designated training user `carlos`; the lab displayed its solved state.

## Finding
**Administrative functionality was protected only by obscurity/client-side presentation, not adequate server-side authorization.** The page hid the admin link for non-admin visitors, but exposed its URL in JavaScript and allowed direct access to the privileged functionality.

The exact randomized path can differ between lab instances. Refreshing a page in one instance does not normally generate a new admin URL.

## Impact
A non-admin visitor who inspects the page source can discover the administrative endpoint and perform privileged actions in the training application.

## Recommended remediation
- Validate the user's authenticated identity and administrative privileges **on the server** for every admin request.
- Protect privileged actions independently of whether the admin navigation link is visible.
- Treat client-side flags and unpredictable URLs as presentation or defense-in-depth measures, never as authorization.
- Add tests for direct access attempts by non-admin users.

## Evidence
The lab's solved state was observed during the hands-on session. Supporting screenshots can be added under `evidence/` later.

## What I learned
An unpredictable URL is not a substitute for access control. Client-side JavaScript is visible to users, and the server must enforce permissions regardless of what the interface displays.
