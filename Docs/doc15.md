# Testing for Mass Assignment Vulnerabilities

Mass assignment vulnerabilities occur when an application allows users to modify sensitive data that they shouldn't have access to. Here's how to test for these vulnerabilities:

## Objective

To check if a user can modify sensitive data, such as admin status.

## Testing Steps

### 1. Send a PATCH Request

Send a PATCH request with the following JSON payload:

```json
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": false
}
```

Check if the application accepts this update.

### 2. Test with Invalid Data

Send another PATCH request with invalid data:

```json
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": "foo"
}
```

If the application responds differently, it might not be handling the `isAdmin` property properly.

### 3. Exploit the Vulnerability

Attempt to exploit the vulnerability by setting `isAdmin` to `true`:

```json
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": true
}
```

If successful, the user 'wiener' could potentially gain admin rights.

## Verification

To verify if the exploitation was successful:

1. Log in as the user 'wiener'
2. Check if you can access admin features

If you can access admin features, the application is vulnerable to mass assignment.

## Implications

- Mass assignment vulnerabilities can lead to privilege escalation
- They allow users to modify data they shouldn't have access to
- This can result in unauthorized access to sensitive features or information

## Mitigation

To prevent mass assignment vulnerabilities:

- Implement strict input validation
- Use whitelisting for allowed properties in user updates
- Separate sensitive data operations from mass assignment processes

Remember to use this information responsibly and ethically, always with the goal of improving application security.
