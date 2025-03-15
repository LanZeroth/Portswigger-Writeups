Here's a **structured write-up** for your LinkedIn post or report:  

---

## 🔓 **Web Security Academy Lab: User ID Controlled by Request Parameter (with Unpredictable User IDs) – Solved!**  

In this lab, I exploited **horizontal privilege escalation** by identifying a user's **GUID** and modifying an API request to access sensitive data. 🕵️‍♂️  

### **📌 Lab Summary**  
The challenge required retrieving **Carlos' API key** by bypassing access controls. However, instead of sequential user IDs, the application used **GUIDs (Globally Unique Identifiers)** for user identification, making enumeration more challenging.  

### **🚀 Solution Approach**  

🔹 **Step 1: Locate Carlos' GUID**  
- While browsing the site, I found **a blog post by Carlos**.  
- The URL contained a **userId parameter**, revealing Carlos' GUID:  
  ```
  https://0a06005d04aba646933fb082001300ed.web-security-academy.net/blogs?userId=2d6f8147-4367-4398-944a-bafd815a24f8
  ```
- I **noted the GUID** (`2d6f8147-4367-4398-944a-bafd815a24f8`) for later use.  

🔹 **Step 2: Perform Horizontal Privilege Escalation**  
- Logged in with the provided credentials: `wiener:peter`.  
- Navigated to my account page, which made the following GET request:  
  ```http
  GET /my-account?id=6cff7129-3655-4258-9223-a3e5c06317ae HTTP/2
  ```
- Modified the `id` parameter, replacing my own GUID with Carlos' GUID:  
  ```http
  GET /my-account?id=2d6f8147-4367-4398-944a-bafd815a24f8 HTTP/2
  ```
- The **server responded with Carlos’ account details**, including his **API key**! 🎯  

🔹 **Step 3: Submit the API Key**  
- Extracted Carlos' **API key** and used it to complete the lab. ✅  

### **💡 Key Takeaways**  
🔸 **Even if user IDs are GUIDs, poor access controls make privilege escalation possible.**  
🔸 **GUIDs are harder to guess but not impossible to obtain through indirect leaks (like blog URLs).**  
🔸 **Always validate user permissions on the backend instead of trusting user-submitted IDs.**  

🔹 **Defensive Measures:**  
✔ Implement **proper authorization checks** on all endpoints.  
✔ Use **session-based authentication** instead of exposing user identifiers in URLs.  
✔ Prevent **IDOR (Insecure Direct Object Reference) attacks** by enforcing **access control lists (ACLs).**  
![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola7.PNG)

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola8.PNG)

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola9.PNG)

🔍 **What’s your approach to preventing IDOR attacks? Let’s discuss!** 🔥  

#BugBounty #WebSecurity #Pentesting #EthicalHacking #IDOR #CyberSecurity