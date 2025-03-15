# **Web Security Academy Write-up: User Role Controlled by Request Parameter**  

## **Lab Overview**  
This lab demonstrates an **insecure access control mechanism** where **admin privileges** are determined by a **forgeable cookie**. The goal is to **exploit this weakness** to access the **admin panel** and delete the user **carlos**.  

---

## **Solution Walkthrough**  

### **Step 1: Attempt to Access the Admin Panel**  
1. **Navigate to `/admin`** in the browser.  
2. Observe that **access is denied**, indicating that the application verifies admin privileges before granting access.  

---

### **Step 2: Log in as a Regular User**  
1. Go to the **login page** and enter the provided credentials:  
   - **Username:** `wiener`  
   - **Password:** `peter`  
2. Click **Login** and capture the request using **Burp Suite**.  

---

### **Step 3: Modify the Admin Cookie**  
1. In **Burp Suite**, enable **interception** and **response modification**.  
2. After submitting the login form, capture the **server response**.  
3. Look for a **Set-Cookie** header containing:  
   ```
   Set-Cookie: Admin=false
   ```  
4. Modify it to:  
   ```
   Set-Cookie: Admin=true
   ```  
5. **Forward the modified response** to the browser.  

---

### **Step 4: Access the Admin Panel**  
1. Reload `/admin` in the browser.  
2. Since the application now **recognizes your session as an admin**, you should have full access to the panel.  

---

### **Step 5: Delete the User "Carlos"**  
1. Locate **user management** in the admin panel.  
2. Find the user **"carlos"** and click **Delete**.  
3. Confirm the action if prompted.  
4. Once the deletion is successful, the lab should display a **"Congratulations!"** message.  

---

## **Key Takeaways from the Lab**  

✅ **Cookies should not store privilege-related values that can be modified by the client.**  
   - User roles should be **validated on the server-side**, not trusted from a client-controlled parameter.  

✅ **Access control mechanisms should be properly implemented.**  
   - The server should verify **session privileges securely**, instead of relying on **a single cookie flag**.  

✅ **Always validate authentication & authorization securely.**  
   - Role-based access should be enforced via **secure session handling** and **server-side validation** rather than client-modifiable parameters.  

---

## **Real-World Implications**  
This vulnerability is a classic example of **broken access control**, which ranks among the **OWASP Top 10 security risks**. If an attacker can **modify their role** by changing a cookie or request parameter, they can gain **unauthorized access** to sensitive functionalities.  

### **🛡️ Mitigation Strategies:**  
1. **Use secure session management** – Assign roles **server-side** and do not rely on client-controlled parameters.  
2. **Implement proper authorization checks** – Verify user roles on the **backend** before granting access to admin functions.  
3. **Use secure cookie attributes** – Apply `HttpOnly` and `Secure` flags to prevent tampering.  

---

### **Final Thoughts**  
This lab highlights why **client-side role control is insecure** and why **authorization logic must always be enforced on the server**. A small misconfiguration like this can lead to **admin privilege escalation**, making it a **critical vulnerability** in real-world applications.  

🎯 Another step forward in **Web Security & Pentesting!** 🚀  


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola4.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola5.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola6.PNG)
---

Would love to hear your thoughts on **secure access control!** Let’s discuss in the comments. 👇  

#CyberSecurity #BugBounty #WebSecurity #Pentesting #AccessControl #WebSecurityAcademy