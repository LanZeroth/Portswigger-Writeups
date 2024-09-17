API Testing – Identifying Supported HTTP Methods 💻
Overview
Today, I focused on testing the HTTP methods supported by API endpoints. HTTP methods define what actions the API allows on its resources, such as retrieving, updating, or deleting data. Identifying and testing these methods is important, as it can reveal additional functionality and sometimes expose unintended vulnerabilities.

1. Key HTTP Methods
Different HTTP methods serve different purposes when interacting with an API. Here are the key methods I tested:

GET: This method retrieves data from a resource. It’s typically used for reading data without making any modifications to the server.

Example: /api/tasks could allow a GET request to return a list of all tasks.

PATCH: This method applies partial updates to a resource. It’s used when you only need to change certain fields in an object without sending the entire dataset.

Example: /api/tasks/1 might allow a PATCH request to update the status of a specific task.

OPTIONS: This method is particularly useful during testing because it can retrieve the allowed HTTP methods for a given resource. Sending an OPTIONS request to an endpoint provides information about the supported actions, such as whether the API allows GET, POST, PUT, or DELETE requests.

Example: Sending an OPTIONS request to /api/tasks would return a list of the methods you can use on that endpoint.

2. Benefits of Testing Multiple HTTP Methods
When testing APIs, trying different HTTP methods is crucial because it helps to:

Discover Hidden Functionality: Some APIs may not publicly document all the methods they support. Testing each method manually or using tools like Burp Intruder can reveal additional actions the API allows.

For example, while an API might publicly advertise support for GET and POST, it could also support DELETE, which might allow unauthorized data removal if not properly secured.

Expand the Attack Surface: By testing different methods, we can find additional functionality that may not be properly secured. An endpoint may handle GET requests securely, but could be vulnerable to improper use of PATCH or DELETE requests.

Example: An API endpoint like /api/tasks may support the following:

GET: Retrieves a list of tasks.
POST: Creates a new task.
DELETE: Deletes a specific task (e.g., /api/tasks/1 deletes task 1).
3. Using Burp Intruder for HTTP Method Testing
Burp Intruder makes it easy to cycle through various HTTP methods automatically. By setting up a list of common HTTP methods (GET, POST, DELETE, PATCH, etc.), I was able to test how the API reacts to each one. This technique speeds up the process and helps identify the methods an API allows without having to manually test each method one by one.

However, caution is necessary during testing. I focused on low-priority objects (such as non-critical or temporary data) to avoid causing unintended disruptions like data loss or excessive record creation.

Key Takeaway
The main lesson from today's testing is that exploring different HTTP methods can reveal a broader attack surface in an API. Methods that seem harmless at first glance, like PATCH or OPTIONS, can provide valuable information about the API’s functionality or open new avenues for further testing.

In summary:

Test multiple HTTP methods (GET, POST, DELETE, etc.) on every API endpoint.
Use Burp Intruder to automate testing and save time.
Focus on low-priority objects to minimize the risk of unintentional changes during testing.
By following these steps, we can uncover hidden API behavior and ensure a more thorough security assessment.

