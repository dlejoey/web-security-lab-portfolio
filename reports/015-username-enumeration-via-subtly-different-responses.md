# Assessment 015 — Username Enumeration via Subtly Different Responses

**Assessment type:** Authentication security  
**Environment:** PortSwigger Web Security Academy — authorized training lab  
**Status:** Solved (browser completion banner verified)  
**Tools:** Burp Suite Community Edition (Repeater, Intruder), browser

## Assessment Overview
Tested the login endpoint for username enumeration through subtle response differences, followed by password testing for an identified account.

## Testing and Evidence
1. Sent `POST /login` to Burp Repeater, then used Intruder to test username candidates while comparing login responses.
2. Used Burp's response extraction and length comparisons to inspect login failure messages. The username-enumeration phase required careful inspection; the final supplied screenshots do not independently establish which precise username-response difference identified the account.
3. Conducted a separate password-payload test for username `akamai`.
4. In the password attack results, most visible candidates returned HTTP `200` and the extracted warning `Invalid username or password.`.
5. The payload `chelsea` (request 83) stood out with HTTP `302`, response length `188`, and no invalid-login warning.
6. The browser confirmed successful authentication at `/my-account?id=akamai`, displayed the username `akamai`, and showed the PortSwigger **Solved** banner.

**Verified lab credentials:** `akamai` / `chelsea` (training environment only).

## Vulnerability and Impact
Username enumeration through subtly different login responses can reveal which account names exist, enabling more targeted password attacks. Successful password testing is a distinct step and does not by itself demonstrate the enumeration discrepancy.

## Remediation
- Make unsuccessful authentication responses consistent across existing and nonexistent accounts, including content and status.
- Assess timing and response-length differences.
- Apply appropriate login rate limiting, monitoring, and multifactor authentication.

## Technical Analysis
An HTTP `200` login response can still represent a failed attempt. In the password phase, `302` was a strong success indicator, independently confirmed by the authenticated account page and solved banner.

## Scope
Intentionally vulnerable PortSwigger Web Security Academy lab; not a commercial client assessment. Screenshots were provided in the conversation but are not embedded in this report.
