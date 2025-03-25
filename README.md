# MySQL: Complete Guide with Examples

This document provides a comprehensive overview of MySQL, including theory and practical usage with examples. MySQL is one of the most popular relational database management systems (RDBMS) used in web development and data-driven applications.

---

## 🔹 What is MySQL?
MySQL is an open-source RDBMS based on Structured Query Language (SQL). It stores data in tables and supports various operations such as data insertion, querying, updating, and schema modifications.

---

## 🔹 Key MySQL Concepts

### 1. **Databases and Tables**
- **CREATE DATABASE**: Initializes a new database.
```sql
CREATE DATABASE sample_db;
```
- **USE**: Switches to a particular database.
```sql
USE sample_db;
```
- **CREATE TABLE**: Creates a new table with specified columns and constraints.
```sql
CREATE TABLE departments (
  dept_id INT PRIMARY KEY,
  dept_name VARCHAR(50)
);

CREATE TABLE employees (
  emp_id INT PRIMARY KEY,
  emp_name VARCHAR(100),
  age INT,
  salary DECIMAL(10,2),
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

---

### 2. **Data Manipulation**
- **INSERT INTO**: Adds new records to a table.
```sql
INSERT INTO departments VALUES (1, 'HR'), (2, 'Engineering');
INSERT INTO employees VALUES (101, 'Alice', 30, 60000, 1);
```

- **UPDATE**: Modifies existing data.
```sql
UPDATE employees SET salary = salary + 5000 WHERE emp_id = 101;
```

- **DELETE**: Removes records.
```sql
DELETE FROM employees WHERE emp_id = 101;
```

---

### 3. **Querying Data**
- **SELECT**: Fetches data from tables.
```sql
SELECT * FROM employees;
SELECT emp_name, salary FROM employees;
```

- **WHERE**: Filters results.
```sql
SELECT * FROM employees WHERE age > 30;
```

- **AND / OR**: Combines conditions.
```sql
SELECT * FROM employees WHERE age > 25 AND salary > 50000;
```

- **LIKE**: Pattern matching.
```sql
SELECT * FROM employees WHERE emp_name LIKE 'A%';
```

- **IN**: Checks if a value matches any value in a list.
```sql
SELECT * FROM employees WHERE dept_id IN (1, 2);
```

- **EXISTS**: Tests for existence of a record in subquery.
```sql
SELECT * FROM employees e WHERE EXISTS (
  SELECT 1 FROM departments d WHERE d.dept_id = e.dept_id
);
```

---

### 4. **Sorting and Grouping**
- **ORDER BY**: Sorts results.
```sql
SELECT * FROM employees ORDER BY salary DESC;
```

- **GROUP BY**: Groups rows with the same values.
```sql
SELECT dept_id, COUNT(*) AS total_employees FROM employees GROUP BY dept_id;
```

- **HAVING**: Filters grouped records.
```sql
SELECT dept_id, COUNT(*) FROM employees GROUP BY dept_id HAVING COUNT(*) > 1;
```

---

### 5. **CASE Statement**
Used for conditional logic inside queries.
```sql
SELECT emp_name, salary,
  CASE
    WHEN salary >= 60000 THEN 'High'
    WHEN salary >= 50000 THEN 'Medium'
    ELSE 'Low'
  END AS salary_band
FROM employees;
```

---

### 6. **Schema Modifications**
- **ALTER TABLE**: Adds, modifies, or deletes columns.
```sql
ALTER TABLE employees ADD COLUMN join_date DATE;
ALTER TABLE employees MODIFY COLUMN emp_name VARCHAR(150);
ALTER TABLE employees DROP COLUMN join_date;
```

- **DROP TABLE**: Deletes a table.
```sql
DROP TABLE employees;
```

---

## 🔹 Constraints and Keys
- **PRIMARY KEY**: Uniquely identifies each record.
- **FOREIGN KEY**: Maintains referential integrity.
- **NOT NULL**: Ensures a column cannot have NULL values.
- **UNIQUE**: Ensures all values in a column are distinct.
- **DEFAULT**: Assigns a default value.
```sql
CREATE TABLE users (
  user_id INT PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  status VARCHAR(10) DEFAULT 'active'
);
```

---

## 🔹 Indexing
Indexes improve the performance of SELECT queries, especially on large datasets.
```sql
CREATE INDEX idx_salary ON employees(salary);
DROP INDEX idx_salary ON employees;
```
- Indexes can be single-column or multi-column.
- Can significantly speed up lookups, joins, and WHERE conditions.
- Too many indexes can slow down INSERT/UPDATE operations.

---

## 🔹 Views
A **View** is a virtual table created by a SELECT query. Useful for simplifying complex queries or hiding sensitive columns.
```sql
CREATE VIEW high_salary_employees AS
SELECT emp_name, salary FROM employees WHERE salary > 60000;

SELECT * FROM high_salary_employees;
```
- Views do not store data themselves.
- Can be queried like regular tables.
- Use **WITH CHECK OPTION** to restrict updates through the view.

---

## 🔹 Joins
Joins are used to fetch data from multiple related tables.

### INNER JOIN
Returns only matching rows.
```sql
SELECT e.emp_name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;
```

### LEFT JOIN
Returns all rows from the left table and matching rows from the right table.
```sql
SELECT e.emp_name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

### RIGHT JOIN
Returns all rows from the right table and matching rows from the left table.
```sql
SELECT e.emp_name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

### FULL OUTER JOIN (Workaround in MySQL)
```sql
SELECT e.emp_name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
UNION
SELECT e.emp_name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

Joins are essential for normalized databases with relationships across multiple tables.

---

## 🔹 Transactions
Ensures atomicity of SQL operations.
```sql
START TRANSACTION;
UPDATE employees SET salary = salary + 1000 WHERE dept_id = 1;
COMMIT; -- or ROLLBACK;
```
- **START TRANSACTION** begins the block
- **COMMIT** saves changes
- **ROLLBACK** undoes changes in case of error

---

## 🔹 Stored Procedures
Reusable SQL blocks.
```sql
DELIMITER //
CREATE PROCEDURE GetEmployees()
BEGIN
  SELECT * FROM employees;
END //
DELIMITER ;
```
- Can accept parameters
- Encapsulate logic for reuse

---



