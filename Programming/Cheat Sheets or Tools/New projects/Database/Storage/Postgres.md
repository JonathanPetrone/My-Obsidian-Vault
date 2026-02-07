# PostgreSQL with Go (lib/pq or pgx)

**NOTES:**
- Two main drivers: `lib/pq` (stable, maintenance mode) or `pgx` (modern, actively developed)
- `pgx` recommended for new projects - better performance, more features
- Module cache at `~/go/pkg/mod/` shared across projects

### Installation:

**PostgreSQL Server (One-Time):**
```bash
# Homebrew (Mac)
brew install postgresql@16
brew services start postgresql@16

# Verify installation
psql --version
```

**Create Database:**
```bash
# Create database
createdb myapp

# Or via psql
psql postgres
CREATE DATABASE myapp;
\q
```

### New Project Setup (pgx driver):

1. **Add dependency:**
```bash
go get github.com/jackc/pgx/v5
```

2. **Basic usage:**
```go
import (
    "database/sql"
    _ "github.com/jackc/pgx/v5/stdlib"
)

// Connection string format:
// postgres://username:password@localhost:5432/database?sslmode=disable
db, err := sql.Open("pgx", "postgres://postgres:password@localhost:5432/myapp?sslmode=disable")
if err != nil {
    log.Fatal(err)
}
defer db.Close()

// Verify connection
err = db.Ping()
if err != nil {
    log.Fatal(err)
}
```

3. **Initialize schema:**
```go
_, err = db.Exec(`
    CREATE TABLE IF NOT EXISTS tasks (
        id SERIAL PRIMARY KEY,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        title TEXT NOT NULL
    )
`)
```

### Connection String Formats:

**Basic:**
```
postgres://localhost:5432/myapp
```

**With authentication:**
```
postgres://username:password@localhost:5432/myapp
```

**With SSL disabled (development):**
```
postgres://postgres:password@localhost:5432/myapp?sslmode=disable
```

**Production (SSL required):**
```
postgres://user:pass@host:5432/db?sslmode=require
```

**From environment variable:**
```go
import "os"

connStr := os.Getenv("DATABASE_URL")
db, err := sql.Open("pgx", connStr)
```

### PostgreSQL-Specific Features:

**SERIAL vs IDENTITY:**
```sql
-- Old style (still works)
CREATE TABLE users (
    id SERIAL PRIMARY KEY
);

-- Modern style (Postgres 10+)
CREATE TABLE users (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY
);
```

**RETURNING clause:**
```sql
-- Get inserted row back
INSERT INTO users (email) 
VALUES ($1) 
RETURNING id, created_at;
```

**Array types:**
```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    tags TEXT[]
);

INSERT INTO posts (tags) VALUES (ARRAY['go', 'postgres']);
```

**JSON/JSONB:**
```sql
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    data JSONB
);

INSERT INTO events (data) VALUES ('{"user": "john", "action": "login"}');
```

### Using with goose (migrations):
```bash
# Run migrations
goose -dir sql/migrations postgres "postgres://postgres:password@localhost:5432/myapp?sslmode=disable" up

# Check status
goose -dir sql/migrations postgres "postgres://postgres:password@localhost:5432/myapp?sslmode=disable" status
```

### Using with sqlc:

**sqlc.yaml:**
```yaml
version: "2"
sql:
  - engine: "postgresql"
    queries: "sql/queries/"
    schema: "sql/schema/"
    gen:
      go:
        package: "db"
        out: "internal/db"
```

**Query example** (`sql/queries/users.sql`):
```sql
-- name: GetUser :one
SELECT * FROM users
WHERE id = $1 LIMIT 1;

-- name: CreateUser :one
INSERT INTO users (email)
VALUES ($1)
RETURNING *;
```

### Connection Pooling:
```go
db, err := sql.Open("pgx", connStr)
if err != nil {
    log.Fatal(err)
}

// Configure pool
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(5)
db.SetConnMaxLifetime(5 * time.Minute)
```

### Alternative: Direct pgx (without database/sql):

For advanced features (prepared statements, LISTEN/NOTIFY, COPY):
```bash
go get github.com/jackc/pgx/v5
```
```go
import (
    "context"
    "github.com/jackc/pgx/v5"
)

conn, err := pgx.Connect(context.Background(), "postgres://postgres:password@localhost:5432/myapp")
if err != nil {
    log.Fatal(err)
}
defer conn.Close(context.Background())

var greeting string
err = conn.QueryRow(context.Background(), "SELECT 'Hello, Postgres!'").Scan(&greeting)
```

### Updates:
```bash
# Update pgx driver
go get -u github.com/jackc/pgx/v5
go mod tidy
```

### Common PostgreSQL CLI Commands:
```bash
# Connect to database
psql myapp

# List databases
\l

# List tables
\dt

# Describe table
\d table_name

# Execute SQL file
psql myapp < schema.sql

# Dump database
pg_dump myapp > backup.sql

# Restore database
psql myapp < backup.sql

# Drop database
dropdb myapp
```

### Environment Variables Pattern:
```bash
# .env file
DATABASE_URL=postgres://postgres:password@localhost:5432/myapp?sslmode=disable
```
```go
import (
    "database/sql"
    "os"
    _ "github.com/jackc/pgx/v5/stdlib"
)

func main() {
    connStr := os.Getenv("DATABASE_URL")
    if connStr == "" {
        log.Fatal("DATABASE_URL not set")
    }
    
    db, err := sql.Open("pgx", connStr)
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()
}
```

