# PortSwigger Lab: Race Conditions - Limit Overrun

## Lab Overview
The goal of this lab was to exploit a **race condition vulnerability** by sending multiple requests to the application simultaneously, allowing us to apply the same discount coupon multiple times in a shopping cart. This overcomes the intended limit on how many times a coupon can be applied, enabling us to gain unintended discounts.

## Key Details
### Lab URL:
[PortSwigger Lab: Race Conditions - Limit Overrun](https://portswigger.net/web-security/race-conditions/lab-race-conditions-limit-overrun)

### Target Endpoint:
```http
POST /cart/coupon HTTP/2
```

### Original Request:
```http
POST /cart/coupon HTTP/2
Host: 0ac90093034a116281d0b139006c00bf.web-security-academy.net
Cookie: session=BNFvvvO6v5nSIBzzJPlRXkzAZmWzVZ0j
Content-Length: 52
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="131", "Not_A Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Origin: https://0ac90093034a116281d0b139006c00bf.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ac90093034a116281d0b139006c00bf.web-security-academy.net/cart
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

csrf=BpWbwggguqxYDsqMmn8Dpl7Gg4OrIoMW&coupon=PROMO20
```

### Response:
```http
HTTP/2 302 Found
Location: /cart?couponError=COUPON_ALREADY_APPLIED&coupon=PROMO20
X-Frame-Options: SAMEORIGIN
Content-Length: 22

Coupon already applied
```

## Exploitation Process
To exploit the race condition, we crafted multiple POST requests applying the coupon **PROMO20** and sent them in parallel. By doing so, we bypassed the application's logic that prevents applying the same coupon multiple times.

### Tools Used
- **Burp Suite (Community Edition)**: Intruder module was used to send parallel requests.
- **Turbo Intruder**: For high-speed request fuzzing and testing race conditions.

### Steps:
1. **Intercept the POST Request**:
   - Captured the request using Burp Suite Proxy while applying the coupon PROMO20.
   - Verified that the response indicated "Coupon already applied" when reusing the coupon.

2. **Send Parallel Requests**:
   - Modified the captured request to include the necessary `csrf` token and coupon parameter.
   - Configured Burp Intruder to send the same request simultaneously using the "Pitchfork" attack type.

3. **Observe the Race Condition**:
   - Monitored the responses and observed that some requests successfully applied the coupon multiple times, resulting in an unintended discount.

4. **Verify Exploit Success**:
   - Checked the cart total to confirm the discount was applied beyond the intended limit.

## Lessons Learned
- **Understanding Race Conditions**: A race condition occurs when multiple processes or threads attempt to access and modify shared data simultaneously, leading to unexpected behavior.
- **Practical Exploitation**: Exploiting race conditions often involves sending concurrent requests to achieve undesired behavior, such as bypassing restrictions.
- **Prevention**: Applications can mitigate race conditions by:
  - Implementing proper synchronization mechanisms (e.g., locks).
  - Using unique, server-verified tokens for sensitive actions.
  - Testing endpoints for concurrency issues during development.

## Key Takeaways
- Identifying race conditions requires understanding of backend logic and how state transitions occur.
- Tools like Burp Suite and Turbo Intruder are invaluable for detecting and exploiting these vulnerabilities.
- Proper server-side validation and locking mechanisms are critical to prevent such exploits.

---

![Race Conditions - Limit Overrun Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/race-cond.PNG)

![Race Conditions - Limit Overrun Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/race-cond2.PNG)

![Race Conditions - Limit Overrun Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/race-cond3.PNG)

![Race Conditions - Limit Overrun Lab1](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/race-cond4.PNG)

**Note**: This write-up is for educational purposes only and should not be used for malicious intent. Always seek permission before testing systems for vulnerabilities.

