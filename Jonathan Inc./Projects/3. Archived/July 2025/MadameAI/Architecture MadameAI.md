### Authentication and User Management
- Authentication Strategy: **Session-Based** ✅
- User Management Approach: **Simple email/password registration** ✅

[[Authentication considerations]]
### Session Storage
- Session Storage Strategy: **Database sessions** ✅
### Database and setup:
- Database to use: **Postgres** ✅
- Database deployment option: **Same server** ✅
- Schema management option: **Migration tool** **(goose)** ✅
- Query building option: **Code Generator** **(sqlc)** ✅

[[Database considerations]], [[Schema considerations]], [[User management considerations]]
### Password Security Details
- Password requirements: ?
- Password reset flow: Email-based reset tokens?
- Account verification: Email verification on signup?
### Session Security
- Session duration: How long before auto-logout?
- Session cleanup: Remove expired sessions automatically? Job for that?
- Multiple sessions: Allow login from multiple devices?
### Database Connection Management
- Connection pooling: How many concurrent DB connections?
- Connection timeouts: What if DB is slow/down?
### Future Auth Features (plan for later)
- Remember me: Longer-lived sessions?
- User profiles: Name, preferences, reading history?
- Admin users: Different permission levels?
- API authentication: JWT for mobile app later?
### Payment Integration:
- Payment Integration: Since users are ordering readings, need to integrate payment processing (Stripe, PayPal) and track order fulfillment status.



