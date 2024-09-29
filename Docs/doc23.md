# Overriding Existing Parameters in URLs

To test if an application is vulnerable to **server-side parameter pollution**, you can try overriding an existing parameter by sending the same parameter twice.

## Example Query String

You could modify the query string like this:

GET /userSearch?name=peter&name=carlos&back=/home


This sends two `name` parameters to the server: `name=peter` and `name=carlos`. The impact of this depends on how the server processes these duplicates, and this behavior varies based on the technology used.

## How Different Technologies Handle Duplicate Parameters

- **PHP**: Only the **last** parameter is used, so the server would search for "carlos."
- **ASP.NET**: It **combines** both parameters, resulting in "peter,carlos." This might cause an error.
- **Node.js / Express**: Only the **first** parameter is used, so the server would search for "peter."

## Potential Exploit

If you're able to override the original parameter, you might be able to exploit this. For example, adding `name=administrator` could trick the server into logging you in as the administrator.

This type of test helps uncover how the server handles multiple identical parameters and whether it's vulnerable to parameter pollution.

![Invalid Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/override-params1.png)

![Invalid Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/override-params2.png)