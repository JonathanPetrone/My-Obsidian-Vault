# goose (Database Migrations) 

**NOTES:** 
- CLI tool installed globally 
- not a per-project dependency 
- Manages database schema versioning with up/down migrations
- Works with SQLite, Postgres, MySQL, and others 
### Installation (One-Time): 
```bash
# Go install
go install github.com/pressly/goose/v3/cmd/goose@latest

# Verify installation
goose -version
```

**Location:** `~/go/bin/goose`

 ### New Project Setup:
 1. **Create migrations directory:** 
```bash
mkdir -p sql/migrations
```

2. **Create first migration:** 
```bash
# Creates timestamped migration file
goose -dir sql/migrations create init_schema sql
``` 

This generates: `sql/migrations/20260128120000_init_schema.sql` 

3. **Edit migration file:** 
```sql
-- +goose Up
CREATE TABLE users ( 
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	email TEXT NOT NULL UNIQUE,
	created_at TEXT DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE posts (
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	user_id INTEGER NOT NULL,
	title TEXT NOT NULL,
	created_at TEXT DEFAULT CURRENT_TIMESTAMP,
	FOREIGN KEY (user_id) REFERENCES users(id)
); 

-- +goose Down 
DROP TABLE posts; 
DROP TABLE users;
``` 

### Running Migrations: 
**SQLite:** 
```bash
goose -dir sql/migrations sqlite3 app.db up
```

**Postgres:** 
```bash
goose -dir sql/migrations postgres "user=postgres password=secret dbname=mydb sslmode=disable" up
```
### Common Commands:
```bash
# Apply all pending migrations
goose -dir sql/migrations sqlite3 app.db up

# Rollback last migration
goose -dir sql/migrations sqlite3 app.db down

# Show migration status
goose -dir sql/migrations sqlite3 app.db status

# Rollback all migrations
goose -dir sql/migrations sqlite3 app.db reset

# Show current version
goose -dir sql/migrations sqlite3 app.db version

# Apply next N migrations
goose -dir sql/migrations sqlite3 app.db up-by-one 
```
### Creating New Migrations: 
```bash
# SQL migration
goose -dir sql/migrations create add_users_table sql

# Go migration (for complex data transformations)
goose -dir sql/migrations create seed_data go
``` 

### Migration File Format: 
**SQL migrations:** 
```sql 
-- +goose Up -- SQL in this section is executed when migration is applied 

-- +goose Down -- SQL in this section is executed when migration is rolled back
```

**Go migrations** (advanced):
```go 
package migrations

import (
	"database/sql"
	"github.com/pressly/goose/v3"
) 

func init() { 
	goose.AddMigration(upComplexData, downComplexData)
}

func upComplexData(tx *sql.Tx) error { 
	// Complex data transformation logic 
	return nil 
} 

func downComplexData(tx *sql.Tx) error { 
	return nil
}
```
### Workflow: 
1. Create migration: `goose create migration_name sql`
2. Edit migration file with `Up` and `Down` sections
3. Apply migration: `goose sqlite3 app.db up`
4. Verify with: `goose sqlite3 app.db status`
### Project Structure: 
```
your-project/
├── sql/
│ └── migrations/
│ ├── 20260128120000_init_schema.sql
│ ├── 20260128130000_add_posts_table.sql
│ └── 20260128140000_add_indexes.sql
└── app.db
```
### Integration with Code: 
**Option 1: Run goose manually** (simple projects)
```bash
goose -dir sql/migrations sqlite3 app.db up
```

**Option 2: Embed in application** (production)
```go
import (
	"database/sql" 
	"embed"
	"github.com/pressly/goose/v3"
)

//go:embed sql/migrations/*.sql 
var embedMigrations embed.FS

func runMigrations(db *sql.DB) error { 
	goose.SetBaseFS(embedMigrations) 
	
	if err := goose.SetDialect("sqlite3"); err != nil { 
		return err
	} 
	
	if err := goose.Up(db, "sql/migrations"); err != nil {
		return err
	} 
	
	return nil
}
```
### Updates: 
```bash
# Update goose binary
go install github.com/pressly/goose/v3/cmd/goose@latest 

# Verify new version
goose -version
``` 
### Best Practices:
- Always write `Down` migrations for rollback capability
- One logical change per migration
- Test migrations on copy of production data
- Never modify applied migrations (create new one instead)
- Use descriptive migration names