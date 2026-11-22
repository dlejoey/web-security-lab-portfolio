# Assessment 019 — Password Reset Poisoning via Middleware

**Assessment type:** Authentication / password-reset poisoning  
**Environment:** PortSwigger Web Security Academy — authorized training lab  
**Difficulty:** Practitioner  
**Status:** Solved — PortSwigger completion banner verified  
**Tools:** Burp Suite Community Edition (Repeater), browser, PortSwigger exploit server

## Assessment Overview

Tested whether the password-reset workflow trusted a client-supplied forwarded-host value when constructing reset links. The lab required access to Carlos's account.

## Reproduction

1. Started the password-reset flow and captured the `POST /forgot-password` request in Burp Repeater.
2. Changed the submitted username to `carlos`.
3. Added an `X-Forwarded-Host` header containing the hostname of the lab-provided exploit server.
4. Sent the modified request. The application returned **HTTP 200 OK**.
5. Opened the exploit server's `/log` page and observed a victim browser request to a password-reset URL containing a `temp-forgot-password-token`. The exploit-server log showed **HTTP 404** for that request; this did not prevent the token from appearing in the logged request path.
6. Extracted the reset token from the logged URL and used it with the legitimate Web Security Academy lab hostname, rather than the exploit-server hostname.
7. The application accepted the token, provided the password-reset flow, and ultimately displayed Carlos's account with the **Solved** banner.

**Verified result:** The captured reset token was accepted by the legitimate application, and PortSwigger confirmed that the lab was solved. The token and session values are intentionally omitted.

## Finding

The application trusted the `X-Forwarded-Host` request header when generating password-reset links. By controlling this header, an attacker could cause a reset link sent to a victim to point to an attacker-controlled host, exposing the reset token when the victim followed the link.

## Impact

An attacker may be able to capture a victim's password-reset token and use it against the legitimate application to reset the victim's password and take over the account.

## Remediation

- Do not derive password-reset link hosts from untrusted `Host`, `X-Forwarded-Host`, or similar client-controlled headers.
- Generate absolute reset URLs using a trusted, explicitly configured canonical application origin.
- Accept forwarded headers only from trusted reverse proxies, with strict proxy configuration and validation.
- Keep reset tokens unpredictable, short-lived, single-use, and bound server-side to the intended account and reset transaction.
- Monitor password-reset requests and suspicious use of forwarded-host headers.

## Technical Notes

- **HTTP 200 OK** from the initial forgot-password request indicated that the request was processed; it did not by itself prove successful exploitation.
- The exploit-server log's **404** response was not evidence that token capture had failed. The requested URL, including the reset token, was still recorded in the log.
- The final proof was successful use of the token on the legitimate lab domain and the PortSwigger **Solved** banner.

## Scope

Testing was performed only against the intentionally vulnerable PortSwigger Web Security Academy lab. This is a training-lab assessment, not a commercial client engagement. Reset tokens, session cookies, and unique lab hostnames are excluded.
