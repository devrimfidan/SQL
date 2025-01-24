# Quick SQL Cheatsheet

A quick reminder of all relevant SQL queries and examples on how to use them.

This repository is constantly being updated and added to by the community. Pull requests are welcome. Enjoy!

# Table of Contents 
1. [ Finding Data Queries. ](#find)
2. [ Data Modification Queries. ](#modify)
3. [ Reporting Queries. ](#report)
4. [ Join Queries. ](#joins)
5. [ View Queries. ](#view)
6. [ Altering Table Queries.](#alter)
7. [ Creating Table Query.](#create)

<a name="find"></a>
# 1. Finding Data Queries
### `SELECT`: Retrieve data from a table
```sql
SELECT * FROM employees;
```

### `DISTINCT`: Remove duplicate values
```sql
SELECT DISTINCT department FROM employees;
```

### `WHERE`: Filter rows
```sql
SELECT name, age FROM employees WHERE age > 30;
SELECT * FROM employees WHERE age > 30 AND department = 'HR';
SELECT * FROM employees WHERE age < 25 OR position = 'Intern';
SELECT * FROM employees WHERE NOT age = 40;
```

### `ORDER BY`: Sort the result set
```sql
SELECT * FROM employees ORDER BY age;
SELECT * FROM employees ORDER BY age DESC;
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```

### `SELECT TOP` (or `LIMIT`): Limit the number of rows
```sql
SELECT TOP 5 name, salary FROM employees;
SELECT name, salary FROM employees LIMIT 10;
```

### `LIKE`: Pattern matching
```sql
SELECT name FROM employees WHERE name LIKE 'J%';
SELECT name FROM employees WHERE email LIKE '%@gmail.com';
SELECT name FROM employees WHERE name LIKE '_a%';
SELECT name FROM employees WHERE name LIKE 'J___';
```

### `IN`: Match any value in a list
```sql
SELECT name FROM employees WHERE department IN ('IT', 'Finance');
```

### `BETWEEN`: Select values in a range
```sql
SELECT name FROM employees WHERE age BETWEEN 25 AND 35;
```

### `NULL`: Check for null values
```sql
SELECT * FROM employees WHERE phone IS NULL;
SELECT * FROM employees WHERE phone IS NOT NULL;
```

### `AS`: Create column or table aliases
```sql
SELECT name AS employee_name, salary AS monthly_salary FROM employees;
```

### `UNION`: Combine result sets
```sql
SELECT name FROM employees WHERE department = 'IT'
UNION
SELECT name FROM employees WHERE department = 'HR';
```

### `GROUP BY`: Aggregate data
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department;
```

### `HAVING`: Filter grouped data
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 5;
```

### `WITH`: Common Table Expression
```sql
WITH EmployeeHierarchy AS (
  SELECT id, name, manager_id FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id FROM employees e
  INNER JOIN EmployeeHierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM EmployeeHierarchy;
```

---

<a name="modify"></a>
# 2. Data Modification Queries

### `INSERT INTO`: Add new rows
```sql
INSERT INTO employees (name, age, department) VALUES ('Alice', 30, 'IT');
```

### `UPDATE`: Modify existing rows
```sql
UPDATE employees SET salary = salary + 500 WHERE department = 'HR';
```

### `DELETE`: Remove rows
```sql
DELETE FROM employees WHERE age < 20;
```

---

<a name="report"></a>
# 3. Reporting Queries


### `COUNT`: Count rows
```sql
SELECT COUNT(*) FROM employees;
```

### `MIN()` and `MAX()`: Find min/max values
```sql
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;
```

### `AVG()`: Calculate average
```sql
SELECT AVG(age) FROM employees;
```

### `SUM()`: Calculate total
```sql
SELECT SUM(salary) FROM employees;
```

---

<a name="joins"></a>
# 4. Join Queries

### `INNER JOIN`: Combine matching rows from two tables
```sql
SELECT e.name, d.department_name FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

### `LEFT JOIN`: Include all rows from the left table
```sql
SELECT e.name, d.department_name FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

### `RIGHT JOIN`: Include all rows from the right table
```sql
SELECT e.name, d.department_name FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```

### `FULL JOIN`: Include all matching rows from both tables
```sql
SELECT e.name, d.department_name FROM employees e
FULL JOIN departments d ON e.department_id = d.id;
```

### `SELF JOIN`: Join a table with itself
```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.id;
```

---
<a name="view"></a>
# 5. View Queries
### `CREATE`: Create a view
```sql
CREATE VIEW IT_Employees AS
SELECT name, age, salary FROM employees WHERE department = 'IT';
```

### `SELECT`: Query a view
```sql
SELECT * FROM IT_Employees;
```

### `DROP`: Remove a view
```sql
DROP VIEW IT_Employees;
```

---

<a name="alter"></a>
# 6. Altering Table Queries


### `ADD`: Add a new column
```sql
ALTER TABLE employees ADD phone_number VARCHAR(15);
```

### `MODIFY`: Change a column's data type
```sql
ALTER TABLE employees MODIFY salary DECIMAL(10, 2);
```

### `DROP`: Remove a column
```sql
ALTER TABLE employees DROP COLUMN phone_number;
```

---

<a name="create"></a>
# 7. Creating Table Query


### `CREATE`: Define a new table
```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  age INT,
  department VARCHAR(50),
  salary DECIMAL(10, 2)
);
