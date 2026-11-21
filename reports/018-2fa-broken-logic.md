# Assessment 018 — 2FA Broken Logic

**Assessment type:** Authentication / multi-factor authentication  
**Environment:** PortSwigger Web Security Academy — authorized training lab  
**Difficulty:** Practitioner  
**Status:** Solved — PortSwigger completion banner and solved-labs list verified  
**Tools:** Burp Suite Community Edition (Intruder, Repeater), browser

## Assessment Overview

Tested whether the application's second-factor verification could be bypassed by exploiting flaws in its 2FA login flow. The lab required access to Carlos's account.

## Reproduction

1. Started the lab's login flow and captured the second-factor submission request in Burp.
2. Sent the request to Burp Intruder and marked only the `mfa-code` value as the payload position.
3. Configured a sequential numeric payload from `0000` to `9999`, preserving four digits.
4. Monitored the responses for a successful redirect. The payload `0176` returned **HTTP 302 Found**, unlike the ordinary invalid-code responses.
5. Used the session cookie issued in the successful response for a request to `GET /my-account?id=carlos`.
6. The server returned **HTTP 200 OK** and the page identified the account as Carlos. PortSwigger then displayed the **Solved** banner, and the lab appeared in the solved-labs list.

**Verified result:** The 2FA code `0176` produced the successful response in this lab instance. No session cookie or other session secret is recorded here.

## Finding

The 2FA verification flow could be defeated by guessing the four-digit verification code. The application accepted a valid code and issued an authenticated session, allowing access to Carlos's account.

## Impact

An attacker able to reach the second-factor step could potentially brute-force the short verification code and gain access to the victim's account, particularly if attempts are not sufficiently rate-limited or protected against automation.

## Remediation

- Enforce strict server-side attempt limits and progressive delays for 2FA verification.
- Invalidate or rotate the verification challenge after a small number of failed attempts.
- Bind each code to the correct user, login transaction, and short validity period.
- Make codes single-use and invalidate them immediately after successful verification.
- Monitor and alert on repeated failed MFA attempts.

## Technical Notes

- A **302 response** was the success indicator in this lab run; the response's newly issued session cookie was then used for the authenticated account request.
- The subsequent **200 response** alone was not the only evidence: the returned page identified Carlos, and the PortSwigger lab completion banner confirmed success.
- A single Intruder attack can be stopped as soon as a verified success response appears; there is no need to wait for all remaining payloads once the lab is solved.

## Scope

Testing was performed only against the intentionally vulnerable PortSwigger Web Security Academy lab. This is a training-lab assessment, not a commercial client engagement. Sensitive session values are excluded.
