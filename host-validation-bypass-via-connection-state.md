# Lab: Host Validation Bypass via Connection State Attack

## Description
This lab demonstrates a routing-based SSRF vulnerability through improper host validation across persistent TCP connections. The front-end server validates the first request’s Host header but assumes the same trust level for all subsequent requests on the same connection.

## Objective
Exploit this flawed assumption to access the internal admin panel at `192.168.0.1/admin` and delete user `carlos`.

## Exploitation Steps

1. Send the initial `GET /` request to **Burp Repeater**.

2. Make the following changes:
   - Change the path to `/admin`.
   - Change the `Host` header to `192.168.0.1`.

3. Send the request. You’ll be redirected to the homepage due to access control.

4. **Duplicate the tab** in Repeater and group both tabs.

5. In the **first tab**, make these changes:
   - Path: `/`
   - Host header: `YOUR-LAB-ID.web-security-academy.net`

6. Change the **send mode** (next to the **Send** button) to: send group in sequence ( single connection)
7. Set the `Connection` header to `keep-alive` in both requests.

8. Send the request group. If successful, the second request (to `/admin`) will now access the **internal admin panel**.

9. In the response, find the HTML form used to delete users. Note:
- The form action: `/admin/delete`
- Input name: `username`
- CSRF token value

10. In the **second tab**, replicate the form submission: POST /admin/delete HTTP/1.1
Host: 192.168.0.1
Cookie: lab=YOUR-LAB-COOKIE; session=YOUR-SESSION-COOKIE
Content-Type: application/x-www-form-urlencoded
Content-Length: CORRECT

csrf=YOUR-CSRF-TOKEN&username=carlos

11. Send both requests **in sequence** (keep-alive connection). The second request will be interpreted as authorized due to the trust established in the first.

✅ The lab is solved when Carlos is successfully deleted.

## Impact
This vulnerability allows access to internal systems behind a front-end proxy, potentially bypassing access control. It may lead to SSRF, unauthorized access to admin panels, or data manipulation.

## Mitigation
- Validate the `Host` header on every request, not just the initial one on a TCP connection.
- Do not rely on connection-based assumptions for request-level security.
- Implement proper request segmentation and isolation between front-end and back-end.
- Reject mismatched Host headers or duplicated headers entirely.

## Tags
`ssrf` `host-header` `tcp-connection-state` `burp-suite` `web-security-academy`

## Author
Shivam Singh (vgod)
Penetration Tester & Bug Hunter  
GitHub: https://github.com/vgod-sec
