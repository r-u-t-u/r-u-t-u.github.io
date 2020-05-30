---
title: Authentication[Part 2] - Token-based authentication
tags: 
- tokens
---

##### <b>Basic Authentication and API keys<b/>
Basic authentication is very simple authentication scheme and built-in with HTTP protocol. The client needs to send requests with Authorization header with `Basic` and base64 encoded string of username and password. 

```ruby
Authorization: Basic ZGVtbzpwQDU1dzByZA==
```
This is secure only if used with HTTPS or SSL.

API Keys - They are token and known only to client and server. For every API call client has to send api-key, it can be in url or header. This is also secure only if used with HTTPS or SSL.

##### <b>Token based authentication</b>
Instead of sending credentials or api-keys for every request, tokens are used. In a few words, an authentication scheme based on tokens follow these steps:

1. The client sends their credentials (username and password) to the server.
2. The server authenticates the credentials and, if they are valid, generate a token for the user.
   So for every request client makes it sends token as type Bearer in authorization header.
```ruby
Authorization: Bearer <token>
``` 
3. The server stores the previously generated token in some storage along with the user identifier and an expiration date.
4. The server sends the generated token to the client.
5. The client sends the token to the server in each request.
6. The server, in each request, extracts the token from the incoming request. With the token, the 7. server looks up the user details to perform authentication.
7. If the token is valid, the server accepts the request.
8. If the token is invalid, the server refuses the request.
9. Once the authentication has been performed, the server performs authorization.
The server can provide an endpoint to refresh tokens.

In above steps we see server is storing tokens. To truely have stateless authentication we can use signed tokens like [JWT(JSON Web Token)](https://jwt.io/introduction/). The JWT has 3 sections header, payload and signature. In header it has type of token and signing algorithm. The payload has user related information. While signature has encoded header and payload, along with secret key to sign it. 


<!-- Authentication - knowing user or any service's identity
Authorization - Verifying grants and access to user or service -->
