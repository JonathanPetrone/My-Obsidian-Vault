### PostgreSQL vs SQLite Decision:

**Use SQLite when:**
- Single user/local application
- Simple data model
- Embedded deployment (no separate database server)
- Development/testing

**Use PostgreSQL when:**
- Multiple concurrent users
- Need advanced features (JSON, arrays, full-text search)
- Scaling beyond single machine
- Production web applications