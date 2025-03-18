### **🔓 Exploiting Username Enumeration & Brute-Forcing Login**  

Just completed another **PortSwigger Web Security Academy** lab on **username enumeration via different responses**, leading to full account access! 🚀  

### **✅ Steps to Exploit:**  

🔹 **Step 1: Enumerate Usernames**  
- Sent invalid login attempts via **Burp Intruder** using a **username list**.  
- Noticed a response length difference (**3250 vs. 3248**) indicating a valid username.  
- Identified **ads** as a valid username. 🎯  

🔹 **Step 2: Brute-Force the Password**  
- Used Burp Intruder with a **password list** for the valid username.  
- Found a response with **302 status**, meaning successful login.  
- Password was **computer**.  

🔹 **Step 3: Login & Access Account**  
- Logged in as **ads:computer** and accessed the account page. ✅  

### **🚀 Key Lessons:**  
✔ **Different error messages or response lengths enable username enumeration.**  
✔ **Response-based detection (200 vs. 302) helps identify valid logins.**  
✔ **Brute-forcing is more efficient after identifying a valid username.**  


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola11.PNG)

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola12.PNG)

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola13.PNG)

![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola14.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola15.PNG)





💡 Have you encountered login pages vulnerable to enumeration? Let’s discuss! 🔥  

#BugBounty #WebSecurity #Pentesting #CyberSecurity #EthicalHacking #BruteForce #UsernameEnumeration