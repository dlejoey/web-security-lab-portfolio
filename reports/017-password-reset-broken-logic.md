# Assessment 017 — Password Reset Broken Logic

**Environment:** PortSwigger Web Security Academy (authorized lab)  
**Difficulty:** Apprentice  
**Status:** Solved — verified by the lab's green Solved banner  
**Tools:** Burp Suite Repeater and browser

## Finding
The password reset endpoint did not properly validate an empty reset token or bind the reset to the intended account, allowing an unauthorized password change.

## Reproduction
1. Initiated the forgot-password flow for `wiener` and captured the password reset submission.
2. Sent the request to Burp Repeater.
3. Left `temp-forgot-password-token` empty in both the URL and request body.
4. Changed the body field to `username=carlos` and supplied matching values in `new-password-1` and `new-password-2`.
5. Sent the modified `POST /forgot-password` request and received `HTTP/2 302 Found`, redirecting to `/`.
6. Logged in as Carlos with the newly set password. The browser displayed `/my-account?id=carlos` and the lab's **Solved** banner.

## Impact
An attacker could reset another user's password and take over the account without possessing the victim's reset token.

## Remediation
- Reject absent, empty, invalid, expired, or previously used reset tokens.
- Bind each token server-side to one account and reset operation.
- Determine the target account from the validated token rather than a user-controlled `username` field.
- Make tokens unpredictable, single-use, and short-lived; log suspicious reset attempts.

## Scope
This assessment was performed only in an intentionally vulnerable PortSwigger training lab. Session cookies, reset tokens, and replacement passwords are omitted from this report.
