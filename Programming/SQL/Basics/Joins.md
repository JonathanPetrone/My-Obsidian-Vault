Joins are one of the most important features that SQL offers. Joins allow us to make use of the relationships we have set up between our tables. In short, joins allow us to query multiple tables at the _same time._

## Inner Join

The simplest and most common type of join in SQL is the `INNER JOIN`. By default, a `JOIN` command is an `INNER JOIN`. An `INNER JOIN` returns all of the records in `table_a` that have matching records in `table_b` as demonstrated by the following Venn diagram.

![inner join|500](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/DS7U62Q.png)

## On

To perform a table join, we need to tell the database how to "match up" the rows from each table. The `ON` clause specifies the columns from each table that should be compared.

When the same column name exists in both tables, we have to **specify which table each column comes from** using the table name (or an alias) followed by a dot `.` before the column name.

```sql
SELECT *
FROM employees
INNER JOIN departments
ON employees.department_id = departments.id;
```

### Namespacing on Tables

When working with multiple tables, you can specify which table a field exists on using a `.`. For example:

SELECT students.name, classes.name
FROM students
INNER JOIN classes ON classes.class_id = students.class_id;

## Left Join

A `LEFT JOIN` will return every record from `table_a` regardless of whether or not any of those records have a match in `table_b`. A left join will _also_ return any matching records from `table_b`. Here is a Venn diagram to help visualize the effect of a `LEFT JOIN`.

![left-join|500](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/JoZYX5L.png)

## Full Join

A `FULL JOIN` combines the result set of the `LEFT JOIN` and `RIGHT JOIN` commands. It returns _all_ records from both from `table_a` and `table_b` regardless of whether or not they have matches.

![Full-join|500](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/uAXzpID.png)