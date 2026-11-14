# Assessment 009 — User ID Controlled by Request Parameter, with Unpredictable User IDs

**Platform:** PortSwigger Web Security Academy  
**Vulnerability class:** Broken Access Control / IDOR  
**Environment:** Authorized, intentionally vulnerable lab  
**Status:** Solved (verified)

## Assessment overview

**Objective:** Obtain the API key of another user, `carlos`, where account URLs use unpredictable user identifiers.

**Result:** The target user's identifier was discovered through publicly visible blog content. Substituting that identifier into the account URL exposed Carlos's account information and API key. The lab displayed its solved banner after the key was submitted.

## Testing methodology

1. Logged in to the provided account as `wiener` and inspected the account URL, which contained an unpredictable identifier in the `id` parameter.
2. Browsed the lab's public blog posts and identified a post authored by `carlos`.
3. Followed the author's blog link, which included a `userId` query parameter containing Carlos's identifier.
4. Replaced the account page's `id` parameter with Carlos's identifier.
5. Confirmed the page displayed `Your username is: carlos` and his API key.
6. Submitted the exposed key as the lab solution and observed the **Solved** banner.

## Vulnerability identified

The application used unpredictable identifiers, but did not enforce that the authenticated session was authorized to access the selected account.

**Illustrative request pattern:**

```http
GET /my-account?id=<carlos-user-id> HTTP/2
Host: <active-lab-host>
Cookie: session=<wiener-session>
```

The same identifier was available from a publicly accessible blog-author link, so unpredictability did not prevent discovery.

## Evidence and outcome

Screenshots shared during testing showed the blog-author link, Carlos's account page with an API key, and the lab's **Congratulations, you solved the lab!** banner. The actual key and session cookie are omitted. Screenshot files are not yet attached to this GitHub report.

## Security impact

An authenticated user could retrieve another user's private account information and API credentials. Random-looking identifiers did not compensate for missing object-level authorization.

## Remediation

- Enforce server-side object-level authorization on every account request.
- Do not use unguessable identifiers as the sole access-control mechanism.
- Avoid exposing secrets in account responses to unauthorized users.
- Add cross-account access tests for all user-controlled identifiers.

## Technical analysis

The identifier's format changed the discovery step, not the underlying flaw. Once Carlos's identifier was found in a public blog link, the account endpoint trusted the requested `id` instead of checking ownership against the active session.
