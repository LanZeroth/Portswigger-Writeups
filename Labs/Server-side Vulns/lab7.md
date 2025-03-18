### **Bypassing Weak 2FA Implementation** 🔓  

I recently tackled the **PortSwigger Web Security Academy** lab on **bypassing simple 2FA** and successfully accessed a victim’s account **without the verification code!** 🚀  

### **📌 Lab Objective:**  
Bypass the **two-factor authentication (2FA)** mechanism and access **Carlos’s account page.**  

### **✅ Steps Taken:**  

🔹 **Step 1: Understand the 2FA Flow**  
- Logged in as `wiener:peter` and received a **2FA code via email.**  
- Checked the URL structure of **/my-account** after successful authentication.  

🔹 **Step 2: Exploit the Weak 2FA Implementation**  
- Logged out and attempted to log in as **carlos:montoya.**  
- When prompted for a **2FA code**, I **manually changed the URL** from:  
  ```
  https://example.web-security-academy.net/2fa
  ```
  to  
  ```
  https://example.web-security-academy.net/my-account
  ```
- **BOOM!** 🔥 Access granted without entering a code!  

### **🚀 Why This Worked?**  
The **server didn’t properly enforce** the 2FA step—it only checked if the user was authenticated but didn’t require verification **before allowing direct navigation to protected pages.**  

### **🔐 How to Prevent This?**  
✔ **Enforce 2FA at the backend**, not just in the UI.  
✔ Restrict access to sensitive pages **until 2FA verification is complete.**  
✔ Implement **session-based security checks** to validate authentication state.  

💡 **Ever encountered a weak 2FA implementation? Let’s discuss!** 👇  

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola17.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola18.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola19.PNG)

#BugBounty #CyberSecurity #Pentesting #EthicalHacking #WebSecurity #2FA #AccessControl #VulnerabilityResearch


My Account
Your username is: wiener

Your email is: wiener@exploit-0a23003d0446dfb984973b1701c80062.exploit-server.net

https://0a8d00cd047fdf6784c03c82002c00ac.web-security-academy.net/my-account?id=wiener

https://exploit-0a23003d0446dfb984973b1701c80062.exploit-server.net/email