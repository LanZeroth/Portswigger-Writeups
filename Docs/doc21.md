# Injecting Invalid Parameters

When you try "injecting invalid parameters" into a URL, you're essentially trying to add extra information to see how the server handles it. Here's a simple explanation:

## The Basic URL
Suppose you want to search for a user named "peter." The URL might look like this:

GET /userSearch?name=peter&back=/home


Here, `name=peter` is the parameter you're sending to the server, and it's asking the server to look for someone named Peter.

## Injecting Another Parameter
Now, you're trying to add another parameter, which the server might not expect. You can do this by URL-encoding the `&` symbol (which separates parameters). So, you change the URL to:

GET /userSearch?name=peter%26foo=xyz&back=/home


`%26` represents the `&` character, so you're trying to add `foo=xyz` as an extra parameter.

## What Happens on the Server
When the server processes the URL, it might treat this request as if you were saying:

GET /users/search?name=peter&foo=xyz&publicProfile=true



Now, it looks like you added an extra parameter (`foo=xyz`), even though that wasn’t part of the original request.

## Reviewing the Response
If the response you get back doesn't change, it means the server accepted the extra parameter, but either ignored it or didn’t do anything with it. If it does change, then the server might be doing something with this extra parameter.

## Testing Further
To fully understand how the server behaves, you’d need to try other parameters and combinations to see what happens. The goal is to find out if the server mishandles these extra parameters in a way that might expose a vulnerability or unintended behavior.

## Others
Injecting invalid parameters involves adding extra information to a URL to see how the server processes it. By encoding the & symbol (%26), you can introduce another parameter the server may not expect. After sending the request, check the response. If nothing changes, the parameter might be ignored. Further testing is needed to see how the server handles different combinations, which could reveal vulnerabilities.
  
