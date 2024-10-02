# PortSwigger Progress: Mass Assignment Lab (14 of 29)

## Lab Completion 🎉

I successfully completed the Mass Assignment lab on PortSwigger. This lab provided valuable insights into identifying hidden parameters in APIs.

## Key Learnings 🧠

### Identifying Hidden Parameters

- When users update their information (e.g., username or email), the API might include hidden fields such as `id` or `isAdmin`.
- By examining both requests and responses, it's possible to spot these hidden parameters.
- This knowledge helps prevent attackers from tampering with sensitive data like admin rights.

## Example Scenario

### User Update Request

A `PATCH /api/users/` request allows users to update their username and email:

```json
{
    "username": "wiener",
    "email": "wiener@example.com"
}
```

### Concurrent GET Request

A concurrent `GET /api/users/123` request returns the following JSON:

```json
{
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "isAdmin": "false"
}
```

### Analysis

This scenario may indicate that the hidden `id` and `isAdmin` parameters are bound to the internal user object, alongside the updated `username` and `email` parameters.

## Implications for Security 🛡️

- Understanding hidden parameters is crucial for preventing unauthorized data manipulation.
- Attackers could potentially exploit these hidden fields if not properly secured.
- Proper API design and security measures are essential to protect sensitive data and user privileges.

## Next Steps 🚀

- Continue exploring API security best practices.
- Learn more about securing hidden parameters in web applications.
- Apply this knowledge to real-world scenarios and penetration testing.

Remember, always use this knowledge ethically and responsibly, and only on systems you have permission to test.
