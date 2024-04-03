Objectifs:
- learning about security on high level
- Security context
- authentication
- authorization

![[Screenshot 2024-04-03 at 09.10.15.png]]`

Imagine attempting to enter a military base:

- You must present your identity card to the security guard for entry.
- Once the guard verifies your identity, you receive an access card containing information such as expiration date, granting you access through the gate.
- To enter a building, you must confirm your permission, as different buildings may have varying requirements.
- Upon expiration of your access card, you must exit the facility.

We have three processes:

1. **Authentication**: Confirming your identity; the security guard verifies your identity and issues an internal access card.
2. **Security Context**: Comprising all relevant identity information for the facility, including permissions to enter buildings.
3. **Authorization**: Ensuring that the security context meets access requirements.


### Authentication & Authorization Flow

![[Screenshot 2024-04-03 at 10.36.14.png]]

When a user logs in, the web server verifies their credentials against the database (e.g., ID and password). If validated, the web server generates a Security Context and returns it to the browser, representing the authentication process. Subsequently, the browser stores this authentication to determine whether the user is logged in or not (identifying "who you are"), and utilizes it for each request to the web server. Now, it simply needs to deserialize the security context to identify the user. The web server then determines whether the user is authorized to access the requested information or page.

### Quick cover of basics ASP.NET CORE

ASP.NET  Core is a platform for creating web applications that includes web APIs.

ASP.NET Core provides a robust framework for developing web applications, including web APIs.

Web applications operate through HTTP requests, which carry extensive information:

- **URL**
- **Headers**
- **Body**
- **Other areas**

Both the request and response primarily consist of data exchanged between the front end (browser) and the back end (server).

Within the server, there are designated stages for processing requests and generating responses.

This process involves multiple steps, including:

- **Authentication**
- **Authorization**
- **Exception Handling**
- **Logging**
- **Routing**
- **Validation**

Each of these steps is essential and must be executed effectively to ensure the smooth functioning of the application.

To handle this process effectively, we can employ various software design patterns, one of which is the "chain of responsibility." This involves organizing multiple functions that are interconnected. When a request is made, it passes through these functions sequentially, from the first to the Nth, following a predefined order. Afterwards, the response is returned in reverse order, from the Nth to the first, until it reaches the browser. Each function modifies the message (request) to transform it into a response for the browser. The message is encapsulated within the HTTP context object, containing both the HTTP request and response. This structure is commonly referred to as the ==middleware pipeline==. Each middleware (function/process) within the pipeline assumes different responsibilities. Typically, the final middleware is responsible for processing business logic and may have its own pipeline with specific filters tailored to individual requests.

![[Screenshot 2024-04-03 at 11.10.35.png]]

### Security Context

What is actually the security context ?

The security context contains all the information that the user has for security purpose.

