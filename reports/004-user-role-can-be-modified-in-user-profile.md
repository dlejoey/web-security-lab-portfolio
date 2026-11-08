# Assessment 004 — User Role Can Be Modified in User Profile

**Platform:** PortSwigger Web Security Academy  
**Vulnerability:** Broken Access Control (Privilege Escalation / Mass Assignment)  
**Status:** Solved

## 1. Overview

I tested whether a normal user could change their account permissions through the email update feature.

I found that adding `"roleid": 2` to the request gave my account admin access.

## 2. How I Tested It

1. Logged in with the normal user account provided by PortSwigger.
2. Used Burp Suite to find the `POST /my-account/change-email` request.
3. Noticed `"roleid": 1` in the server's response.
4. Sent the email-change request to Burp Repeater and added `"roleid": 2` to its JSON body.
5. Sent the modified request and received a `302` redirect to `/admin`.
6. Checked the browser and saw the Admin panel link and PortSwigger's solved message.

## 3. Security Issue

The website's email update feature also accepted a change to the user's role. A normal user should not be able to give themselves admin permissions.

This is called **mass assignment**: the server accepts changes to fields that users are not meant to control.

## 4. Impact

A normal user could gain admin access and potentially use features meant only for administrators.

## 5. Recommended Fix

The server should only allow users to update fields they have permission to change, such as their email address. Account roles should be changed only through properly protected admin functions.

## 6. Evidence

Burp Repeater showed the modified request and a `302` response. The browser displayed the Admin panel link and the lab's solved confirmation.

Screenshots are pending upload.

*Testing was carried out in PortSwigger's authorized practice environment.*
