# Server-side Parameter Pollution (SSPP) and Web App Firewall Bypass Techniques

## What is SSPP?

Server-side Parameter Pollution (SSPP) is a vulnerability where attackers can exploit how a web application handles user input. It occurs when input values (like query parameters, form fields, or headers) aren't properly checked before being sent to another system within the server. Attackers can manipulate these inputs to:

1. **Override Data:** Replace or alter values being sent to the server.
2. **Change App Behavior:** Force the app to perform actions it wasn’t meant to.
3. **Access Unauthorized Data:** Gain access to sensitive data they shouldn't see.

### Common Entry Points:

- **Query Parameters:** The part of the URL that follows a "?" (e.g., `www.example.com/page?param=value`).
- **Form Fields:** Inputs like login forms or search bars that users interact with.
- **Headers:** Hidden information sent along with a web request.
- **URL Path Parameters:** Specific segments in the URL path (e.g., `/user/12345`).

## Why is Input Validation Important?

When user input is not validated, it can lead to security issues like SSPP. Proper checks must be in place to ensure input values are safe and valid before processing them. For example:

- Enforce strict rules for allowed parameters.
- Discard or sanitize unexpected or malicious input.

## Web App Firewall Bypass Techniques

A Web Application Firewall (WAF) protects against many attacks, but some SSPP exploits can bypass it. Attackers might:

- **Use multiple values for the same parameter:** In some cases, the server processes only the first or last value, allowing attackers to slip in malicious input.
  
  Example:  
  `www.example.com/page?param=value1&param=value2`  
  The server might ignore `value1` and process only `value2`.
  
- **Encode input to confuse the WAF:** Techniques like URL encoding (e.g., converting spaces to `%20`) can trick the firewall into letting harmful inputs through.
  
  Example:  
  `www.example.com/page?param=%3Cscript%3Ealert(1)%3C/script%3E`

## Prevention

1. **Validate and Sanitize Input:** Ensure all inputs follow strict validation rules.
2. **Limit Parameter Acceptances:** Restrict the use of duplicate or unnecessary parameters.
3. **Monitor Logs:** Regularly check for suspicious input patterns in web server logs.
4. **Test the WAF:** Continuously test the WAF to ensure it is functioning correctly and handling all input variations.

By securing input points and properly configuring the WAF, SSPP attacks can be minimized.
