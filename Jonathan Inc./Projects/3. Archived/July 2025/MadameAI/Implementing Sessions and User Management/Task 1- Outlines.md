## Password hashing: ✅
- `HashPassword(plaintext string) (string, error)` - bcrypt with appropriate cost **auth.HasPassword**  ✅
- `VerifyPassword(plaintext, hash string) bool` - constant-time comparison **auth.VerifyPassword**  ✅
- `NeedsRehash(hash string) bool` - for upgrading old hashes if needed **auth.NeedsRehash**  ✅

## Password validation rules:  ✅
- Minimum length (8-12+ characters?) -> **auth.ValidatePassword**  ✅
- Character requirements (uppercase, lowercase, numbers, symbols?) -> **auth.ValidatePassword**  ✅
- Blacklist common passwords ("password123", etc.)? ->  **auth.ValidatePassword**  ✅
- Check against user's email/name? -> **auth.ValidatePassword**  ✅

## Implementation Approach:  ✅
- Single `ValidatePassword(password, email string) []string` returning list of violations? **auth.ValidatePassword**  ✅
- Or boolean with detailed error struct?
- Client-side validation rules should match server-side exactly

## Secure Session ID Generation
- Single `ValidatePassword(password, email string) []string` returning list of violations
- Or boolean with detailed error struct?
- Client-side validation rules should match server-side exactly