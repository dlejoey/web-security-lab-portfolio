# Assessment 003 — User Role Controlled by Request Parameter

**Platform:** PortSwigger Web Security Academy  
**Vulnerability:** Broken Access Control (Privilege Escalation)  
**Status:** Solved

## 1. Overview

I tested whether a normal user could access the admin panel by changing a cookie value.

I found that changing `Admin=false` to `Admin=true` allowed me to access the admin panel and delete a user.

## 2. How I Tested It

1. Logged in with the account provided by PortSwigger.
2. Used Burp Suite to check the website's requests and cookies.
3. Found a cookie named `Admin` with the value `false`.
4. Changed it to `true` using Burp Repeater.
5. Accessed `/admin` and saw the list of users.
6. Sent a request to delete `carlos`, the lab's test user.
7. Received a `302` redirect, and PortSwigger confirmed the lab was solved.

## 3. Security Issue

The website trusted a cookie that users could change themselves.

This allowed a normal user to gain admin privileges without being an actual administrator.

## 4. Impact

Someone could access private admin information or perform actions they shouldn't be allowed to, such as deleting user accounts.

## 5. Recommended Fix

The server should check a user's actual permissions before allowing access to admin features.

Changing a cookie should never be enough to gain admin access.

## 6. Evidence

The admin panel was accessed, the test user was deleted, and PortSwigger displayed the successful lab completion message.

Screenshots are pending upload.

*Testing was carried out in PortSwigger's authorized practice environment.*
