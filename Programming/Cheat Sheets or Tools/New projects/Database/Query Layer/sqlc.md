# sqlc (SQL Code Generator)

**NOTES:** 
- CLI tool installed globally - not a per-project dependency 
- Generates type-safe Go code from SQL queries 
- No runtime dependency - generated code uses stdlib `database/sql` 
### Installation (One-Time): 
```bash
# Homebrew (Mac) 
brew install sqlc 

# Or Go install 
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest 

# Verify installation 
sqlc version 
```

**Current Version:** v1.30.0  **Location:** `~/go/bin/sqlc` 
### New Project Setup: 
1. **Create directory structure:** 
```bash
mkdir -p sql/schema sql/queries
```
 
2. **Create `sqlc.yaml` in project root:** 
 ```yaml 
 version: "2" 
 sql: 
	 - engine: "sqlite" # or "postgresql", "mysql" 
	   queries: "sql/queries/"
	   schema: "sql/schema/" 
	   gen: 
		   go: 
			   package: "db" 
			   out: "internal/db"
 ```
 
3. **Create schema file** (`sql/schema/001_schema.sql`): 
```sql
CREATE TABLE users ( 
	id INTEGER PRIMARY KEY AUTOINCREMENT, 
	email TEXT NOT NULL UNIQUE, 
	created_at TEXT DEFAULT CURRENT_TIMESTAMP
	); 
```

 4. **Create query file** (`sql/queries/users.sql`): 
```sql
-- name: GetUser :one 
SELECT * FROM users WHERE id = ? LIMIT 1; 

-- name: ListUsers :many 
SELECT * FROM users ORDER BY created_at DESC; 

-- name: CreateUser :one
INSERT INTO users (email) VALUES (?) RETURNING *;

-- name: DeleteUser :exec
DELETE FROM users WHERE id = ?; 
``` 

5. **Generate Go code:** 
```bash
sqlc generate
``` 
This creates type-safe functions in `internal/db/`: 
- `queries.sql.go` - Generated query methods 
- `models.go` - Go structs matching your tables 
- `db.go` - Interface and setup code 

6. **Use generated code:** 
```go
import ( 
	"database/sql" 
	"yourproject/internal/db" 
	_ "github.com/ncruces/go-sqlite3" 
) 

database, err := sql.Open("sqlite3", "app.db")
if err != nil { 
	log.Fatal(err)
}

queries := db.New(database) // Type-safe queries 

user, err := queries.GetUser(ctx, 1)
users, err := queries.ListUsers(ctx)
newUser, err := queries.CreateUser(ctx, "user@example.com")
err = queries.DeleteUser(ctx, 1)
``` 

### Query Annotations: 
- `:one` - Returns single row (error if 0 or >1 results) 
- `:many` - Returns slice of rows 
- `:exec` - Returns no data (INSERT/UPDATE/DELETE)
- `:execrows` - Returns rows affected count 
- `:execresult` - Returns `sql.Result` 
### Workflow: 
1. Write/modify SQL in `sql/queries/` or `sql/schema/` 
2. Run `sqlc generate` 
3. Use generated functions in your code 
4. Regenerate whenever SQL changes
### Updates: 
```bash
# Update sqlc binary brew upgrade
sqlc 

# Or with Go
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest

# Verify new version
sqlc version
``` 
### Common Patterns:
**Transactions:** 
```go
tx, err := database.Begin() 
if err != nil {
	 return err 
} 
defer tx.Rollback() 

qtx := queries.WithTx(tx) // Use qtx for transaction-bound queries
err = qtx.CreateUser(ctx, "user@example.com") 
if err != nil {
	return err 
} 

return tx.Commit()
``` 

**Nullable fields:** 
```sql
-- Use sql.NullString, sql.NullInt64, etc. 
CREATE TABLE users ( 
	id INTEGER PRIMARY KEY,
	bio TEXT -- Generates sql.NullString in Go
);
```