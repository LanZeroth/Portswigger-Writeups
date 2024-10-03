# Testing for Server-Side Parameter Pollution (SSPP) in JSON

## What is SSPP?
Server-Side Parameter Pollution (SSPP) is when an attacker adds extra parameters (data) to a request to exploit vulnerabilities in the way the server handles this data. For example, the server might not validate all the data properly, allowing the attacker to change important information.

### Example Scenario:
Let's say you are using an app that allows users to edit their profile information (like changing your name). When you submit your changes, the app sends the following request to the server:

**Request to change your name:**
```plaintext
POST /myaccount
name=peter
```
**What happens on the server:**
The server processes this and sends a request like this to update your profile:

```json
PATCH /users/7312/update
{"name": "peter"}
```

This is normal behavior. Your name is updated to "peter."

**Exploit Example:**
Now, what if you try to add another piece of data, such as trying to make yourself an administrator? You might modify the request like this:

**Modified request**
```plaintext
POST /myaccount
name=peter","access_level":"administrator
```
Here, you added access_level="administrator". If the server does not properly validate this input, it might process the new parameter, resulting in the following server-side request:



```json
PATCH /users/7312/update
{"name": "peter", "access_level": "administrator"}

```

**What could happen:**
If the server accepts the extra access_level parameter without checking, you could gain unauthorized privileges. In this example, you might become an administrator, which is a serious security risk.


## How to Prevent SSPP:
*Input validation*: Ensure that only allowed parameters (like "name") are processed.
*Sanitize inputs*: Remove any extra parameters before sending data to the server.

** Note ** here the added payload is
```json
","access_level":"administrator
```
![Testing SSP](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/testing-ssp.png)

![Testing SSP](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/testing-ssp2.png)

