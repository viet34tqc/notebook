# JWT

<https://www.youtube.com/watch?v=67mezK3NzpU&t=2493s>

JWT is used for authorization, a replacement for session-cookie model

## Why use JWT over session-cookie model for authorization

<https://viblo.asia/p/authentication-voi-jwt-luu-token-o-dau-la-bao-mat-nhat>

- Session-cookie have to store session data in server (ram, database) and look it up on every request => introduce latency. JWT flow self-contains user data and send back to client

## Downside of JWT

- Token can be stolen
- Token might be stale, for example if the user role is updated, he can still use the old token to access to the page with the old role

## JWT components

<https://blog.bytebytego.com/p/ep69-explaining-json-web-token-jwt>
  
JWT consists of three components:

- `Header`: stores data of how to create a JWT. It's an object that has token type and the hashing algorithm type
- `Payload`: stores data we want to send to the server. You might want to include as little detail as possible, your best bet is just provide `userID`. Firstly, the data might be stale. Secondly, it's more secure
- `Signature`: calculated from `Header` and `Payload`

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

## How to use JWT token in FE request

Conventionally, you need to prefix the token with `Bearer ` and put in `Authorization` header. This is recommendation from JWT team <https://jwt.io/introduction/>

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

## JWT downside

<https://redis.com/blog/json-web-tokens-jwt-are-dangerous-for-user-sessions>

## Where to store JWT on client-side

<https://www.ducktypelabs.com/is-localstorage-bad/>

A JWT needs to be stored in a safe place inside the user's browser. Any way, you shouldn't store a JWT in local storage (or session storage). If you store it in a LocalStorage/SessionStorage then it can be easily grabbed by an XSS attack. 

Putting your JWT in an httpOnly + secure + sameSite=strict cookie is more secure than putting it in local storage. Why?

- Cookie are automatically sent from browser to server on every requests
- It is not accessible to javascript. This is good, because it means that the token can’t be stolen
- Protections against CSRF (a vulnerability you open yourself up when you store the JWT in a cookie) are easy to add. In addition to the sameSite flag that you’d set on your cookies, you could also set up anti-CSRF protections on the server (most server frameworks have solutions for this)
- Reduces amount of time attackers have to actually exploit the vulnerability
- You'd write slightly less code on the front-end because you can take advantage of browser behavior (submits cookie with requests, server can set expiry, lower risk of sending cookie to server unencrypted and potentially other things)
