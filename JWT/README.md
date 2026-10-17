# What is JWT (JSON Web Token) and How Does It Work?

In modern web applications, user authentication and authorization are critical components. Traditionally, web applications relied on **session-based authentication**, where user sessions were stored on the server side. However, as applications scaled and decoupled architectures (Single Page Applications and microservices) became mainstream, **stateless authentication** became the preferred approach.

This is where **JWT (JSON Web Token)** comes in. In this article, you can find what a JWT is, its internal structure, how stateless authentication works, and best practices for securing tokens.

---

## What is a JSON Web Token (JWT)?

A **JSON Web Token (JWT)** is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact, self-contained method for securely transmitting information between parties as a JSON object. 

Because tokens are digitally signed by the issuing server, the receiver can verify their authenticity and integrity without needing to query a central session store.


```

+-----------------------------------------------------------------------+
|                              JWT Token                                |
+-------------------------------+-------------------+-------------------+
|            Header             |      Payload      |     Signature     |
|   eyJhbGciOiJIUzI1Ni...       | eyJzdWIiOiIx...   | SflKxwRJSMeKKF... |
+-------------------------------+-------------------+-------------------+

```

---

## The Structure of a JWT

A JWT string consists of three distinct parts separated by dots (`.`): `Header.Payload.Signature`

### 1. Header

The header typically consists of two parts: the type of token (`JWT`) and the signing algorithm being used, such as **HMAC SHA256** (`HS256`) or **RSA** (`RS256`).

```json
{
  "alg": "HS256",
  "typ": "JWT"
}

```

This JSON is then **Base64URL encoded** to form the first part of the token.

### 2. Payload

The payload contains **claims**—statements about an entity (typically, the user) and additional metadata. Claims fall into three categories:

* **Registered Claims:** Predefined, recommended claims such as `iss` (issuer), `sub` (subject/user ID), `exp` (expiration time), and `iat` (issued at).
* **Public Claims:** Custom claims defined by developers (e.g., `role`, `email`).
* **Private Claims:** Custom claims created to share information between agreed-upon parties.

```json
{
  "sub": "1234567890",
  "name": "Jane Doe",
  "role": "admin",
  "iat": 1700000000,
  "exp": 1700003600
}

```

The payload is also **Base64URL encoded** to form the second part of the token.

> **Crucial Note:** Base64URL encoding is **not encryption**. Anyone who inspects the token can easily decode the payload and read its contents. Never place sensitive information—such as passwords, credit card numbers, or personally identifiable data—inside a JWT payload.

### 3. Signature

To create the signature part, the server takes the encoded header, the encoded payload, and a secret key (or private key), then hashes them using the algorithm specified in the header:

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret_key
)

```

The signature guarantees that the message wasn't altered along the way. If a malicious user attempts to modify the payload (for example, changing `"role": "user"` to `"role": "admin"`), the server's signature check will fail because they do not possess the server's secret key.

---

## The JWT Authentication Flow

Unlike session-based systems that require database lookups on every incoming request, JWT enables stateless authorization.

```
 [ Client App ]                                       [ API Server ]
       |                                                     |
       | --- 1. POST /login (Username & Password) ---------> |
       |                                                     |
       |                                             [ Verify Credentials ]
       |                                             [ Sign JWT Token ]
       |                                                     |
       | <-- 2. 200 OK (Returns Access Token) -------------- |
       |                                                     |
 [ Store Token ]                                             |
 (e.g., Memory)                                              |
       |                                                     |
       | --- 3. GET /protected-route -----------------------> |
       |        Header: Authorization: Bearer <TOKEN>        |
       |                                                     |
       |                                             [ Verify Signature ]
       |                                             [ Check Expiry ]
       |                                                     |
       | <-- 4. 200 OK (Protected Data) -------------------- |

```

### Step-by-Step Breakdown

1. **Authentication:** The client sends user credentials to the authentication server (`/login`).
2. **Token Issuance:** Upon successful authentication, the server generates a JWT using its secret key and returns it to the client.
3. **Storage & Request:** The client stores the token (ideally in memory or secure storage) and includes it in the `Authorization` header of subsequent requests using the `Bearer` scheme:
```http
Authorization: Bearer <your_jwt_token_here>

```


4. **Validation:** The API server receives the request, extracts the token, verifies its signature using the secret key, and checks if the token has expired (`exp`). If valid, the server serves the request without querying a session database.

---

## Session-Based vs. JWT Authentication

| Feature | Session-Based Authentication | JWT (Stateless) Authentication |
| --- | --- | --- |
| **State Location** | Server Memory / Database / Redis | Client-Side (Inside the token itself) |
| **Scalability** | Harder to scale (requires shared session storage across server nodes) | Effortless horizontally scalable (any node with the key can verify) |
| **Revocation** | Easy (delete the session record from DB) | Hard (tokens remain valid until they expire) |
| **Payload Size** | Small (transmits only a session ID) | Larger (carries claims, header, and signature) |

---

## Best Practices and Security Guidelines

1. **Keep Expiration Times Short:** Because stateless JWTs cannot easily be revoked once issued, set short expiration times (e.g., 15 minutes) for access tokens and use **Refresh Tokens** for long-lived sessions.
2. **Use Strong Signing Algorithms:** Avoid weak secrets or algorithms like `none`. Ensure the server strictly validates the signing algorithm specified in the header.
3. **Store Tokens Safely:** Avoid storing sensitive tokens in `localStorage`, as it is vulnerable to **Cross-Site Scripting (XSS)** attacks. Store access tokens in memory and refresh tokens in `HttpOnly`, `SameSite` cookies where possible.
4. **Always Use HTTPS:** Transporting tokens over unencrypted HTTP exposes them to packet sniffing and man-in-the-middle attacks.

---

## Summary

JSON Web Tokens provide an efficient, stateless approach to authentication in modern systems:

* **Compact & Self-Contained:** Contains user identity and claims within the token structure itself.
* **Cryptographically Signed:** Guarantees data integrity using symmetric (HMAC) or asymmetric (RSA/ECDSA) keys.
* **Stateless Efficiency:** Enables seamless horizontal scaling across microservices without centralized session storage.
