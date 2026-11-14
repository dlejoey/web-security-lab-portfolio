# Assessment 011 — User ID Controlled by Request Parameter with Password Disclosure

**Platform:** PortSwigger Web Security Academy  
**Status:** Solved  
**Scope:** Authorized training lab

## Assessment overview
Logged in as the normal user `wiener`, then changed the account URL parameter to `id=administrator`. The application returned the administrator's account information, including a populated password input field in the HTML response. The exposed password was used to access the administrator account, delete the designated training user `carlos`, and complete the lab.

## Reproduction
1. Log in as `wiener`.
2. Capture `GET /my-account?id=wiener` in Burp Suite Repeater.
3. Change the request to `GET /my-account?id=administrator` using the normal user's session.
4. Inspect the response for `Your username is: administrator` and the password field `name="password" value="[redacted]"`.
5. Log in as `administrator` with the disclosed credential.
6. Use the admin panel to delete `carlos`.

## Verified evidence
- Administrator account data was returned to a normal user's session.
- Password value was exposed in HTML, despite being masked in the browser input.
- Administrator login and deletion succeeded.
- The page displayed **User deleted successfully!** and **Congratulations, you solved the lab!**.

## Security impact
Broken object-level authorization caused credential disclosure, enabling administrator account takeover and privileged actions.

## Remediation
- Authorize every account read against the authenticated user on the server.
- Never return existing plaintext passwords; store password hashes and use secure password reset flows.
- Independently authorize all privileged actions.
- Test cross-user access and sensitive-field disclosure.

## Technical analysis
The vulnerability was not merely an editable `id` parameter: the server failed to enforce ownership and returned sensitive account data. An HTML input with `type="password"` masks its display but does not protect a password value included in the HTTP response.

**Evidence note:** Screenshots were reviewed during the assessment but are not yet attached to the GitHub report. Passwords and session tokens are deliberately omitted.
