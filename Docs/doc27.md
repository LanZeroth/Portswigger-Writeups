
# Server-Side Parameter Pollution (SSPP) in Structured JSON Data

## Scenario: Editing a User's Name

1. **Initial Request**: When you want to change your name on a website, the browser sends a request like this:
   ```
   POST /myaccount
   {"name": "peter"}
   ```
   - This is a simple request where only the `name` parameter is being updated to "peter".
   
2. **Server-Side Request**: On the server, it processes your input and sends another request internally:
   ```
   PATCH /users/7312/update
   {"name":"peter"}
   ```
   - The server uses your input to update the "name" field for user ID 7312.

## Attempting SSPP: Adding More Parameters

1. **Modified Request by the Attacker**: Instead of just changing the name, they might modify the request to include a hidden parameter:
   ```
   POST /myaccount
   {"name": "peter","access_level":"administrator"}
   ```
   - Here, `"access_level":"administrator"` is sneaked into the `name` parameter.

2. **Resulting Server-Side Request**: If the server doesn’t handle this properly, it might see:
   ```
   PATCH /users/7312/update
   {"name":"peter","access_level":"administrator"}
   ```
   - This would mean that not only the name was updated, but also a new parameter `access_level` was added, making the user "peter" an administrator.

## Why Does This Happen?
- The server blindly trusts the client input and merges it into its own structured data (in this case, JSON) without checking it properly.
- This is called **Server-Side Parameter Pollution** because extra parameters are "polluting" the server's data.

## Key Takeaways
- **Structured Format Injection** can happen with **JSON**, **XML**, or any structured data.
- It occurs when **user input** is included in a request/response **without proper encoding or validation**.
- To prevent this:
  - Always validate user input.
  - Never trust client-side data.
  - Encode special characters before adding to structured formats.

![Testing Structured JSON ](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/testing-structured3.png)

![Testing Structured JSON ](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/testing-structured2.png)

![Testing Structured JSON ](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/testing-structured.png)
