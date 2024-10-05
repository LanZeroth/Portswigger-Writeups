
# Testing for Server-Side Parameter Pollution (SSPP) Using Automated Tools

When testing for vulnerabilities like **Server-Side Parameter Pollution (SSPP)**, tools like **Burp Suite** make the process easier by automating parts of it. Let’s break this down with simple examples:

## 1. Burp Scanner: Detecting Suspicious Input Transformations

Burp Scanner can automatically detect when user input is being processed in unexpected ways. It looks for **"suspicious input transformations"** — this means the application takes what you entered, changes it slightly, and uses it for something else.

### Example: 
- You send the input: `{"name": "peter"}`
- The application turns this into something like `{"name": "Peter", "role": "guest"}` and processes it.

While this change may seem harmless, it could indicate the server isn't handling input safely. The tool flags this behavior for you to investigate further.

> **Important:** Burp Scanner only **flags suspicious behavior**. You’ll still need to manually test if this behavior can be exploited, like in the SSPP example earlier.

## 2. Backslash Powered Scanner BApp: Classifying Inputs

This Burp extension helps identify injection vulnerabilities by classifying inputs into three categories:

- **Boring**: No suspicious behavior detected.
- **Interesting**: Something unusual is happening that could be risky.
- **Vulnerable**: The input clearly shows a potential vulnerability.

### Example: 
- You send the input: `{"name": "peter","access_level":"administrator"}`
- If the server doesn’t block the `access_level` parameter, the scanner might classify this as **"vulnerable"**.
  
You’ll need to check **interesting** inputs further with manual testing to see if they can be exploited (e.g., giving a user admin access).

## Summary:
- **Burp Scanner** automatically detects **suspicious input transformations**, which could be a sign of vulnerability.
- **Backslash Powered Scanner** categorizes inputs, flagging the ones that might be vulnerable for further testing.

Both tools give you a starting point, but **manual testing** is key to confirming real issues.
