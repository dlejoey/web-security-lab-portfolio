# Assessment 014 — Username Enumeration via Different Responses

**Assessment type:** Authentication security  
**Environment:** PortSwigger Web Security Academy — intentionally vulnerable, authorized lab  
**Status:** Solved (verified by lab completion banner)  
**Tools:** Burp Suite Community Edition (Proxy, Repeater, Intruder), browser

## 1. Assessment Overview
Assessed the login endpoint for information disclosure caused by distinguishable authentication failure messages. Tested candidate usernames with a constant password and compared the application's responses.

## 2. Testing Methodology
1. Captured an HTTP `POST /login` request and verified its behavior in Burp Repeater.
2. Sent the login request to Burp Intruder, configured a single username payload position, and loaded the Academy-provided candidate username list.
3. Submitted the candidate requests and inspected response status, length, and response bodies.
4. Identified differing application messages for invalid usernames versus valid usernames supplied with an incorrect password.
5. Conducted further password testing in the authorized lab and verified successful login and lab completion.

## 3. Vulnerability Identified
**Username enumeration through distinguishable login responses.**

The login form returned `Invalid username` for a nonexistent account but `Incorrect password` for an existing account with an incorrect password. An unauthenticated requester could therefore establish whether a candidate username exists without knowing its password.

## 4. Proof of Concept and Evidence
- The Intruder results for the username test showed numerous HTTP `200` responses of length `3352`, while one inspected response for the candidate `ai` was length `3354` and contained `Incorrect password`.
- A subsequent Intruder results screenshot from a separate run shows an HTTP `302 Found` response for the payload `daniel`, with `Location: /my-account?id=ai` and a session cookie. This is evidence of a successful authentication response, but the username in the payload and redirect differ; no assertion is made that the redirect identifies the submitted username.
- The final browser screenshot displays `Congratulations, you solved the lab!` and an authenticated account page displaying username `ai`.
- The submitted screenshots capture the username enumeration, successful response, configured Intruder payload lists, and lab completion. **Screenshots are not yet embedded in this repository report.**

## 5. Security Impact
An attacker could reduce the uncertainty of password guessing by first discovering valid accounts. This information disclosure increases the effectiveness of credential-stuffing or password-spraying attempts, especially when other authentication protections are insufficient.

## 6. Remediation Recommendations
- Use consistent, generic failure messages for nonexistent usernames and incorrect passwords.
- Avoid revealing account validity through differing HTTP status codes, response structures, or obvious timing discrepancies.
- Apply risk-based login rate limits, monitoring, and multifactor authentication as defense in depth.
- Test the full authentication workflow to confirm that error handling does not leak account existence through alternate paths.

## 7. Technical Analysis
The weakness is **not** the use of HTTP `200` for unsuccessful login attempts. It is the difference in the information disclosed by unsuccessful responses. The `302` redirect and lab completion are separate evidence of successful authentication, not the underlying enumeration flaw.

## Assessment Scope
This is an authorized PortSwigger Web Security Academy training assessment, not a commercial client engagement.
