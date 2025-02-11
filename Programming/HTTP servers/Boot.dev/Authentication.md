Authentication is the process of verifying _who_ a user is. If you don't have a secure authentication system, your back-end systems will be open to attack!

Imagine if I could make an HTTP request to the YouTube API and upload a video to _your_ channel. YouTube's authentication system prevents this from happening by verifying that I am who I say I am.

Passwords are a common way to authenticate users.

1. **Storing passwords in plain text is awful.** If someone gets access to your database, they will be able to see all of your users' passwords. If you store passwords in plain text, you are giving away your users' passwords to anyone who gets access to your database.
2. **Password strength matters.** If you allow users to choose weak passwords, they will be more likely to reuse the same password on other websites. If someone gets access to your database, they will be able to log in to your users' other accounts.

### Hashing
On the other hand, we _will_ be writing code to store passwords in a way that prevents them from being read by anyone who gets access to your database. This is called _hashing_. Hashing is a one-way function. It takes a string as input and produces a string as output. The output string is called a _hash_.

The most critical thing we can do to protect our users' passwords is to _never_ store them in plain text. We should use cryptographically strong key derivation functions (which are a special class of hash functions) to store passwords in a way that prevents them from being read by anyone who gets access to your database.

[Bcrypt](https://blog.boot.dev/cryptography/bcrypt-step-by-step/) is a great choice. [SHA-256](https://blog.boot.dev/cryptography/how-sha-2-works-step-by-step-sha-256/) and [MD5](https://en.wikipedia.org/wiki/MD5) are not.

## Types of Authentication

Here are a few of the most common authentication methods you'll see in the wild:

1. Password + ID (username, email, etc.)
2. 3rd Party Authentication ("Sign in with Google", "Sign in with GitHub", etc)
3. Magic Links
4. API Keys

### 1. Password + ID

This is the most common type of authentication that requires a manual login from a user. When users use password managers, it's one of the more secure ways to authenticate users, unfortunately, many users don't, so it's not as secure as it could be.

That said, it's a valid choice.

### 2. 3rd Party Authentication

3rd party authentication is a way to authenticate users using a service like Google or GitHub (similarly to how we do it here on Boot.dev). 3rd party auth is great for user experience because it allows users to use their existing accounts to log in to your app, lowering friction.

It's also nice because you don't need to worry about storing passwords yourself, meaning you can outsource the security of your users' passwords to a company that, _hopefully_, does a good job.

The only real drawbacks to 3rd party auth is that you're trusting a 3rd party and if your users don't have an account with that 3rd party, they won't be able to log in.

### 3. Magic Links

Magic links are a way to authenticate users without a password. It relies on the assumption that the user's email is something that they have unique access to.

The webserver sends a link to the user's email and encodes a unique token in that link. When the user clicks the link, the webserver can decode the token and use it to authenticate the user. Eg:

`https://example.com/login?token=...`

### 4. API Keys

API keys are a fantastic way to authenticate users and systems programmatically. An API Key is just a long, secure string that uniquely identifies a user or system, and that can't be guessed. Because they're intended to be used in code, they don't need to be memorized and, as such, can be much longer and double as an identifier. An API key might look something like this:

`bd_JDS543J3n5NMKspDXNRlowiqw523lKHK32K43kl`

## What Is a JWT?

A JWT is a JSON Web Token. It's a cryptographically signed JSON object that contains information about the user. You'll learn about how the cryptography of JWTs work in our [Learn Cryptography](https://boot.dev/courses/learn-cryptography) course, for now, it's just important to know that once the token is created by the server, the data in the token can't be changed without the server knowing.

![[Pasted image 20250204102548.png]]

# Revoking JWTs

One of the main benefits of JWTs is that they're _stateless_. The server doesn't need to keep track of which users are logged in via JWT. The server just needs to issue a JWT to a user and the user can use that JWT to authenticate themselves. Statelessness is _fast and scalable_ because your server doesn't need to consult a database to see if a user is currently logged in.

However, that same benefit poses a potential problem. JWTs can't be revoked. If a user's JWT is stolen, there's no easy way to stop the JWT from being used. JWTs are just a signed string of text.

The JWTs we've been using so far are more specifically _access tokens_. Access tokens are used to authenticate a user to a server, and they provide _access_ to protected resources. Access tokens are:

- Stateless
- Short-lived (15m-24h)
- Irrevocable

They _must_ be short-lived because they can't be revoked. The shorter the lifespan, the more secure they are. Trouble is, this can create a poor user experience. We don't want users to have to log in every 15 minutes.

## A Solution: Refresh Tokens

Refresh tokens don't provide access to resources directly, but they can be used to get new access tokens. Refresh tokens are much longer lived, and importantly, they _can_ be revoked. They are:

- Stateful
- Long-lived (24h-60d)
- Revocable

Now we get the best of both worlds! Our endpoints and servers that provide access to protected resources can use access tokens, which are fast, stateless, simple, and scalable. On the other hand, refresh tokens are used to keep users logged in for longer periods of time, and they can be revoked if a user's access token is compromised.

# Cookies

HTTP [cookies](https://en.wikipedia.org/wiki/HTTP_cookie) are one of the most talked about, but least understood, aspects of the web.

When cookies are talked about in the news, they're usually implied to simply be privacy-stealing bad guys. While cookies can certainly invade your privacy, that's not what they _are_.

## What Is an HTTP Cookie?

A cookie is a small piece of data that a server sends to a client. The client then dutifully stores the cookie and sends it back to the server on subsequent requests.

Cookies can store any arbitrary data:

- A user's name or other tracking information
- A JWT (refresh and access tokens)
- Items in a shopping cart
- etc.

The server decides _what_ to put in a cookie, and the client's job is simply to store it and send it back.

## How Do Cookies Work?

Simply put, cookies work through HTTP headers.

Cookies are sent from the server to the client in the `Set-Cookie` header. Cookies are most popular for web (browser-based) applications because browsers _automatically_ send any cookies they have back to the server in the `Cookie` header.

## Why Aren't We Using Cookies?

Simply put, Chirpy's API is designed to be consumed by mobile apps and other servers. Cookies are primarily for browsers.

A good use-case for cookies is to serve as a more strict and secure transport layer for JWTs within the context of a browser-based application.

For example, when using [httpOnly cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#block_access_to_your_cookies), you can ensure that 3rd party JavaScript that's being executed on your website can't access any cookies. That's a lot better than storing JWTs in the browser's local storage, where it's easily accessible to any JavaScript running on the page.
