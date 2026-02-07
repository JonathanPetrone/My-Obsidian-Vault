### 1. Raw SQL (database/sql standard library)
- What you're using now
- Write SQL directly, scan results manually
- **Pros:** Full control, no magic, portable SQL knowledge
- **Cons:** Verbose, manual type mapping, typo-prone
### 2. Query Builders (squirrel, goqu)
- Programmatic SQL construction
- **Pros:** Type-safe queries, composable, prevents SQL injection
- **Cons:** Learning curve, Go syntax instead of SQL
### 3. ORMs (GORM, ent, bun)
- Map structs to tables automatically
- **Pros:** Fast CRUD, migrations, relationships handled
- **Cons:** Abstracts SQL away, performance surprises, magic behavior
### 4. Code Generators (sqlc, sqlboiler)
- Write SQL → generates type-safe Go code
- **Pros:** SQL control + type safety, zero runtime overhead
- **Cons:** Build step, regenerate on schema changes
### 5. Hybrid (sqlx)
- Extends `database/sql` with struct scanning
- **Pros:** Minimal abstraction, familiar SQL
- **Cons:** Still manual query writing