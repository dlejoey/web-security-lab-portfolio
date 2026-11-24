# Assessment 020 — Password Brute-Force via Password Change

**Assessment type:** Authentication / password brute-force  
**Environment:** PortSwigger Web Security Academy — authorized training lab  
**Difficulty:** Practitioner  
**Status:** Solved — PortSwigger completion banner verified  
**Tools:** Burp Suite Community Edition (Proxy HTTP history, Repeater, Intruder), browser

## Assessment Overview

Tested whether the password-change workflow could be abused to identify another user's current password by submitting candidate passwords as the current password and deliberately making the two new-password fields different.

## Reproduction

1. Logged in to the lab with the provided account and opened the password-change form.
2. Submitted a password-change request and sent it to Burp Repeater from Proxy HTTP history. Interception did not need to remain enabled; the captured request could be replayed directly in Repeater.
3. Set the target username to the lab's target account.
4. Kept the two new-password values different (for example, `test123` and `test456`). This ensured that a request with a valid current password would progress to the new-password confirmation check and return a distinguishable error.
5. Sent the request to Intruder and placed a single payload position around the `current-password` value.
6. Loaded candidate passwords and ran the attack.
7. Compared response lengths and bodies. One candidate produced a response length of **4114**, while ordinary responses were **4117**. The shorter response contained the message **“New passwords do not match”**, showing that the current-password check had succeeded and processing had reached the confirmation check.
8. Used the identified candidate to log in to the target account.

**Verified result:** Successfully accessed the target account, and PortSwigger displayed the **Solved** banner. The candidate password is intentionally omitted from this public report.

## Finding

The password-change endpoint allowed a user identifier to be supplied and exposed distinguishable responses depending on whether the submitted current password was valid. By deliberately making the new-password fields mismatch, an attacker could use the confirmation error as an oracle to validate password guesses for another account.

## Impact

An attacker able to reach this endpoint could brute-force another user's current password if the application does not properly bind the password-change operation to the authenticated user's identity and enforce effective rate limiting and anti-automation controls. A successful guess could enable account takeover.

## Remediation

- Derive the account being changed from the authenticated session; do not trust a client-supplied username to select the target account.
- Require recent re-authentication and verify the current password for the authenticated account.
- Return generic, indistinguishable errors that do not reveal which validation stage failed.
- Apply rate limits, progressive delays, and monitoring to password-change attempts.
- Invalidate relevant sessions and notify users after sensitive credential changes.

## Technical Notes

- **HTTP status alone was not sufficient.** Responses could share the same status code while differing in body content and response length.
- The useful signal was the shorter response and its explicit **“New passwords do not match”** message.
- Proxy interception was not required for the attack. Capturing the request in HTTP history and replaying it through Repeater/Intruder was sufficient.
- The different new-password values were intentional: they forced a valid current-password guess to fail at a later validation step, creating a distinguishable response.

## Scope

Testing was performed only against the intentionally vulnerable PortSwigger Web Security Academy lab. This is a training-lab assessment, not a commercial client engagement. Real credentials and session cookies are not included.
