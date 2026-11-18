# Assessment 014 — Username Enumeration via Subtly Different Responses

**Assessment type:** Authentication security  
**Environment:** PortSwigger Web Security Academy — intentionally vulnerable, authorized lab  
**Status:** Solved — confirmed by PortSwigger completion banner  
**Tools:** Burp Suite Community Edition (Repeater and Intruder), browser

## 1. Assessment Overview
Tested the authentication endpoint for username enumeration and followed up with password testing against an identified account in the authorized PortSwigger lab. The lab was ultimately solved by logging in as `akamai`.

## 2. Testing Methodology
1. Captured and replayed `POST /login` requests in Burp Suite.
2. Configured Burp Intruder with a single username payload position and examined candidate responses, including response lengths and extracted login messages.
3. Used the candidate username identified during testing, `akamai`, for a separate password payload run.
4. Compared password-test responses by HTTP status code, length, and extracted login warning.
5. Verified the anomalous response and authenticated account in the browser.

## 3. Vulnerability Identified
**Username enumeration through subtly different authentication responses.**

The lab's authentication behavior allows an attacker to distinguish valid usernames from invalid ones by comparing responses to unsuccessful login attempts. This reduces the uncertainty of subsequent password guessing. The password-testing result is a separate confirmation of successful authentication, not itself proof of the username-enumeration weakness.

## 4. Proof of Concept and Evidence
- During password testing, most visible Intruder responses returned HTTP `200` with the extracted warning `Invalid username or password.`.
- The payload `chelsea` (request 83) produced HTTP `302`, response length `188`, and no extracted invalid-login warning in the results shown.
- The browser then displayed `/my-account?id=akamai`, identified the authenticated user as `akamai`, and showed PortSwigger's **Solved** banner and **Congratulations, you solved the lab!** message.
- The user's earlier username-testing observations informed the assessment; the supplied final screenshots directly substantiate the password-response anomaly and completed login. They do not, by themselves, show the exact subtle username-response difference.
- Evidence screenshots were supplied in the assessment conversation; they are not embedded in this repository report.

## 5. Security Impact
Distinguishable authentication responses can expose valid account names, making password attacks more targeted. If combined with weak passwords and insufficient authentication protections, this may enable unauthorized account access.

## 6. Remediation Recommendations
- Return uniform login-failure messages and avoid differences that disclose whether a username exists.
- Review response status, body, length, and timing for username-dependent discrepancies.
- Apply login rate limits and monitoring, with multifactor authentication as defense in depth.
- Validate the controls against automated enumeration and credential-guessing attempts.

## 7. Technical Analysis
An HTTP `200` does not imply a successful login. In the password test, the `302` response was the standout result; the authenticated account page and PortSwigger completion banner provided stronger confirmation of success. The core vulnerability remains username enumeration via differing unsuccessful-login responses.

## Assessment Scope
This is an authorized PortSwigger Web Security Academy training assessment, not a commercial client engagement.
