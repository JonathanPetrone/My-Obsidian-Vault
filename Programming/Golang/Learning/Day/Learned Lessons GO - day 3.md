You need three fundamental pieces:

1. **Database driver** - The `database/sql` package is just an interface. You need an actual SQLite driver that implements it (like `github.com/mattn/go-sqlite3` or `modernc.org/sqlite` - the latter is pure Go, no CGO)
2. **Connection pool** (`sql.DB`) - This is your main handle. Despite the name, it's not a single connection but manages a pool. You create it once at startup and reuse it throughout your app's lifetime. Thread-safe.
3. **Query execution methods** - The `DB` has methods for different query patterns:
    - `Query/QueryRow` - for SELECT statements that return data
    - `Exec` - for INSERT/UPDATE/DELETE that don't return rows
    - `Prepare` - for reusing the same query multiple times with different parameters