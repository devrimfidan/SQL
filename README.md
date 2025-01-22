# SQL Examples Repository

Welcome to the SQL Examples repository! This collection is aimed at helping developers and data enthusiasts understand and work with SQL. Here, you will find SQL queries for various use cases, ranging from basic operations to more advanced techniques. The goal is to provide practical SQL examples that can be directly applied or adapted to your own projects.

## What is SQL?

SQL (Structured Query Language) is a standard programming language used to manage and manipulate relational databases. It is essential for querying, inserting, updating, and deleting data in databases. SQL is used by most relational database management systems (RDBMS) like MySQL, PostgreSQL, Microsoft SQL Server, and SQLite.

## Basic SQL Concepts

Here are some of the fundamental SQL concepts and operations that you’ll encounter in this repository:

### 1. **SELECT Statement**
   - The `SELECT` statement is used to retrieve data from a database.
   - Example: `SELECT column_name FROM table_name;`
   
### 2. **WHERE Clause**
   - The `WHERE` clause is used to filter records based on specific conditions.
   - Example: `SELECT * FROM employees WHERE salary > 50000;`
   
### 3. **INSERT Statement**
   - The `INSERT INTO` statement is used to add new records to a table.
   - Example: `INSERT INTO employees (name, salary) VALUES ('John Doe', 60000);`

### 4. **UPDATE Statement**
   - The `UPDATE` statement is used to modify existing records in a table.
   - Example: `UPDATE employees SET salary = 70000 WHERE name = 'John Doe';`

### 5. **DELETE Statement**
   - The `DELETE` statement removes records from a table.
   - Example: `DELETE FROM employees WHERE name = 'John Doe';`

### 6. **Joins**
   - Joins are used to combine rows from two or more tables based on a related column.
   - Example: `SELECT employees.name, departments.name FROM employees INNER JOIN departments ON employees.department_id = departments.id;`

### 7. **Aggregation Functions**
   - SQL provides functions such as `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()` to perform calculations on data.
   - Example: `SELECT COUNT(*) FROM employees WHERE salary > 50000;`

### 8. **GROUP BY Clause**
   - The `GROUP BY` clause groups rows that have the same values in specified columns, often used with aggregation functions.
   - Example: `SELECT department_id, AVG(salary) FROM employees GROUP BY department_id;`

### 9. **Subqueries**
   - A subquery is a query nested inside another query. It is used to perform operations like filtering data, joining tables, and more.
   - Example: `SELECT name FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);`

### 10. **Creating Tables**
   - The `CREATE TABLE` statement is used to define a new table along with its columns and data types.
   - Example: 
     ```sql
     CREATE TABLE employees (
         id INT PRIMARY KEY,
         name VARCHAR(100),
         salary DECIMAL
     );
     ```

### 11. **Transactions**
   - Transactions are used to execute a series of SQL operations as a single unit. This ensures that either all operations succeed or none.
   - Example: 
     ```sql
     BEGIN;
     INSERT INTO employees (name, salary) VALUES ('Jane Smith', 70000);
     COMMIT;
     ```

### 12. **Indexes**
   - An index improves the speed of data retrieval. It’s created on one or more columns of a table.
   - Example: `CREATE INDEX idx_employee_name ON employees (name);`

## How to Use This Repository

In this repository, you will find various SQL files with practical examples for different SQL tasks, including:

- Basic queries (SELECT, INSERT, UPDATE, DELETE)
- Join operations
- Advanced queries (subqueries, aggregation, etc.)
- Database management (CREATE, ALTER, DROP)

Feel free to explore the examples, adapt them for your own projects, and contribute your own SQL queries or improvements!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Happy querying! ✨
