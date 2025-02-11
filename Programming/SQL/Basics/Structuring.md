### Limit
The `LIMIT` keyword can be used at the end of a select statement to reduce the number of records returned.

SELECT * FROM products
    WHERE product_name LIKE '%berry%'
    LIMIT 50;

### ORDER BY
SQL also offers us the ability to sort the results of a query using `ORDER BY`. By default, the `ORDER BY` keyword sorts records by the given field in ascending order, or `ASC` for short. However, `ORDER BY` does support descending order as well with the keyword `DESC`.

SELECT name, price, quantity FROM products
    ORDER BY quantity DESC;