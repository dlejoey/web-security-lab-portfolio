# Assessment 001 — Unprotected Admin Functionality

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Broken Access Control  
**Test environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved

## 1. Assessment overview

**Objective:** Determine whether an ordinary visitor can discover and use administrative functionality without administrator authorization.

**Result:** An administrative user-management interface was reachable, and the lab's designated user-deletion action succeeded without an administrator privilege check sufficient to prevent it.

## 2. Testing methodology

1. Accessed the lab's online shop.
2. Inspected the site's `/robots.txt` file.
3. Identified an administrative path in its crawler directives.
4. Requested the disclosed path directly in the browser.
5. Examined the accessible administrative interface and its user-management actions.
6. Executed the lab-authorized deletion of the designated training user, `carlos`, and completed the exercise.

## 3. Vulnerability identified

**Finding:** Missing server-side authorization for administrative functionality.

The publicly readable `robots.txt` file disclosed the endpoint:

```text
User-agent: *
Disallow: /administrator-panel
```

The disclosure was a **discovery clue**, not the underlying vulnerability. The security failure was that a visitor lacking administrator privileges could access the endpoint and use a privileged function.

## 4. Proof of concept and evidence

**Endpoint:** `/administrator-panel`

**Observed behavior:** The admin interface displayed user-management actions, including deletion. The authorized training action against `carlos` succeeded, satisfying the lab objective.

**Evidence status:** Lab completion was observed during testing. Screenshots and raw HTTP captures have not yet been uploaded to this repository.

## 5. Security impact

Missing authorization on privileged operations can permit unauthorized user-account modification or deletion. The demonstrated impact is limited to the intentionally vulnerable training application; production impact would depend on the actual functions exposed.

## 6. Remediation recommendations

- Enforce server-side authorization checks for every administrative route and state-changing operation.
- Apply deny-by-default access policies and validate permissions on each request.
- Never treat hidden URLs, navigation visibility, or `robots.txt` directives as access controls.
- Add automated negative tests confirming that non-admin sessions cannot access privileged operations.

## 7. Technical analysis

Endpoint enumeration identifies potential attack surfaces; it does not prove exploitation. The decisive test is whether the server rejects a request made without the privileges required for that operation. In this lab, the server permitted an administrative action, demonstrating broken access control.
