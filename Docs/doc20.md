# Truncating Query Strings

When sending a request to a website, you can sometimes trick it by adding a special symbol called `%23`, which is the URL-encoded version of `#`. Normally, `#` is used in URLs to mark the end of important information and anything after it is ignored by the server. However, if you encode it as `%23`, the server might treat it differently and stop processing the request.

## Example

If you search for a user named "peter," you could modify the request like this:

```
GET /userSearch?name=peter%23foo&back=/home
```

If the server treats the `%23` like a normal `#`, it might stop reading the rest of the request, such as the part where it checks if the profile is public. This might allow you to see hidden information about a user that's not normally accessible.

## How it works

The trick is to observe how the server reacts to the `%23`:

- If it shows "peter" in the response, you might have successfully truncated the request.
- If you get an error, it means the server is still reading the rest of the information.

## Summary

Using `%23` can potentially manipulate how the website processes information and expose things you shouldn't normally see. This technique involves interfering with the server's request parsing and may reveal hidden or restricted data.
