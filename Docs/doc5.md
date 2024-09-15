# Identifying API Endpoints

When identifying API endpoints in an application, it's important to manually explore even if you have documentation, as it can be incomplete or outdated. Here’s how this can work, along with examples:

## Example 1: URL Patterns for API Endpoints
- **What to look for:** Common patterns in URLs, such as `/api/`, `/v1/`, or `/data/`.
- **Example:** While browsing an e-commerce website, you notice that when you add an item to your cart, the URL shows `https://example.com/api/add-to-cart`. This suggests that the API is responsible for handling the cart actions. This can be an entry point for further testing.

## Example 2: Reviewing JavaScript Files
- **What to look for:** JavaScript files often contain hardcoded API endpoints that might not be visible through normal browsing.
- **Example:** In the same e-commerce site, you find a JavaScript file named `app.js`. Upon reviewing it in Burp Suite, you discover a line like `fetch('/api/update-cart', ...)`. This API endpoint (`/update-cart`) wasn't triggered directly by your actions in the browser, but you can now manually test it.

## Example 3: Using Burp Suite’s Scanner
- **How it helps:** Burp Suite's scanner automatically crawls the web app, identifying various endpoints and attack surfaces.
- **Example:** Running Burp Scanner on the e-commerce site reveals another API call: `https://example.com/api/remove-item`. This endpoint was never visible during manual browsing but might allow for additional testing, such as checking if unauthorized users can remove items from the cart.

## Example 4: JS Link Finder BApp
- **What it does:** This Burp extension helps extract API endpoints from JavaScript files more thoroughly than manual review.
- **Example:** The JS Link Finder might reveal an API endpoint like `/api/admin/override`, which isn’t used in the public site but is used for administrative functions, providing a potential target for further security testing.

These tools and methods help discover hidden or less obvious API endpoints that may be vulnerable to attack.



