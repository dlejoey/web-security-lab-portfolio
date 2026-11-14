# Assessment 008 — User ID Controlled by Request Parameter

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Insecure Direct Object Reference (IDOR) / Horizontal Privilege Escalation  
**Environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved

## 1. Assessment overview

**Objective:** Test whether a standard authenticated user could access another user's API key by changing an account identifier in the request.

**Result:** While authenticated as `wiener`, requesting Carlos's account using `id=carlos` exposed Carlos's API key. The key was submitted to the lab and the solved banner was observed.

## 2. Testing methodology

1. Logged into the lab with the provided standard-user credentials for `wiener`.
2. Observed the account page URL `/my-account?id=wiener` and Wiener's own API key.
3. Changed the `id` query parameter from `wiener` to `carlos` while retaining Wiener's authenticated session.
4. Inspected the response in Burp Suite Repeater.
5. Confirmed the returned account data identified `carlos` and included an API key.
6. Submitted the exposed API key through the lab's **Submit solution** function and observed **Congratulations, you solved the lab!**.

## 3. Vulnerability identified

**Finding:** Missing object-level authorization on the account information endpoint.

The server returned a different user's sensitive account data based on a caller-controlled `id` parameter without verifying that the logged-in user was authorized to access that account.

## 4. Proof of concept and evidence

**Authorized account request:**
```http
GET /my-account?id=wiener HTTP/2
Cookie: session=<wiener-session>
```

**Unauthorized cross-account request:**
```http
GET /my-account?id=carlos HTTP/2
Cookie: session=<wiener-session>
```

**Observed result:** The response body displayed `Your username is: carlos` and `Your API Key is:` followed by Carlos's key. The key is intentionally omitted from this report. Submission of that key produced the lab's solved state.

**Evidence status:** Burp Repeater response and solved-state screenshots were observed during the assessment. Image evidence has not yet been uploaded to the repository.

## 5. Security impact

An authenticated user could access another user's private API key, potentially enabling unauthorized use of functionality protected by that key. The flaw crosses a horizontal authorization boundary between peer user accounts.

## 6. Remediation recommendations

- Enforce object-level authorization on every account-data request.
- Determine the account accessible to the caller from the authenticated session or an explicit authorization policy, not merely the supplied `id`.
- Prevent exposure of API keys to other users; minimize display of sensitive secrets.
- Add negative tests verifying that user A cannot retrieve user B's private account data.

## 7. Technical analysis

Authentication established that the requester was `wiener`, but the application used the editable `id` query parameter to select the account data returned. The successful cross-account response and lab completion demonstrated an IDOR vulnerability. The relevant security boundary is ownership of the requested object, not the user's login status.
