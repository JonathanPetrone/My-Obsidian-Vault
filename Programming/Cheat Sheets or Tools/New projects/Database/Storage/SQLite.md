# SQLite with Go (ncruces/go-sqlite3) 

**NOTES:**
- Pure Go implementation via WebAssembly
- no C compiler neede
- Module cache at `~/go/pkg/mod/` shared across projects 
### New Project Setup:
1. **Add dependency:** 
```bash 
go get github.com/ncruces/go-sqlite3 
``` 

2. **Basic usage:** 
```go 
import ( 
"database/sql" 
_ "github.com/ncruces/go-sqlite3" 
) 

db, err := sql.Open("sqlite3", "app.db") 
if err != nil { log.Fatal(err) } 

defer db.Close() 

err = db.Ping() // Verify connection 
if err != nil { log.Fatal(err) } 
``` 

3. **Initialize schema:** 
```go
_, err = db.Exec(` CREATE TABLE IF NOT EXISTS tasks ( id INTEGER PRIMARY KEY, created_at TEXT DEFAULT CURRENT_TIMESTAMP, title TEXT NOT NULL ) `) 
```

### Database Locations: 
- `app.db` - same directory as binary 
- `./data/app.db` - subdirectory (must exist first)
- `:memory:` - in-memory (lost on restart) 
### Updates: 
```bash
go get -u github.com/ncruces/go-sqlite3 go mod tidy
```