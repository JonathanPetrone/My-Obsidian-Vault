## CREATE
CREATE TABLE people(
  id INTEGER PRIMARY KEY,
  tag TEXT,
  name TEXT,
  age INTEGER,
  balance INTEGER,
  is_admin BOOLEAN
);
## ALTER
ALTER TABLE people
RENAME TO users;
ALTER TABLE users
RENAME COLUMN tag TO username;
ALTER TABLE users
ADD COLUMN password TEXT;

A database [migration](https://en.wikipedia.org/wiki/Schema_migration) is a set of changes to a relational database. In fact, the `ALTER TABLE` statements is examples of migrations!

When writing _reversible_ migrations, we use the terms "up" and "down" migrations. An "up" migration is simply the set of changes you want to make, like altering/removing/adding/editing a table in some way. A "down" migration includes the changes that would _revert_ any of the "up" migration's changes.

SQL as a language can support many different data types. However, the datatypes that _your database management system ([DBMS](https://en.wikipedia.org/wiki/Database#:~:text=A%20database%20management%20system%20(DBMS)))_ supports will vary depending on the specific database you're using.