
There are 3 primary types of relationships in a relational database:
1. One-to-one
2. One-to-many
3. Many-to-many

![[Pasted image 20250113202242.png|500]]

## One-to-one
A `one-to-one` relationship most often manifests as a field or set of fields on a row in a table. For example, a `user` will have exactly one `password`.
## One-to-many
A `customers` table and an `orders` table. Each customer has `0`, `1`, or many orders that they've placed.

CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    amount INTEGER NOT NULL,
    customer_id INTEGER,
    CONSTRAINT fk_customers
    FOREIGN KEY (customer_id)
    REFERENCES customers(id)
);
## Many-to-many
A `classes` table and a `students` table - Students can take potentially many classes and classes can have many students enrolled.

Joining tables help define many-to-many relationships between data in a database. As an example when defining the relationship above between products and suppliers, we would define a joining table called `products_suppliers` that contains the primary keys from the tables to be joined.

Then, when we want to see if a supplier supplies a specific product, we can look in the joining table to see if the ids share a row.

### Unique Constraint Across 2 Fields

When enforcing specific schema constraints we may need to enforce the `UNIQUE` constraint across two different fields.

CREATE TABLE product_suppliers (
  product_id INTEGER,
  supplier_id INTEGER,
  UNIQUE(product_id, supplier_id)
);

## Normalization Review
The _exact_ definitions of 1st, 2nd, 3rd and Boyce-Codd normal forms simply are _not all that important_ in your work as a back-end developer.

However, what _is important_ is to understand the basic principles of data integrity and data redundancy that the normal forms teach us. Let's go over some rules of thumb that you should commit to memory - they'll serve you well when you design databases and even just in coding interviews.

## Rules of Thumb for Database Design

1. Every table should always have a unique identifier (primary key)
2. 90% of the time, that unique identifier will be a single column named `id`
3. Avoid duplicate data
4. Avoid storing data that is completely dependent on other data. Instead, compute it on the fly when you need it.
5. Keep your schema as simple as you can. Optimize for a _normalized_ database first. Only denormalize for speed's sake when you start to run into performance problems.
