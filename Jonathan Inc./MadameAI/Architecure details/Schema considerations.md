### Schema Management Options

#### 1. Raw SQL + Manual Migrations
```sql
-- 001_create_users.sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL
);

-- 002_create_sessions.sql  
CREATE TABLE sessions (
    id VARCHAR(255) PRIMARY KEY,
    user_id INTEGER REFERENCES users(id)
);
```
- You write SQL files and run them manually
- Simple but error-prone at scale
#### 2. Migration Tools (recommended)
```bash
# golang-migrate, goose, atlas, etc.
migrate create -ext sql -dir migrations create_users
migrate up
```
- Tracks which migrations have run
- Can rollback changes
- Version control for schema changes
#### 3. ORM with Migrations
```go
// GORM auto-migration
db.AutoMigrate(&User{}, &Session{})

// Or manual GORM migrations
type User struct {
    ID    uint   `gorm:"primaryKey"`
    Email string `gorm:"uniqueIndex"`
}
```
#### 4. Schema-First Tools (like Atlas, sqlc with schema)
```sql
-- schema.sql
CREATE TABLE users (id SERIAL PRIMARY KEY, ...);
```
- Tool generates migrations from schema differences

---

### Query Building Options

#### 1. Raw SQL
```go
rows, err := db.Query("SELECT id, email FROM users WHERE email = $1", email)
```
#### 2. Query Builders (Squirrel, etc.)
```go
sql, args, _ := squirrel.Select("id", "email").
    From("users").
    Where(squirrel.Eq{"email": email}).
    ToSql()
```
#### 3. Code Generators (sqlc, sqlboiler)
```sql
-- queries.sql
-- name: GetUser :one
SELECT id, email FROM users WHERE email = $1;
```
```go
// Generated code
user, err := queries.GetUser(ctx, email)
```
#### 4. ORMs (GORM, Ent, etc.)
```go
var user User
db.Where("email = ?", email).First(&user)
```
#### Database Schema Considerations:
- User accounts and authentication
- Reading orders (payment status, reading type, questions asked)
- Completed readings (linked to users, with access controls)
- Reading types/pricing (what kinds of spreads you offer)