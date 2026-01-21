+++
date = '2026-01-03T21:32:16+01:00'
draft = false
title = 'Browser Caching'
+++


## What is browser caching?
Broswer caching is a technique where the web browser stores copies of website files (HTML, CSS, Javacsript, images, etc .) on the user's device so that the website loads faster on future visits.

---

## Use Case
- Faster page loads
- Reduced bandwith
- Lower serve load
- Better user experience

---

## How browser caching works?
Browser follow rules sent by the server using HTTP headers that tell them:
 - What to cache
 - How long to cache
 - Whether to re-check the server

---

## Cache control headers
 - Cache-Control: max-age  -> Cache for X seconds 
 - Cache-Control: no-cache -> Re-check with server before use
 - Cache-Control: no-store -> Don't store anything
 - Expires                 -> Absolute expiration date
 - ETag                    -> Version identifier

---

## Cahing Flow (Simple)
 1. Brwoser -> Server: "Give me the Page"
 2. Server -> Browser: Page + Cache rules
 3. Browser: Saves files locally
 4. Next visit:
	- If fresh -> load from cache
	- If expired -> Ask server

---

## 💡 Final memory trick 
 -  Cache = Speed
 -  No-cache = Freshness
 -  No-store = Security


---

## Clearing cache
Deletes stored files -> forces browser to download fresh content.

---

## Final one line summary
 Browser caching improves website performance by storing static files locally in the browser and reusing them on future visits, reducing load time, bandwidth usage, and server load - while HTTP headers control what gets cached and for how long.
