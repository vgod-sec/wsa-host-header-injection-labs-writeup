# Lab: Host Header Authentication Bypass

## Description
This lab demonstrates a vulnerability where the server makes access control decisions based on the value of the HTTP `Host` header. By manipulating the `Host` header, an attacker can gain unauthorized access to restricted functionality.

## Objective
Bypass admin panel authentication based on Host header manipulation and delete the user `carlos`.

## Exploitation Steps

1. **Access the Lab** and observe that the home page loads successfully.

2. **Open Burp Suite** and intercept a normal GET `/` request.
   - Send this request to **Burp Repeater**.
   - Modify the `Host` header to any arbitrary value (e.g., `Host: attacker.com`) and observe that the page still loads fine.
   - This indicates the server does not validate the Host header correctly.

3. **Discover admin panel path**:
   - Visit `/robots.txt` and observe that it disallows access to `/admin`.

4. **Access `/admin`**:
   - Try opening `/admin` directly in the browser.
   - You will receive an error saying access is only allowed from localhost.

5. **Bypass restriction**:
   - Send a GET request to `/admin` in **Burp Repeater**.
   - Change the `Host` header to `localhost`:
     ```
     GET /admin HTTP/1.1
     Host: localhost
     ```
   - Send the request.
   - You will now have access to the admin panel.

6. **Delete user carlos**:
   - Change the request to:
     ```
     GET /admin/delete?username=carlos HTTP/1.1
     Host: localhost
     ```
   - Send the request.
   - You should get a confirmation, and the lab will be marked as solved.

## Impact
If a web application uses the `Host` header to make security decisions (e.g., granting admin access based on Host value), it can be trivially bypassed by an attacker. This can lead to unauthorized access to administrative functionality and critical actions like user deletion.

## Mitigation
- Never rely on client-supplied headers (like `Host`) to make access control or authentication decisions.
- Use secure server-side logic to enforce access controls.
- Validate the `Host` header against a whitelist or expected value at the server side, especially when generating URLs or granting access based on it.

## Tags
`host-header-injection` `authentication-bypass` `web-security-academy` `burp-suite` `bug-bounty`

## Author
Shivam Singh (vgod) | GitHub: https://github.com/vgod-sec | Bug Hunter & Pentester
