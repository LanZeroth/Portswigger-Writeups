Here’s the full write-up for **"Unprotected Admin Functionality with Unpredictable URL"** lab:  

---

# **Web Security Academy Write-up: Unprotected Admin Functionality with Unpredictable URL**  

### **Lab Overview**  
This lab features an **unprotected admin panel** that is not located at a predictable path (such as `/administrator-panel`). Instead, its location is dynamically set within the application and can be found somewhere in the source code.  

The objective is to **find the hidden admin panel and delete the user "carlos"** to complete the lab.  

---

## **Solution Walkthrough**  

### **Step 1: Investigate the Page Source**  
Since the admin panel is at an **unpredictable location**, we need to **analyze the page source code** to find clues:  

1. Open the **lab homepage** in your browser.  
2. Right-click anywhere on the page and select **View Page Source** (or press `Ctrl + U`).  
3. Alternatively, use **Developer Tools (`F12`)** or **Burp Suite** to inspect the source.  

---

### **Step 2: Identify the Hidden Admin Panel URL**  
Upon reviewing the **HTML source**, we find the following JavaScript snippet:  

```javascript
var isAdmin = false;
if (isAdmin) {
   var topLinksTag = document.getElementsByClassName("top-links")[0];
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-71gvrp');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
   var pTag = document.createElement('p');
   pTag.innerText = '|';
   topLinksTag.appendChild(pTag);
}
```

🔍 **Analysis of the Code:**  
- The `isAdmin` variable is set to **false**, meaning the admin panel link is **not displayed** to normal users.  
- However, the script **hardcodes the admin panel URL** (`/admin-71gvrp`).  
- The URL is still present in the source code even though it’s not visible on the webpage.  

---

### **Step 3: Access the Admin Panel**  
1. Copy the discovered path **`/admin-71gvrp`**.  
2. Append it to the lab’s base URL:  

   ```
   https://lab-url/admin-71gvrp
   ```  

3. Press **Enter** to load the page.  

---

### **Step 4: Delete the User "Carlos"**  
Once inside the admin panel:  

1. Look for **user management** functionality.  
2. Locate the user **"carlos"** in the list.  
3. Click **"Delete"** next to the username.  
4. Confirm the deletion if prompted.  

Upon successful deletion, the lab should display a **"Congratulations!"** message, indicating that the challenge is complete. 🎯  

---

## **Key Takeaways from the Lab**  

✅ **Client-side JavaScript can expose sensitive information.**  
   - Even if the admin panel link isn’t displayed, its URL was still present in the **source code**, making it discoverable.  

✅ **Security through obscurity is not a real defense.**  
   - Simply using **unpredictable URLs** does not protect sensitive admin panels. **Proper authentication is required.**  

✅ **Always implement access control on the server-side.**  
   - The admin panel should have required authentication instead of assuming non-admins won’t find the link.  

---

### **Real-World Implications**  
This vulnerability is common in **poorly secured web applications** where:  
- **Hidden admin panels** are used instead of **proper authentication**.  
- Sensitive URLs are **exposed in JavaScript** or **comments in the source code**.  
- Developers assume **users won’t inspect the page source**—a dangerous assumption.  

**🛡️ Mitigation Strategies:**  
1. **Enforce authentication & authorization** on all sensitive admin functionalities.  
2. **Do not expose sensitive URLs in JavaScript or client-side code.**  
3. **Use proper role-based access control (RBAC)** to restrict unauthorized access.  

---

### **Final Thoughts**  
This lab highlights why **client-side security is never enough** and why **hidden admin URLs do not provide real security**. A **strong authentication mechanism** and **server-side access control** are **essential** to prevent unauthorized access.  

🎯 Another step forward in **Web Security & Pentesting!** 🚀  

---


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola2.PNG)


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola3.PNG)

Would love to hear your thoughts on securing admin panels! Let’s discuss in the comments. 👇  

#CyberSecurity #BugBounty #WebSecurity #Pentesting #AccessControl #WebSecurityAcademy