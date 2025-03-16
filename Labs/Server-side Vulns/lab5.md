Here’s a **LinkedIn-optimized version** of your write-up:  

---

🔓 **Exploiting IDOR to Disclose an Administrator’s Password**  

I recently completed another **PortSwigger Web Security Academy lab** on **User ID controlled by request parameter (with password disclosure)**. This challenge highlighted a major **security flaw** that can lead to full account takeover! 🚀  

### **📌 Lab Objective:**  
The goal was to retrieve the **administrator's password** and use it to **delete Carlos' account**. The vulnerability stemmed from **IDOR (Insecure Direct Object References)** combined with **password disclosure in a form field**.  

### **✅ Steps Taken:**  

🔹 **Step 1: Inspect Password Autofill**  
- Logged in as `wiener:peter` and accessed the **account settings** page.  
- The password field was **prefilled** (but masked).  
- Using **Inspect Element**, I could see my **plaintext password** in the input field!  

🔹 **Step 2: Exploit IDOR**  
- The account page URL had an **"id"** parameter:  
  ```
  https://example.web-security-academy.net/my-account?id=wiener
  ```
- Changed **wiener** to **administrator**:  
  ```
  https://example.web-security-academy.net/my-account?id=administrator
  ```
- The response contained **the administrator’s password** in plaintext. 🎯  

🔹 **Step 3: Full Account Takeover**  
- Used the **admin credentials** to log in.  
- Navigated to the **admin panel** and **deleted Carlos** to complete the challenge.  

### **🚀 Key Takeaways:**  
🔸 **IDOR combined with sensitive data exposure can lead to account takeovers.**  
🔸 **Prefilling passwords in forms is a dangerous practice.**  
🔸 **Authorization should be enforced server-side, not just via URL parameters.**  

### **🔐 How to Prevent This?**  
✔ **Never prefill passwords** in form fields.  
✔ Use **session-based authentication** instead of exposing user identifiers in URLs.  
✔ Implement **access control checks** to prevent unauthorized users from accessing admin pages.  

💡 **Ever come across a similar vulnerability? Let’s discuss!** 🔥  

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola10.PNG)

#BugBounty #WebSecurity #Pentesting #EthicalHacking #IDOR #CyberSecurity #AccessControl #VulnerabilityResearch