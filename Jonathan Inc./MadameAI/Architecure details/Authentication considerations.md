# Session based Authentication vs JWT? what are the differences?

## Session-Based Authentication

### How it works:

1. User logs in → Server creates session record in database
2. Server sends session ID via HTTP-only cookie
3. Each request → Browser sends cookie → Server looks up session in DB
4. Session contains user info, expires after time limit

```go
// Login creates session
session := Session{
    ID: "abc123",
    UserID: 42,
    ExpiresAt: time.Now().Add(24 * time.Hour),
}
db.Insert(session)
setCookie("session_id", "abc123")

// Each request checks session
sessionID := getCookie("session_id")
session := db.GetSession(sessionID) // DB query every request
if session.ExpiresAt.Before(time.Now()) {
    // Expired, redirect to login
}
```
### Pros:

- ✅ **Instant logout** - delete session from DB, user is logged out everywhere
- ✅ **Server control** - can revoke sessions anytime
- ✅ **Secure by default** - session data never leaves server
- ✅ **Simple to understand** - traditional web approach
- ✅ **Works great with server-side rendering** (your HTMX approach)

### Cons:

- ❌ **Database hit every request** - impacts performance at scale
- ❌ **Server memory/storage** - sessions stored on server
- ❌ **Harder to scale horizontally** - need shared session store
- ❌ **Doesn't work well for APIs** - cookies don't work great for mobile apps

## JWT (JSON Web Tokens)

### How it works:

1. User logs in → Server creates signed JWT containing user info
2. Server sends JWT to client (cookie, localStorage, or header)
3. Each request → Client sends JWT → Server verifies signature (no DB lookup)
4. JWT contains user info and expiration time
```go
// Login creates JWT
token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
    "user_id": 42,
    "email": "user@example.com",
    "exp": time.Now().Add(24 * time.Hour).Unix(),
})
tokenString, _ := token.SignedString([]byte("secret"))

// Each request verifies JWT
token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
    return []byte("secret"), nil
})
// No database query needed!
```

### Pros:

- ✅ **Stateless** - no server storage needed
- ✅ **Scalable** - no database lookup per request
- ✅ **Works across domains** - great for APIs, microservices
- ✅ **Mobile-friendly** - easy to use in mobile apps
- ✅ **Offline validation** - server can verify without external calls

### Cons:

- ❌ **Hard to revoke** - once issued, valid until expiration
- ❌ **Security complexity** - easy to implement incorrectly
- ❌ **Token size** - JWTs are larger than session IDs
- ❌ **No server control** - can't force logout easily
- ❌ **Secret management** - JWT signing secrets are critical

## Real-World Comparison

### Session-Based Example:
```go
// User changes password, invalidate all sessions
db.Exec("DELETE FROM sessions WHERE user_id = ?", userID)
// User is instantly logged out everywhere
```
### JWT Example:
```go
// User changes password... 
// JWTs are still valid until they expire!
// Need blacklist or short expiration times
```