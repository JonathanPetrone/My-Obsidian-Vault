An "aggregation" is a _single_ value that's derived by combining _several_ other values. We performed an aggregation earlier when we used the `COUNT` statement to count the number of records in a table.

## Why Aggregations?

Data stored in a database should generally be stored [raw](https://wagslane.dev/posts/keep-your-data-raw-at-rest/). When we need to calculate some additional data from the raw data, we can use an _aggregation_.

### count aggregation
SELECT COUNT( * )
  FROM transactions
  WHERE (user_id = 6 AND was_successful = true);
### sum aggregation
SELECT sum(salary)
FROM employees;
### max aggregation
SELECT max(price)
FROM products;
### min aggregation
SELECT product_name, min(price)
FROM products;

### group by aggregation
There are times we need to group data based on specific values.

SQL offers the `GROUP BY` clause which can group rows that have similar values into "summary" rows. It returns one row for each group. The interesting part is that each group can have an aggregate function applied to it that operates only on the grouped data.

Imagine that we have a database with songs and albums, and we want to see how many songs are on each album. We can use a query like this:

SELECT album_id, count(song_id)
FROM songs
GROUP BY album_id;

This query retrieves a count of all the songs on each album. One record is returned per album, and they each have their own `count`.

### avg aggregation
SELECT avg(song_length)
FROM songs;

### Having aggregation
When we need to filter the results of a `GROUP BY` query even further, we can use the `HAVING` clause. The `HAVING` clause specifies a search condition for a group.

The `HAVING` clause is similar to the `WHERE` clause, but it operates on groups _after_ they've been grouped, rather than rows _before_ they've been grouped.

SELECT album_id, count(id) as count
FROM songs
GROUP BY album_id
HAVING count > 5;

This query returns the `album_id` and count of its songs, but only for albums with more than `5` songs.


