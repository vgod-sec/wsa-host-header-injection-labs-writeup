# 🔐 Lab: Basic Password Reset Poisoning (Apprentice)

This lab demonstrates a basic **Host Header Injection** vulnerability used to poison a password reset link. By modifying the `Host` header, an attacker can capture the password reset token sent to a victim and take over their account.



## 🧪 Lab Objective

> Exploit the Host header to poison the password reset email and gain access to the victim user `carlos`.



## Tools Used

- 🧰 **Burp Suite (Community or Pro)**
- 🌐 **Web browser**
- 🔗 **Exploit Server provided by the lab**


##  Credentials

Use the following credentials to access your own email: wiener : peter


##  Exploitation Steps

### 🔹 Step 1: Trigger your own password reset
- Go to the login page and click **"Forgot your password?"**
- Enter username `wiener` and submit the form.

### 🔹 Step 2: Intercept the password reset request
- In Burp, intercept the `POST /forgot-password` request.
- Send it to **Repeater**.
🔹 Step 3: Observe the Host header
- In the request, locate the default `Host` header (e.g., `Host: your-lab-id.web-security-academy.net`).

### 🔹 Step 4: Modify the Host header
- Change the `Host` value to your exploit server domain: 
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
- Replace the `username` value with `carlos`.
- Send the modified request from Repeater.

### 🔹 Step 5: Capture Carlos's reset token
- Go to the **exploit server** and open the **access logs**.
- You'll see a request to `/forgot-password` with a `temp-forgot-password-token`—this is Carlos's reset token.

### 🔹 Step 6: Reuse the genuine reset link
- In the email client, open the **original password reset email** sent to `wiener`.
- Copy the full reset link.
- Replace the token (`temp-forgot-password-token=...`) with Carlos’s token from the access log.

### 🔹 Step 7: Reset Carlos’s password
- Visit the modified link in your browser.
- Set a new password for Carlos (e.g., `hacked123`).

### 🔹 Step 8: Log in as Carlos
- Go to the login page.
- Use: username:carlos , password....
- - Submit to solve the lab.
##  Key Takeaways

- Applications must not trust user-supplied `Host` headers.
- Hardcoded, validated, or server-side-defined base URLs should be used in emails.
- Host header injection can lead to full account compromise.


##  Mitigation

- Use server-side configuration for trusted base URLs.
- Validate `Host` headers against a whitelist.
- Avoid echoing the `Host` header in security-critical links.


##  Official Lab Link

[Lab: Basic password reset poisoning](https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-basic)
