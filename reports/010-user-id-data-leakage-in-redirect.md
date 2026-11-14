# Assessment 010 — User ID Controlled by Request Parameter with Data Leakage in Redirect

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Broken Access Control / Sensitive Information Disclosure  
**Environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved (verified)

## Assessment overview

**Objective:** Retrieve the API key belonging to `carlos` by testing an account page that redirects unauthorized access.

**Result:** The application disclosed Carlos's account content in an HTTP response despite its redirect behavior. The exposed API key was used to complete the lab.

## Testing methodology

1. Logged in as the provided normal user, `wiener`.
2. Inspected the account page request in Burp Suite Repeater.
3. Changed the `id` query parameter from `wiener` to `carlos`.
4. Examined the **raw HTTP response in Burp**, rather than relying only on what the browser displayed after navigation.
5. Located `Your username is: carlos` and the API key in the response HTML.
6. Submitted Carlos's API key and verified the lab's **Solved** banner.

## Vulnerability identified

The application attempted to restrict cross-account access through a redirect, but sensitive account content was still exposed in the response body.

**Request pattern:**

```http
GET /my-account?id=carlos HTTP/2
Host: <active-lab-host>
Cookie: session=<wiener-session>
```

The browser's eventual destination does not erase sensitive data already transmitted in an HTTP response.

## Evidence and outcome

The shared Burp Repeater screenshot showed the request for `id=carlos` and response HTML containing Carlos's username and API key. A later screenshot showed **Congratulations, you solved the lab!** The key and session cookie are intentionally omitted. Binary screenshots have not yet been attached to GitHub.

## Security impact

A user may retrieve another user's API key even when the interface appears to redirect them away. Redirecting the browser is not a substitute for withholding unauthorized data.

## Remediation

- Check object-level authorization **before** loading or serializing account details.
- Return an appropriate denial or redirect without including private data in the response body.
- Test both response headers and bodies for sensitive information in denied or redirected requests.
- Avoid returning API credentials except to the verified account owner.

## Technical analysis

The crucial observation was made in the HTTP response, not the final browser view. An access-control decision must prevent unauthorized data from being sent at all; client-side navigation cannot undo disclosure.
