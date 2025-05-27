# JWT

- <https://www.youtube.com/watch?v=fyTxwIa-1U0>
- <https://duthanhduoc.com/blog/p3-giai-ngo-authentication-jwt>
- <https://www.youtube.com/watch?v=67mezK3NzpU&t=2493s>

JWT is used for authorization, a replacement for session-cookie model

## Why use JWT over session-cookie model for authorization

<https://viblo.asia/p/authentication-voi-jwt-luu-token-o-dau-la-bao-mat-nhat>

- Session-cookie have to store session data in server (ram, database) and look it up on every request => introduce latency. JWT flow self-contains user data and send back to client

## Downside of JWT

- <https://redis.com/blog/json-web-tokens-jwt-are-dangerous-for-user-sessions>
- <https://duthanhduoc.com/blog/p3-giai-ngo-authentication-jwt>

- Token can be stolen => expiration time of access token should be short => use refresh token
- Token might not be revoked, for example: when we want to log out an user because we cannot delete the token stored in user browser
- Token might be stale, for example if the user role is updated, he can still use the old token to access to the page with the old role

## JWT components

<https://blog.bytebytego.com/p/ep69-explaining-json-web-token-jwt>
  
JWT consists of three components:

- `Header`: stores data of how to create a JWT. It's an object that has token type and the hashing algorithm type
- `Payload`: stores data we want to send to the server. You might want to include as little detail as possible, your best bet is just provide `userID`. Firstly, the data might be stale. Secondly, it's more secure
- `Signature`: calculated from `Header` and `Payload` using HMACSHA256 algorithm by default

Here is an psuedo code of `Signature` calculation

```js
const data = base64urlEncode(header) + “.” + base64urlEncode(payload);
const hashedData = Hash(data, secret);
const signature = base64urlEncode(hashedData);
```

Besides `header` and `payload`, we also have a `secret` string which is provided by developer. This `secret` string must not be exposed.

## How JWT works

- Client send user credentials to server. Server then creates JWT token with secret like above and send back to the client
- Client save the JWT token in cookie (in case web browser)
- For the future requests, Client attaches JWT token in the header
- Server extracts JWT signature to validate if the received signature is made from the same as hashing function and secret string.

JWT only encoded data, not encrypted data. So, the encoded data can be reversed. A 'man-in-the-middle' attack can extract the JWT and decode to get the user information. Please make sure that you are using HTTPS for sensitive data

## Two types of JWT token in authentication

- Access Token: An access token is a short-lived token created by the server, stored on the client, and attached to HTTP requests when the client sends a request to the server. It helps the server authenticate the client.
- Refresh Token: A refresh token has a longer lifespan such as 1 week, 1 month, or 1 year..., is stored in the server's database and on the client, and is used to generate a new access token whenever the access token expires.

**Why does refresh token has longer lifespan than access token, even if we get new pair keys either way?** That's because you have more time to refresh. Even if the user leaves the app open for 20–30 minutes, they can still refresh later without logging in again.

A Refresh Token is a different token generated simultaneously with an Access Token. The Refresh Token has a longer validity period than the Access Token, 

The authentication flow with Access Token and Refresh Token will be updated as follows:

- The client sends a request to the protected resource on the server. If the client is not authenticated, the server returns a 401 Authorization error. The client then sends their username and password to the server.
- The server verifies the provided credentials against the user database. If the credentials match, the server generates two different JWTs: an Access Token and a Refresh Token, containing a payload such as user_id (or another identifier for the user). The Access Token has a short lifespan (around 5 minutes). The Refresh Token has a longer lifespan (about 1 year). The Refresh Token will be stored in the database, while the Access Token will not.
- The server returns the Access Token and Refresh Token to the client.
- The client stores the Access Token and Refresh Token in device memory (cookie, local storage, etc.).
- For subsequent requests, the client includes the Access Token in the request header.
- The server verifies the Access Token using a secret key to check if the Access Token is valid.
- If valid, the server grants access to the requested resource.
- When the Access Token expires, the client sends the Refresh Token to the server to obtain a new Access Token.
- The server checks if the Refresh Token is valid and exists in the database. If valid, the server deletes the old Refresh Token and creates a new Refresh Token with the same expiration date as the old one (e.g., if the old one expires on 10/5/2023, the new one will also expire on 10/5/2023) and stores it in the database, and generates a new Access Token.
- The server returns the new Access Token and new Refresh Token to the client.
- The client stores the new Access Token and Refresh Token in device memory (cookie, local storage, etc.).
- The client can make subsequent requests with the new Access Token (the refresh token process happens silently, so the client will not be logged out).
- When the user wants to log out, they call the logout API, and the server will delete the Refresh Token from the database. Simultaneously, the client must delete the Access Token and Refresh Token from device memory.
- If the Refresh Token expires (or is invalid), the server will deny the client's request, and the client will delete the Access Token and

## Working with `jsonwebtoken` package

You will need two methods, one for generating token after login, the other is verify that token

```ts
// Generate token
// This token will be send to client after logging in successfully
const accessToken = sign({ id: user.id }, jwtSecret, {
	expiresIn: jwtExpiration
});


// Verify token
jwt.verify(token, jwtSecret, (err, decoded) => {
    // If error like wrong toke, expired token...
    if (err) {
      return res.status(401).send({
        auth: false,
        message: err.message
      });
    }

    // Else, save the decoded payload for the next request after successful login
    // The decoded payload should be like this { id: 6, iat: 1676349774, exp: 1676436174 }
    (req as CustomRequest).jwtDecoded = decoded as string | JwtPayload;
    next();
});
```

## Where to store JWT on client-side

<https://www.ducktypelabs.com/is-localstorage-bad/>

A JWT needs to be stored in a safe place inside the user's browser. Any way, you shouldn't store a JWT in local storage (or session storage). If you store it in a LocalStorage/SessionStorage then it can be easily grabbed by an XSS attack. 

Putting your JWT in an httpOnly + secure + sameSite=strict cookie is more secure than putting it in local storage. Why?

- Cookie are automatically sent from browser to server on every requests
- It is not accessible to javascript. This is good, because it means that the token can’t be stolen
- Protections against CSRF (a vulnerability you open yourself up when you store the JWT in a cookie) are easy to add. In addition to the sameSite flag that you’d set on your cookies, you could also set up anti-CSRF protections on the server (most server frameworks have solutions for this)
- Reduces amount of time attackers have to actually exploit the vulnerability
- You'd write slightly less code on the front-end because you can take advantage of browser behavior (submits cookie with requests, server can set expiry, lower risk of sending cookie to server unencrypted and potentially other things)
