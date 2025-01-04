# Exploiting an API endpoint using documentation
# Tackling a Web Application Security Challenge

I recently tackled a fascinating challenge involving web application security and wanted to share how I achieved a successful outcome!

## 🔍 The Challenge:
While working on a web security application, I was tasked with deleting a user account through the API. This required me to perform a `DELETE` request to the endpoint `/api/user/carlos`.

## 🔧 Steps I Took:

### 1. Exploration:
I used **Burp Suite** to intercept and analyze the HTTP requests and responses of the application. This allowed me to gain insights into how the application processes user data and handles requests.

### 2. Crafting the Request:
I constructed a `DELETE` request with the required headers and session cookie to ensure proper authentication and authorization.

### 3. Execution:
The `DELETE` request was sent to the endpoint `DELETE /api/user/carlos` with all necessary headers. The request included session authentication to ensure it was authorized.

### 4. Verification:
After sending the request, I received a response indicating success:
```json
{
  "status": "User deleted"
}
```

This confirmed that the user account was successfully removed from the system.

🔐 Key Takeaway:
Understanding how to leverage tools like Burp Suite for security testing and request manipulation can be incredibly powerful. This experience reinforced the importance of thorough testing and validation in ensuring application security.
