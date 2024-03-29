# CORS, HSTS

## CORS

**Cross-Origin Resource Sharing** or **CORS** is a security feature implemented by web browsers to restrict web pages from making requests to a different *origin* than the one that served the page.

An **origin** is a combination of protocol, such as HTTP or HTTPS, domain, and port number.

When a web page loaded from one origin tries to make a request to a resource on a different origin (cross-origin request), the browser typically blocks the request for security reasons. However, there are situations where cross-origin requests are necessary, such as when fetching data from an API hosted on a different server.

CORS allows web servers to specify which origins are allowed to access its resources. This is done through HTTP headers. When a browser makes a cross-origin request, it sends an HTTP request with an `Origin` header indicating the origin of the requesting page. The server then checks this header and decides whether to allow the request based on the configured CORS policy.

If the server allows the request, it responds with additional CORS headers indicating which origins are allowed, how long the response can be cached, and which HTTP methods are allowed for the cross-origin request.

CORS helps prevent certain types of cross-site request forgery (CSRF) attacks by enforcing restrictions on cross-origin requests. It's an important security mechanism for modern web applications that rely on APIs and cross-origin communication.

### Examples

An example of *some* headers sent to server during `OPTIONS` [↑ preflight](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) request:

```text
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: POST
Origin: http://localhost:3001
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-site
```

An example of *some* headers returned by server:

```console
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: content-type
Access-Control-Allow-Methods: POST
Access-Control-Allow-Origin: http://localhost:3001
```

By default, CORS does not include cookies on cross-origin requests.

## HSTS

**HTTP Strict Transport Security** or **HSTS** is a web security policy mechanism that helps to protect websites against certain types of attacks, particularly those that involve the interception of secure communication channels.

When a website implements HSTS, it tells web browsers that it should only be accessed using HTTPS (HTTP over SSL/TLS). This means that even if a user types "http://" in the address bar, the browser will automatically convert it to "https://" before making the request to the server.

HSTS helps to prevent attacks like SSL-stripping, where an attacker could downgrade a secure connection to an insecure one. By enforcing HTTPS, HSTS ensures that all communication between the browser and the server is encrypted and secure.

Additionally, HSTS can specify a duration for which the policy is in effect, preventing even the initial insecure connection attempts during that time. This helps to further strengthen security by ensuring that users cannot bypass the secure connection requirement.

[↑ Enforce HTTPS in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/enforcing-ssl).
