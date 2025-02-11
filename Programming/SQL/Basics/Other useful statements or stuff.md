### Count
SELECT COUNT(*) 
FROM employees;

### Where
SELECT name 
FROM users 
WHERE power_level >= 9000;

### AS clause
SELECT note AS birthday_message, amount
FROM transactions WHERE sender_id = 10;

### SQL functions -> SQLite
SELECT * ,
  IIF(was_successful = true, 'No action required', 'Perform an audit') AS audit
  FROM transactions

### Between
SELECT name, age
FROM users
WHERE age BETWEEN 18 and 30;

### Distinct
SELECT DISTINCT country_code FROM users;

### OR
SELECT COUNT( * ) AS junior_count
FROM users
WHERE (country_code = 'CA' AND age < 18) 
   OR (country_code = 'US' AND age < 18);

### IN
SELECT name, age, country_code
  FROM users
  WHERE country_code IN ('US','MX','CA');

### Like
SELECT *
  FROM users
  WHERE name LIKE 'Al%';

The `LIKE` keyword allows for the use of the `%` and `_` wildcard operators. The `%` operator will match zero or more characters. Meanwhile, the `_` wildcard operator only matches a _single_ character

## Round
The SQL `round()` function allows you to specify both the value you wish to round and the precision to which you wish to round it:

SELECT song_name, round(avg(song_length), 1)
FROM songs

## No Tables

When working on a back-end application, this doesn't come up often, but it's important to remember that **SQL is a full programming language**. We usually use it to interact with data stored in tables, but it's quite flexible and powerful.

For example, you can `SELECT` information that's simply calculated, with no tables necessary.

SELECT 5 + 10 as sum;