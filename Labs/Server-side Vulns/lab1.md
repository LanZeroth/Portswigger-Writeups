Here's a write-up for completing the **"Unprotected admin functionality"** lab:  

---

# **Web Security Academy Write-up: Unprotected Admin Functionality**  

### **Lab Overview**  
This lab has an unprotected admin panel that can be accessed without authentication. The goal is to locate this panel and delete the user **carlos** to solve the lab.  

---

## **Solution Walkthrough**  

### **Step 1: Discovering the Admin Panel**  
1. **Access the lab** by clicking on the provided URL.  
2. Append **/robots.txt** to the URL and visit the page (**https://lab-url/robots.txt**).  
3. The **robots.txt** file typically contains directives for web crawlers, including **Disallow** rules that restrict access to sensitive directories.  
4. In this case, the **Disallow** entry reveals the path to the **admin panel** (e.g., `/administrator-panel`).  

### **Step 2: Accessing the Admin Panel**  
1. Replace `/robots.txt` in the URL with `/administrator-panel` and press Enter.  
2. If the panel is unprotected (i.e., no authentication is required), you will gain access.  

### **Step 3: Deleting the User "carlos"**  
1. Inside the admin panel, look for a **user management** or **delete user** option.  
2. Locate the user **carlos** in the list.  
3. Click on the **delete** button next to carlos.  
4. Confirm the deletion if prompted.  

### **Step 4: Verifying Lab Completion**  
1. If the deletion was successful, the lab should show a success message.  
2. Congratulations! You have successfully solved the lab.  

---

## **Key Takeaways**  
- **robots.txt can disclose sensitive directories**, which attackers can leverage to find unprotected areas.  
- **Lack of authentication on admin panels** is a serious security risk, allowing unauthorized users to perform administrative actions.  
- Always **restrict access to admin panels** by implementing authentication and access control measures.  

This vulnerability highlights the importance of **proper access control** in web applications. Developers should enforce authentication and authorization checks to protect sensitive functionalities.  


![Server-Side Vulnerabilities- Broken Access Control Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/bola.PNG)