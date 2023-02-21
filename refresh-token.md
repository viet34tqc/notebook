# Refresh token on the frontend using Axios

<https://www.bezkoder.com/axios-interceptors-refresh-token/>
<https://anonystick.com/blog-developer/authorization-framework-access-token-refresh-token-cung-giong-viec-sinh-vien-thue-nha-tro-2019061161976500>

## Definition of refresh token

- Is a kind of token that is sent along with the first access token, when the user log in the first time
- It's only used for getting new token when the current token expires
- The expiration time for a refresh token is longer than auth token

## How refresh token works

- User send an authorization request
- Server create an access token and refresh token. The expiration of accesss token (ex: 10 minutes) is normally shorter than refresh token (ex: 7 days)
- When user makes another requests that requires token, the token is put into the header of the request
- If the token does not expire yet, the data is return normally
- If the token expires, client will use refresh token to request a new access token
  - If the refresh token is still valid, server will send back a new access token
  - Otherwise, a new authorization is required
 
## Why need both access and refresh token

The access token can be invulnerable and be exploited via cookies, local storage...

## Axios interceptor

- It's like a middleware of Axios request and response
- It allows the client to change the header, params before sending and receiving response