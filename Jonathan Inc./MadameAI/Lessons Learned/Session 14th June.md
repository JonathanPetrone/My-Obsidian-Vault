## What We've Learned So Far:

### **Email & Password Validation**

- **HTML5 patterns**: How to use Unicode patterns like `[\p{L}\p{M}\s\-'.]+` for international characters (ö, å, ä)
- **Email reality check**: `örjan@hotmail.com` isn't valid in practice - major providers don't support international chars in local part
- **Go validation**: Updated your functions to use Unicode character classes (`\p{Lu}`, `\p{Ll}`, `\p{N}`)

### **Password Security Deep Dive**

- **Salt vs Cost**: Salt (random data, auto-generated) ≠ Cost (work factor, you set it)
- **bcrypt automation**: You don't manage salts - bcrypt handles everything automatically
- **Hash format**: `$2a$12$salt22chars$hash31chars` - everything embedded in one string

### **Login Flow Architecture**

1. Get input → 2. Check user exists → 3. Verify password with bcrypt → 4. Generate session → 5. Redirect

- **Password verification**: `bcrypt.CompareHashAndPassword()` extracts salt and rehashes
- **Security**: Always return "invalid credentials" regardless of email/password error

### **Go Context Pattern**

- **Purpose**: Cancellation cascade system
- **r.Context()**: Binds to HTTP request lifecycle
- **Why important**: Cancels DB queries if client disconnects, saves resources

### **Session Storage Strategy**

- **Client-only storage**: Major security vulnerability ❌
- **Server-side sessions**: Store token on client, data on server ✅
- **Redis**: In-memory key-value store for performance, but PostgreSQL sessions work fine for your scale

### **Debugging & Format Strings**

- **%d vs %s vs %v**: Use correct format specifiers to avoid weird output