# 🧠 Host Header Injection Labs – Web Security Academy

This repository contains detailed walkthroughs for the Host Header Injection labs offered by PortSwigger's Web Security Academy. These labs demonstrate how attackers can exploit misconfigured or insecure handling of the `Host` header to perform various attacks, such as password reset poisoning, cache poisoning, and more.

## 📌 Labs Covered
| Host header injection                                                                       | Apprentice | [Lab Link](https://portswigger.net/web-security/host-header/exploiting/lab-host-header-injection) |
| Password reset poisoning via malicious Host headers                                          | Practitioner | [Lab Link](https://portswigger.net/web-security/host-header/exploiting/lab-password-reset-poisoning-via-host-header) |
| Password reset poisoning via multiple headers                                                | Practitioner | [Lab Link](https://portswigger.net/web-security/host-header/exploiting/lab-password-reset-poisoning-via-multiple-headers) |
| Password reset poisoning via dangling markup                                                 | Expert      | [Lab Link](https://portswigger.net/web-security/host-header/exploiting/lab-password-reset-poisoning-via-dangling-markup) |


##  What You'll Learn

- How the `Host` header is used internally in applications.
- How to exploit it to poison password reset flows.
- Real-world risks of Host header trust.
- Advanced techniques like using multiple headers and injecting markup.


##  Tools Required

- Burp Suite (Community or Pro)
- Burp Collaborator
- Browser with proxy support
- Custom exploit server (optional for deeper testing)


##  Repository Structure
Each file contains:

- Lab summary  
-  Step-by-step exploitation  
- Key takeaways  
-  Mitigation strategies  


## Security Note

All labs are intentionally vulnerable and meant for educational purposes only. **Do not attempt to replicate these attacks on real-world systems without permission.**


## 📚 References

- [Host Header Injection – PortSwigger](https://portswigger.net/web-security/host-header)
- [OWASP: Unvalidated Redirects and Forwards](https://owasp.org/www-community/attacks/Unvalidated_Redirects_and_Forwards_Cheat_Sheet)
