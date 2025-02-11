## Migrations

A migration is just a set of changes to your database table. You can have as many migrations as needed as your requirements change over time. For example, one migration might create a new table, one might delete a column, and one might add 2 new columns.

### Goose
[Goose](https://github.com/pressly/goose) is a database migration tool written in Go. It runs migrations from a set of SQL files, making it a perfect fit for this project (we wanna stay close to the raw SQL).

```
-- +goose Up
CREATE TABLE ...

-- +goose Down
DROP TABLE users;
```

### SQLC
[SQLC](https://sqlc.dev/) is a Go program that generates Go code from SQL queries. It's not exactly an [ORM](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/), but rather a tool that makes working with raw SQL easy and type-safe.

### The Context Package

The `context` package is a part of Go's standard library. It does several things, but the most important thing is that it handles timeouts. All of SQLC's database queries accept a [`context.Context`](https://pkg.go.dev/context#Context) as their first argument:

## Popular Databases

You don't _need_ to know about all of these, but you might be curious about some of the database technologies out there. Here are a few of the most popular ones:

- [PostgreSQL](https://www.postgresql.org/): A fantastic open-source SQL database.
- [MySQL](https://www.mysql.com/): Another open-source SQL database. Less fantastic IMO.
- [MongoDB](https://www.mongodb.com/): A popular open-source NoSQL document database.
- [Firebase](https://firebase.google.com/): A popular cloud-based NoSQL database service.
- [SQLite](https://www.sqlite.org/index.html): A popular embedded SQL database.

Feel free to browse [DB Engine](https://db-engines.com/en/ranking) if you want to dive deeper into the world of database technologies.
