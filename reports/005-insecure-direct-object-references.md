# Assessment 005 — Insecure Direct Object References (IDOR)

**Platform:** PortSwigger Web Security Academy  
**Vulnerability:** Broken Access Control (IDOR)  
**Status:** Solved

## 1. Overview

I tested whether I could download another user's private chat by changing the transcript filename.

I found that changing `2.txt` to `1.txt` exposed Carlos's chat log, including his password. I used the password to log into his account and complete the lab.

## 2. How I Tested It

1. Opened the lab's live chat and used **Download transcript**.
2. Found the download request in Burp Suite's HTTP history.
3. Sent `GET /download-transcript/2.txt` to Burp Repeater.
4. Changed the filename from `2.txt` to `1.txt` and sent the request.
5. Read Carlos's chat log and found his exposed password.
6. Returned to the lab website and logged in as Carlos.
7. Saw PortSwigger's **Congratulations, you solved the lab!** message.

## 3. Security Issue

The server let me access another user's private transcript just by changing the filename. It didn't properly check whether I was allowed to read that file.

This is called **IDOR** (Insecure Direct Object References).

## 4. Impact

Someone could read other users' private conversations. If those chats contain passwords or other sensitive details, their accounts could also be at risk.

## 5. Recommended Fix

The server should check that a user has permission to download each transcript. Changing a filename should never be enough to access someone else's private chat.

## 6. Evidence

The modified request exposed Carlos's chat log, and logging in with the exposed password completed the lab. PortSwigger showed the solved confirmation.

Screenshots are pending upload. The password is intentionally not included in this report.

*Testing was carried out in PortSwigger's authorized practice environment.*
