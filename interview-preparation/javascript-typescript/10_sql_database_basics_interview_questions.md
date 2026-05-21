# SQL and Database Basics Interview Questions

## What this document covers

This document covers relational database and SQL basics for full-stack interviews. It focuses on tables, keys, common SQL commands, joins, indexes, transactions, relationships, and common mistakes.

## Example Tables

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  user_id INTEGER NOT NULL,
  total_amount DECIMAL(10, 2) NOT NULL,
  status VARCHAR(50) NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Interview Questions

1. **What is a relational database?**

   A relational database stores data in tables with rows and columns. Tables can be connected using keys.

2. **What is a table?**

   A table is a collection of related data. For example, a `users` table stores user records.

   ```sql
   SELECT * FROM users;
   ```

3. **What is a row?**

   A row is one record in a table. In a `users` table, one row represents one user.

4. **What is a column?**

   A column is one field in a table, such as `id`, `name`, or `email`.

5. **What is a primary key?**

   A primary key uniquely identifies each row in a table. It should not be null and should not be duplicated.

   ```sql
   id INTEGER PRIMARY KEY
   ```

6. **What is a foreign key?**

   A foreign key links one table to another table. For example, `orders.user_id` can reference `users.id`.

   ```sql
   FOREIGN KEY (user_id) REFERENCES users(id)
   ```

7. **What is SQL?**

   SQL stands for Structured Query Language. It is used to read, create, update, and delete data in relational databases.

8. **What does `SELECT` do?**

   `SELECT` reads data from a table.

   ```sql
   SELECT id, name, email
   FROM users;
   ```

9. **What does `WHERE` do?**

   `WHERE` filters rows based on a condition.

   ```sql
   SELECT id, name, email
   FROM users
   WHERE id = 1;
   ```

10. **What does `ORDER BY` do?**

    `ORDER BY` sorts query results by one or more columns.

    ```sql
    SELECT id, name, email
    FROM users
    ORDER BY name ASC;
    ```

11. **What does `LIMIT` do?**

    `LIMIT` controls how many rows are returned.

    ```sql
    SELECT id, name, email
    FROM users
    LIMIT 10;
    ```

12. **What does `INSERT` do?**

    `INSERT` adds a new row to a table.

    ```sql
    INSERT INTO users (id, name, email)
    VALUES (1, 'Alex', 'user@example.test');
    ```

13. **What does `UPDATE` do?**

    `UPDATE` changes existing rows in a table. Always use a `WHERE` clause unless you intend to update every row.

    ```sql
    UPDATE users
    SET name = 'Updated User'
    WHERE id = 1;
    ```

14. **What does `DELETE` do?**

    `DELETE` removes rows from a table. Always use a `WHERE` clause unless you intend to delete every row.

    ```sql
    DELETE FROM users
    WHERE id = 1;
    ```

15. **What is an `INNER JOIN`?**

    `INNER JOIN` returns only rows that have matching records in both tables.

    ```sql
    SELECT users.id, users.name, orders.id AS order_id, orders.total_amount
    FROM users
    INNER JOIN orders ON orders.user_id = users.id;
    ```

16. **What is a `LEFT JOIN`?**

    `LEFT JOIN` returns all rows from the left table and matching rows from the right table. If there is no match, the right-side columns are `NULL`.

    ```sql
    SELECT users.id, users.name, orders.id AS order_id
    FROM users
    LEFT JOIN orders ON orders.user_id = users.id;
    ```

17. **What is the difference between `INNER JOIN` and `LEFT JOIN`?**

    `INNER JOIN` returns only matching rows. `LEFT JOIN` returns all rows from the left table, even when there is no matching row in the right table.

18. **What is an index?**

    An index is a database structure that helps queries find rows faster. It works like a lookup table for one or more columns.

    ```sql
    CREATE INDEX idx_users_email
    ON users(email);
    ```

19. **Why use indexes?**

    Indexes improve read performance for searches, joins, and sorting. However, too many indexes can slow down inserts and updates.

20. **What is a transaction?**

    A transaction is a group of database operations that succeed or fail together. It helps protect data when multiple changes must be completed as one unit.

    ```sql
    BEGIN;

    UPDATE users
    SET email = 'updated.user@example.test'
    WHERE id = 1;

    INSERT INTO orders (id, user_id, total_amount, status)
    VALUES (100, 1, 49.99, 'created');

    COMMIT;
    ```

21. **What is data consistency?**

    Data consistency means the database remains valid and correct after operations. For example, an order should not reference a user that does not exist.

22. **What is normalization in simple words?**

    Normalization means organizing data to reduce duplication and keep related data in separate tables. For example, user data belongs in `users`, while order data belongs in `orders`.

23. **What is a one-to-many relationship?**

    A one-to-many relationship means one row in one table can relate to many rows in another table. For example, one user can have many orders.

    ```text
    users.id -> orders.user_id
    ```

24. **What is a many-to-many relationship?**

    A many-to-many relationship means many rows in one table can relate to many rows in another table. It is usually implemented with a join table.

    ```sql
    CREATE TABLE students (
      id INTEGER PRIMARY KEY,
      name VARCHAR(100) NOT NULL
    );

    CREATE TABLE courses (
      id INTEGER PRIMARY KEY,
      title VARCHAR(100) NOT NULL
    );

    CREATE TABLE student_courses (
      student_id INTEGER NOT NULL,
      course_id INTEGER NOT NULL,
      PRIMARY KEY (student_id, course_id),
      FOREIGN KEY (student_id) REFERENCES students(id),
      FOREIGN KEY (course_id) REFERENCES courses(id)
    );
    ```

25. **What are common SQL mistakes in interviews?**

    Common mistakes include forgetting `WHERE` in `UPDATE` or `DELETE`, confusing `INNER JOIN` and `LEFT JOIN`, selecting too many columns with `SELECT *`, not understanding primary and foreign keys, ignoring indexes, and not using transactions when multiple related changes must happen together.

    ```sql
    -- Mistake: updates every user
    UPDATE users
    SET status = 'inactive';

    -- Better: update one user
    UPDATE users
    SET status = 'inactive'
    WHERE id = 1;
    ```

## Common Query Examples

### SELECT User By ID

```sql
SELECT id, name, email
FROM users
WHERE id = 1;
```

### INSERT User

```sql
INSERT INTO users (id, name, email)
VALUES (1, 'Alex', 'user@example.test');
```

### UPDATE User

```sql
UPDATE users
SET name = 'Updated User'
WHERE id = 1;
```

### DELETE User

```sql
DELETE FROM users
WHERE id = 1;
```

### JOIN Users With Orders

```sql
SELECT
  users.id AS user_id,
  users.name,
  orders.id AS order_id,
  orders.total_amount,
  orders.status
FROM users
INNER JOIN orders ON orders.user_id = users.id;
```

### Simple Index Example

```sql
CREATE INDEX idx_orders_user_id
ON orders(user_id);
```
