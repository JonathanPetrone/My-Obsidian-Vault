## CREATE -> Insert
INSERT INTO employees(id, name, title)
VALUES (1, 'Allan', 'Engineer');

## READ -> Select
SELECT * FROM crud;

## UPDATE -> Update
UPDATE employees
SET job_title = 'Backend Engineer', salary = 150000
WHERE id = 251;

## DELETE -> Delete
DELETE 
FROM users 
WHERE id = 2;

## ORM
An [Object-Relational Mapping](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) or an _ORM_ for short, is a tool that allows you to perform CRUD operations on a database using a traditional programming language. These typically come in the form of a library or framework that you would use in your backend code.

The primary benefit an ORM provides is that it maps your database records to in-memory objects. For example, in Go we might have a struct that we use in our code:

```go
type User struct {
    ID int
    Name string
    IsAdmin bool
}
```

```
user := User{
    ID: 10,
    Name: "Lane",
    IsAdmin: false,
}

// generates a SQL statement and runs it,
// creating a new record in the users table
db.Create(user)
```
