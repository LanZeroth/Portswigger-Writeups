# CSRF - SameSite Strict Bypass via Client-Side Redirect

🚀 Lab Completed: Tackling another web security challenge from PortSwigger's Web Security Academy! This time, it was about bypassing SameSite cookie restrictions set to Strict using a client-side redirect.

## Steps and Insights

### 🔍 Study the Change Email Function
- **Logged in** to my account via Burp's browser and changed the email address.
- Observed the `POST /my-account/change-email` request in Burp's **Proxy > HTTP history** tab. Noticed it lacked unpredictable tokens, making it vulnerable to CSRF if SameSite restrictions are bypassed.
- Confirmed from the **POST /login** response that the website explicitly sets `SameSite=Strict` for session cookies. This prevents the browser from sending these cookies in cross-site requests.

### 🛠️ Identify a Suitable Gadget
- Visited a blog post and posted an arbitrary comment. The browser first loaded a confirmation page at `/post/comment/confirmation?postId=x` before redirecting back to the blog post after a few seconds.
- Checked Burp's proxy history and found that this redirect is controlled by JavaScript in `/resources/js/commentConfirmationRedirect.js`.
- The `postId` query parameter dynamically constructs the redirect path.
- Modified the `postId` parameter in the URL `/post/comment/confirmation?postId=foo` to see how it behaves. This caused the JavaScript to redirect to `/post/foo`.
- Tested path traversal sequences like `/post/comment/confirmation?postId=1/../../my-account` and confirmed it redirected to my account page. This proved the `postId` parameter could elicit GET requests to arbitrary endpoints on the site.

### 🏹 Bypass the SameSite Restrictions
- Created an exploit script on the exploit server to induce a GET request:

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=../my-account";
</script>
```

- Stored and tested the exploit, confirming that the browser included my session cookie in the second request, even when initiated from an external site.

### 💡 Crafting the Exploit
1. Sent the `POST /my-account/change-email` request to Burp Repeater.
2. Right-clicked and used **Change request method** to convert it to a GET request.
3. Confirmed that the endpoint accepted a GET request for changing the email address.
4. Updated the exploit script to redirect to the `change-email` endpoint with the necessary parameters:

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
</script>
```

- URL encoded the ampersand (`&`) to prevent breaking out of the `postId` parameter in the initial setup.
- Tested the exploit on my account to confirm it successfully changed the email address.

### 🎯 Deliver the Payload
- Modified the email address in the exploit to target the victim.
- Delivered the exploit. Once executed by the victim, the lab was solved!

## 🛡️ Key Takeaways
- SameSite cookies, even with Strict settings, aren't foolproof against CSRF. Supplement them with CSRF tokens, Origin headers, and robust request validation.


