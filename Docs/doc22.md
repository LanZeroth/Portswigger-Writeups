# Injecting Valid Parameters in Query Strings

When testing how a server processes URLs, you may be able to modify parts of the **query string** (the part after the `?` in a URL). One technique is to add a second valid parameter to see how the server handles it. This can help identify additional hidden parameters that the server uses internally but doesn’t expose directly.

## Query String Basics

A query string contains parameters in key-value pairs, typically formatted like `?key=value`. These parameters help the server process and respond to requests. For example:
```http
GET /userSearch?name=peter
```

Here, `name=peter` is the parameter being sent to the server.

## Adding a Second Valid Parameter

If you’ve identified that the server accepts another parameter (like `email`), you can inject it into the query string alongside the existing parameter:


Here, `name=peter` is the parameter being sent to the server.

GET /userSearch?name=peter%26email=foo&back=/home


In this case, `%26` is URL encoding for the `&` symbol. This effectively sends both the `name` and `email` parameters to the server in one request.

## Server-Side Request

The server might process your query like this:


GET /users/search?name=peter&email=foo&publicProfile=true


Notice that the server accepted both parameters and might even add its own parameters, such as `publicProfile=true`.

## Analyzing the Response

After submitting the modified request, review the server’s response to understand how it handled the extra parameter. Look for clues such as:
- Whether the server accepted the new parameter.
- Any useful data or error messages returned by the server.

This process helps uncover hidden parameters and provides insight into how the server processes different inputs.



![Invalid Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/valid-params1.png)
![Invalid Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/valid-params2.png)
