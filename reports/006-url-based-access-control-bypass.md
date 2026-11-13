# Assessment 006 — URL-Based Access Control Can Be Circumvented

**Platform:** PortSwigger Web Security Academy  
**Vulnerability:** Broken Access Control  
**Status:** Solved

## 1. Overview

I tested whether the website's admin page could be accessed even though direct visits were blocked.

I found that the server accepted an `X-Original-URL` header and let me access the admin panel through a different URL.

## 2. How I Tested It

1. Tried opening `/admin` and received `403 Forbidden`.
2. Sent the request to Burp Repeater.
3. Changed the request path to `/` and added `X-Original-URL: /admin`.
4. Received `200 OK` and confirmed the response showed the admin panel, including the users `wiener` and `carlos`.
5. Sent another request with `GET /?username=carlos` and `X-Original-URL: /admin/delete`.
6. Received `302 Found`, and PortSwigger displayed **Congratulations, you solved the lab!**

## 3. Security Issue

The website blocked the admin URL when it appeared directly in the request, but still processed it when it was supplied through the `X-Original-URL` header.

This let a normal user bypass the protection.

## 4. Impact

Someone could reach admin-only features and perform actions they should not be allowed to, such as deleting users.

## 5. Recommended Fix

The server should check permissions for every admin action, no matter how the URL is supplied. It should not trust user-controlled headers to bypass access checks.

## 6. Evidence

Burp Repeater showed `403 Forbidden` for direct access, `200 OK` for the header-based request, and `302 Found` after the delete request. PortSwigger confirmed the lab was solved.

Screenshots are pending upload.

*Testing was carried out in PortSwigger's authorized practice environment.*
