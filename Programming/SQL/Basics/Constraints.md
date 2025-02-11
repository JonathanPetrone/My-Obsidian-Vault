## NULL 
In SQL, a cell with a `NULL` value indicates that the value is _missing_. A `NULL`value is _very_ different from a _zero_ value. Setting a `NOT NULL` constraint on a column ensures that the column will not accept `NULL` values.

CREATE TABLE employees(
    id INTEGER PRIMARY KEY,
    name TEXT UNIQUE,
    title TEXT NOT NULL
);
## Primary Keys
A _key_ defines and protects relationships between tables. A [`primary key`](https://en.wikipedia.org/wiki/Primary_key) is a special column that uniquely identifies records within a table. Each table can have one, and only one primary key.

It's _very_ common to have a column named `id` on each table in a database, and that `id` is the primary key for that table. No two rows in that table can share an `id`.

## Foreign Keys

Foreign keys are what makes relational databases relational! Foreign keys define the relationships _between_ tables. Simply put, a `FOREIGN KEY` is a field in one table that references another table's `PRIMARY KEY`.

CREATE TABLE departments (
    id INTEGER PRIMARY KEY,
    department_name TEXT NOT NULL
);

CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department_id INTEGER,
    CONSTRAINT fk_departments
    FOREIGN KEY (department_id)
    REFERENCES departments(id)
);

## Schema

A database's [schema](https://www.ibm.com/think/topics/database-schema) describes how data is organized within it.

Data types, table names, field names, constraints, and the relationships between all of those entities are part of a database's _schema_.