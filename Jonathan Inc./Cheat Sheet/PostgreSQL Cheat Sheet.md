## Connection Commands (Terminal)

### Basic Connection
```bash
# Connect to default database as current user
psql

# Connect to specific database
psql database_name

# Connect as specific user
psql -U username

# Connect to specific database as specific user
psql -U username -d database_name

# Connect to remote host
psql -h hostname -U username -d database_name

# Connection string format
psql "postgresql://username:password@localhost:5432/database_name"
```
### Common Connection Examples

```bash
# For your tarot app
psql -U aitarot_user -d aitarot

# Connect to default postgres database
psql postgres

# Create database from command line
createdb database_name

# Drop database from command line
dropdb database_name
```

## psql Meta Commands (Inside psql)

### Getting Help
```sql
\?              -- Show all psql commands
\h              -- Show SQL command help
\h CREATE TABLE -- Help for specific SQL command
```
### Database Operations
```sql
\l              -- List all databases
\c database     -- Connect to database
\c aitarot      -- Connect to aitarot database
\conninfo       -- Show connection info
```

### Table Operations

```sql
\dt             -- List all tables
\dt+            -- List tables with details
\d table_name   -- Describe table structure
\d+ table_name  -- Detailed table description
\di             -- List indexes
\ds             -- List sequences
```

### User & Permission Operations

```sql
\du             -- List all users/roles
\du+            -- List users with details
\z              -- List table permissions
\dp             -- Same as \z
```

### Schema Operations

```sql
\dn             -- List all schemas
\dn+            -- List schemas with details
```

### Query & Execution

```sql
\i filename.sql -- Execute SQL file
\o filename     -- Send output to file
\o              -- Stop sending output to file
\e              -- Open editor to write query
\g              -- Execute last query again
```

### Display & Formatting

```sql
\x              -- Toggle expanded output (good for wide tables)
\timing         -- Toggle query timing display
\pset           -- Set output formatting options
```

### Exit

```sql
\q              -- Quit psql
\quit           -- Same as \q
```

## SQL Commands for Database Setup

### Database Management

```sql
-- Create database
CREATE DATABASE aitarot;

-- Drop database
DROP DATABASE database_name;

-- Show current database
SELECT current_database();

-- Show database size
SELECT pg_size_pretty(pg_database_size('aitarot'));
```

### User Management

```sql
-- Create user
CREATE USER aitarot_user WITH ENCRYPTED PASSWORD 'password';

-- Create user with login privileges
CREATE USER username WITH LOGIN PASSWORD 'password';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE aitarot TO aitarot_user;
GRANT ALL ON SCHEMA public TO aitarot_user;
GRANT CREATE ON SCHEMA public TO aitarot_user;

-- Change password
ALTER USER aitarot_user WITH PASSWORD 'new_password';

-- Drop user
DROP USER username;

-- Show current user
SELECT current_user;

-- Show session user
SELECT session_user;
```

### Table Operations

```sql
-- Create table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Show table structure
\d users

-- Show table data
SELECT * FROM users;

-- Show table size
SELECT pg_size_pretty(pg_total_relation_size('users'));

-- Drop table
DROP TABLE table_name;
```

### Useful Queries

```sql
-- Show all tables in current database
SELECT tablename FROM pg_tables WHERE schemaname = 'public';

-- Show table row counts
SELECT 
    schemaname,
    tablename,
    n_tup_ins as "inserts",
    n_tup_upd as "updates",
    n_tup_del as "deletes",
    n_live_tup as "live_rows"
FROM pg_stat_user_tables;

-- Show database connections
SELECT * FROM pg_stat_activity;

-- Show PostgreSQL version
SELECT version();

-- Show current time
SELECT NOW();
```

## Common Troubleshooting

### Check if PostgreSQL is running

```bash
# Check process
ps aux | grep postgres

# Check if listening on port
lsof -i :5432

# macOS - check Homebrew services
brew services list | grep postgres
```

### Start/Stop PostgreSQL

```bash
# macOS (Homebrew)
brew services start postgresql
brew services stop postgresql
brew services restart postgresql

# Linux (systemd)
sudo systemctl start postgresql
sudo systemctl stop postgresql
sudo systemctl restart postgresql
```

### Connection Issues

```sql
-- Check current connections
SELECT * FROM pg_stat_activity WHERE datname = 'aitarot';

-- Kill connection by PID
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'aitarot';
```

### Reset forgotten password

```bash
# 1. Edit pg_hba.conf to allow trust authentication
# 2. Restart PostgreSQL
# 3. Connect without password
psql -U postgres
# 4. Change password
ALTER USER postgres WITH PASSWORD 'new_password';
# 5. Restore pg_hba.conf
# 6. Restart PostgreSQL
```

## Your Tarot App Setup

### Initial Setup Commands

```bash
# Connect to postgres database
psql postgres

# Create your database and user
CREATE DATABASE aitarot;
CREATE USER aitarot_user WITH ENCRYPTED PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE aitarot TO aitarot_user;

# Connect to your new database
\c aitarot
GRANT ALL ON SCHEMA public TO aitarot_user;
GRANT CREATE ON SCHEMA public TO aitarot_user;

# Test connection
\q
psql -U aitarot_user -d aitarot
```

### Environment Variables

```bash
# Add to .env file
DATABASE_URL=postgres://aitarot_user:your_password@localhost:5432/aitarot?sslmode=disable
```

### Quick Test Commands

```sql
-- Test your connection works
\conninfo

-- Create a test table
CREATE TABLE test (id SERIAL, name TEXT);

-- Insert test data
INSERT INTO test (name) VALUES ('Hello World');

-- Query test data
SELECT * FROM test;

-- Clean up
DROP TABLE test;
```