
# Preventing Server-Side Parameter Pollution (SSPP)

To **prevent server-side parameter pollution**, you should follow these key steps:

1. **Use an Allowlist**: This means defining which characters are safe and acceptable in user input. Any characters that aren’t on this list should be encoded (transformed) before they are included in a server-side request.
   
2. **Validate User Input**: Ensure that the data coming from users follows the correct format and structure (e.g., if you're expecting a name, don't allow numbers or special characters).

## Example of Bad Practices

Here’s an example of bad practices where SSPP could happen:

### Bad Example:

```js
// A simple API request to update user information
app.post('/updateUser', (req, res) => {
    const userData = req.body;
    
    // No validation, trusting user input
    updateUserInDatabase(userData); 
    res.send('User updated!');
});
```

In this example:
- **No validation**: The server blindly trusts whatever data the user sends. A malicious user could send something like:
  
  ```json
  {"name": "John", "access_level": "admin"}
  ```

- Since there’s no input validation, this request could give the user admin access.

## Example of Good Practices

Let’s fix the issues with **good practices**:

### Good Example:

```js
app.post('/updateUser', (req, res) => {
    const userData = req.body;
    
    // Allowlist: Only accepting certain fields (e.g., name)
    const allowedFields = ['name'];
    
    // Creating a new object with only allowed fields
    const sanitizedData = {};
    allowedFields.forEach(field => {
        if (userData[field]) {
            sanitizedData[field] = userData[field]; // Assign safe data
        }
    });
    
    // Additional validation: Make sure 'name' only contains letters
    if (!/^[a-zA-Z]+$/.test(sanitizedData.name)) {
        return res.status(400).send('Invalid input');
    }
    
    updateUserInDatabase(sanitizedData); 
    res.send('User updated!');
});
```

In this example:
- **Allowlist**: We only accept the `name` field from the user's input. Anything else (like `access_level`) will be ignored.
- **Input validation**: We make sure the `name` field only contains letters (using a regular expression) before updating the database.

This ensures that malicious users can’t add unexpected parameters and keeps the app safe!
