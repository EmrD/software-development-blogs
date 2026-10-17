# What is CORS (Cross-Origin Resource Sharing)?

In modern web development, front-end applications (React, Vue, Angular) often run on a different domain or port than the back-end API services they communicate with. By default, web browsers block these cross-domain requests for security reasons. 

**CORS (Cross-Origin Resource Sharing)** is an HTTP-header-based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources.

In this article, you can find how the browser enforces origin checks, the difference between simple and preflight requests, and how to configure CORS safely.

---

## The Same-Origin Policy (SOP)

To understand CORS, we must first look at the **Same-Origin Policy (SOP)**. SOP is a critical security mechanism implemented by web browsers to restrict how a document or script loaded from one origin can interact with a resource from another origin.

An **origin** is defined by three components: `Protocol + Domain + Port`.


```

[https://example.com:443/page.html](https://www.google.com/search?q=https://example.com:443/page.html)
│         │        │
Protocol  Domain    Port

```

If any of these three components differ, the browser treats the request as **Cross-Origin**:

| Origin A | Origin B | Same Origin? | Reason |
| :--- | :--- | :--- | :--- |
| `http://example.com/app` | `http://example.com/api` | **Yes** | Same protocol, domain, and port |
| `http://example.com` | `https://example.com` | **No** | Different protocol (`http` vs `https`) |
| `http://example.com` | `http://api.example.com` | **No** | Different subdomain |
| `http://localhost:3000` | `http://localhost:8000` | **No** | Different port (`3000` vs `8000`) |

Without CORS headers from the server, the browser will block the front-end application from reading responses sent from a different origin.

---

## How CORS Works: Simple vs. Preflight Requests

When a web application executes a cross-origin HTTP request (e.g., using `fetch()` or `axios`), the browser evaluates whether the request requires an initial check before sending the actual payload.

### 1. Simple Requests

A request is classified as a **Simple Request** if it meets all of the following conditions:
- Uses safe HTTP methods: `GET`, `HEAD`, or `POST`.
- Uses only standard headers (e.g., `Accept`, `Accept-Language`, `Content-Language`).
- The `Content-Type` header is restricted to:
  - `application/x-www-form-urlencoded`
  - `multipart/form-data`
  - `text/plain`

For simple requests, the browser sends the request directly, appending an `Origin` header. The server processes it and responds with `Access-Control-Allow-Origin`. If the origin matches, the browser lets the application read the response.

### 2. Preflighted Requests

If a request modifies server state or sends custom data—such as sending a `POST` request with a `Content-Type: application/json` payload or passing a custom `Authorization` header—the browser will **not** send the actual request immediately.

Instead, the browser automatically sends an HTTP `OPTIONS` request called a **Preflight Request**.


```

[ Browser / Client ]                                   [ API Server ]
([http://app.example.com](https://www.google.com/search?q=http://app.example.com))                             ([http://api.example.com](https://www.google.com/url?sa=E&source=gmail&q=http://api.example.com))
|                                                     |
| --- 1. OPTIONS /api/resource (Preflight) ---------> |
|        Header: Origin: [http://app.example.com](https://www.google.com/search?q=http://app.example.com)      |
|        Header: Access-Control-Request-Method: POST  |
|        Header: Access-Control-Request-Headers: JSON |
|                                                     |
| <-- 2. 200 OK (Preflight Response) ---------------- |
|        Header: Access-Control-Allow-Origin: ...     |
|        Header: Access-Control-Allow-Methods: POST   |
|        Header: Access-Control-Allow-Headers: ...   |
|                                                     |
| --- 3. Actual POST /api/resource -----------------> |
| <-- 4. 200 OK (Actual Data Response) -------------- |

```

#### Steps of a Preflight Request:
1. **Preflight `OPTIONS` Call:** The browser asks the server for permission, listing the HTTP method (`Access-Control-Request-Method`) and custom headers (`Access-Control-Request-Headers`) it intends to use.
2. **Server Verification:** The server evaluates the origin and requested permissions.
3. **Preflight Response:** The server returns HTTP `200 OK` or `204 No Content` alongside the allowed origins, methods, and headers.
4. **Actual Request:** If the preflight succeeds, the browser sends the actual request payload. If it fails, the browser throws a CORS error in the developer console.

---

## Important CORS Response Headers

Servers use several standard HTTP headers to communicate CORS policies to the browser:

- `Access-Control-Allow-Origin`: Specifies which origins are allowed to access the resource (e.g., `https://app.example.com` or `*` for public APIs).
- `Access-Control-Allow-Methods`: Lists allowed HTTP methods (e.g., `GET, POST, PUT, DELETE, OPTIONS`).
- `Access-Control-Allow-Headers`: Specifies custom headers allowed in actual requests (e.g., `Content-Type, Authorization`).
- `Access-Control-Allow-Credentials`: Indicates whether the browser should expose the response when credentials (cookies, HTTP authentication, or SSL client certificates) are sent.
- `Access-Control-Max-Age`: Defines how long (in seconds) the results of a preflight request can be cached by the browser to reduce extra `OPTIONS` calls.

---

## Common Misconceptions & Security Practices

1. **CORS is Not a Server-Side Firewall:** CORS is enforced by the **browser**, not the server. If you send a request using tools like `curl`, Postman, or a Python script, CORS checks are bypassed entirely.
2. **Avoid Wildcards with Credentials:** Setting `Access-Control-Allow-Origin: *` while simultaneously configuring `Access-Control-Allow-Credentials: true` is invalid and will be rejected by modern browsers. Always explicitly state trusted domains when credentials are involved.
3. **Handle `OPTIONS` Requests explicitly:** Ensure your API routing configuration returns a successful status code (`200` or `204`) for `OPTIONS` requests before requiring authentication middleware.

---

## Summary

Cross-Origin Resource Sharing is an essential bridge between strict browser security and modern web architectures:

- **Same-Origin Policy (SOP)** blocks cross-domain responses by default.
- **CORS** allows servers to grant explicit access permissions to web browsers via HTTP headers.
- **Preflight Requests (`OPTIONS`)** protect back-end servers by verifying method and header safety before executing non-simple requests.
