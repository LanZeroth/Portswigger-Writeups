# Testing for Server-side Parameter Pollution (SSPP) in REST Paths

A RESTful API might use the URL path to pass parameter names and values, rather than using the query string. Consider this example path:

/api/users/123

The URL path is broken down as follows:
- **/api**: Root API endpoint.
- **/users**: Resource, in this case, users.
- **/123**: Parameter, representing a specific user’s identifier.

## Testing URL Path Parameters for SSPP

When testing for SSPP, we attempt to manipulate the URL path parameters to exploit the API. Here’s an example scenario:

### Example:
The application allows you to edit user profiles based on the username, using this endpoint:
```http
GET /edit_profile.php?name=peter
```

Internally, the application may generate a server-side request like:
```http
GET /api/private/users/peter
```


### Exploit Path Traversal:

To test for SSPP, you can attempt a **path traversal attack** by submitting URL-encoded values. For instance, submitting `peter/../admin` encoded in the URL might look like this:
```http
GET /edit_profile.php?name=peter%2f..%2fadmin
```

This may result in the following server-side request:


### Possible Exploit:
If the server-side API normalizes this path, it may resolve to:
```http
/api/private/users/admin
```

This would allow the attacker to access the profile or data of the user `admin` instead of `peter`.

### What to Check:
- **Does the application handle path traversal?** If it allows path traversal sequences, the API might be vulnerable to SSPP.
- **Does the server normalize the URL path?** If so, it could resolve an unintended resource like `admin`.

## Conclusion

Testing for SSPP involves manipulating URL path parameters and observing how the server processes them. Look out for path traversal vulnerabilities that could give access to unauthorized resources.

![Server-Side Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/server-side-params.png)


![Server-Side Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/server-side-params2.png)



![Server-Side Params](https://github.com/LanZeroth/Portswigger-Writeups/blob/main/Images/server-side-params3.png)
