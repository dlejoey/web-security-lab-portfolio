# Assessment 016 — 2FA Simple Bypass

**Environment:** PortSwigger Web Security Academy (authorized lab)
**Difficulty:** Apprentice
**Status:** Solved — browser completion banner verified
**Tools:** Browser

## Reproduction
1. Logged in as `wiener` using supplied credentials and noted the account URL `/my-account?id=wiener`.
2. Started login as `carlos` using the supplied password.
3. Arrived at `/login2`, requesting a four-digit security code.
4. Without entering the code, navigated directly to `/my-account?id=carlos`.
5. Carlos's account page opened and PortSwigger displayed **Solved**.

## Finding
The application allowed access to an authenticated account endpoint after password verification without enforcing completion of the second factor.

## Impact
An attacker who has a user's password may bypass 2FA and access their account.

## Remediation
Enforce MFA completion server-side for every protected endpoint. Do not grant a fully authenticated session before verifying the second factor.

## Scope
Only an intentionally vulnerable PortSwigger training environment was tested. No session secrets are included.
