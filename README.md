# MySQL: Complete Guide with Examples

This document provides a comprehensive overview of MySQL, including theory and practical usage with examples. MySQL is one of the most popular relational database management systems (RDBMS) used in web development and data-driven applications.

---

## 🔹 What is MySQL?
MySQL is an open-source RDBMS based on Structured Query Language (SQL). It stores data in tables and supports various operations such as data insertion, querying, updating, and schema modifications.

## 🔹 Order of Execution?
```sql
/*(8)*/  SELECT /*9*/ DISTINCT /*11*/ TOP  
/*(1)*/  FROM
 /*(3)*/        
JOIN
 /*(2)*/       
ON
 /*(4)*/  WHERE
 /*(5)*/  GROUP BY
 /*(6)*/  WITH {CUBE | ROLLUP}
 /*(7)*/  HAVING
 /*(10)*/ ORDER BY
 /*(11)*/ LIMIT
```
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
### 7. Filtering Data

Filtering is used to retrieve only the specific rows that meet certain criteria using clauses like `WHERE`, `AND`, `OR`, `BETWEEN`, `IN`, and more.

- ####  WHERE – Basic filtering condition:
```sql
SELECT * FROM employees WHERE age > 30;

```
- ####  AND / OR – Combine multiple conditions:
```sql
SELECT * FROM employees WHERE age > 25 AND salary > 50000;
SELECT * FROM employees WHERE dept_id = 1 OR dept_id = 2;

```
- ####  BETWEEN – Match a range of values:
```sql
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 70000;

```

- #### IN – Match against a list of values:
```sql
SELECT * FROM employees WHERE dept_id IN (1, 3, 5);

```
- #### NOT IN – Exclude from a list:
```sql
SELECT * FROM employees WHERE dept_id NOT IN (2, 4);

```
- #### LIKE – Pattern matching for strings:
```sql
SELECT * FROM employees WHERE emp_name LIKE 'A%';   -- Starts with A
SELECT * FROM employees WHERE emp_name LIKE '%son'; -- Ends with 'son'

```
- ####  IS NULL / IS NOT NULL – Filter NULL values:
```sql
SELECT * FROM employees WHERE salary IS NOT NULL;

```
---
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
## 🔹Aggregate Functions
Aggregate functions perform calculations on sets of values and return a single value:

```sql
-- Basic syntax:
SELECT AGG_FUNC(column_name) FROM table_name [WHERE conditions];
```
### Common Aggregate Functions:
- ```COUNT()``` : Counts rows
  ```sql
  	SELECT COUNT(*) FROM employees;
  ```
- ```SUM()``` : Sums values
  ```sql
  SELECT SUM(salary) FROM employees;
  ```
- ```AVG()```: Averages values
  ```sql
  SELECT AVG(salary) FROM employees;
  ```
- ```MIN()```: Finds minimum value
  ```sql
  SELECT MIN(salary) FROM employees;
  ```
- ```MAX()```: Finds maximum value
  ```sql
  SELECT MAX(salary) FROM employees;
  ```
- ```GROUP_CONCAT()```: Concatenates values
  ```sql
  SELECT GROUP_CONCAT(name) FROM employees;
  ```
### Conditional Aggregation:
Conditional aggregation applies aggregate functions to specific subsets of data:

- #### Using ```CASE``` inside aggregates
  ```sql
  SELECT 
    SUM(CASE WHEN department = 'Sales' THEN salary ELSE 0 END) AS sales_payroll,
    AVG(CASE WHEN gender = 'F' THEN salary ELSE NULL END) AS avg_female_salary
  FROM employees;
  ```
- #### Using IF() function
  ```sql
  SELECT
    SUM(IF(status = 'Active', salary, 0)) AS active_payroll,
    COUNT(IF(joined_date > '2023-01-01', 1, NULL)) AS new_employees
  FROM employees;
  ```
- #### Using FILTER (MySQL 8.0+)
  ```sql
  SELECT
    SUM(salary) FILTER (WHERE department = 'IT') AS it_payroll,
    AVG(salary) FILTER (WHERE years_experience > 5) AS senior_avg_salary
  FROM employees;
  ```
- NULL Handling: Most aggregates ignore NULL values except COUNT(*)
---

## Window Functions in MySQL (8.0+)

Window functions perform calculations across a set of table rows that are somehow related to the current row, without collapsing them into a single output row like GROUP BY does.

### Basic Syntax:
```sql
SELECT 
    column1,
    column2,
    WINDOW_FUNCTION() OVER (
        [PARTITION BY partition_expression]
        [ORDER BY sort_expression]
        [frame_clause]
    ) AS result_column
FROM table_name;
```
### ROW_NUMBER()
  - Assigns a unique sequential integer to rows within a partition of the result set
  - example: Get the top-earning employee in each department.
  - ```sql
    SELECT 
    employee_id, 
    department_id, 
    salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS row_num
    FROM employees;
    ```
### RANK() and DENSE_RANK()
  - Both are window functions used to assign rankings to rows in a result set, but with different handling of ties
  - ```sql
    ```sql
    SELECT 
    column_name,
    RANK() OVER (ORDER BY sort_column) AS rank_col,
    DENSE_RANK() OVER (ORDER BY sort_column) AS dense_rank_col
    FROM table_name;
    ```
  - #### RANK() Function:
    - Assigns a unique rank to each distinct row
    - Skips subsequent ranks after ties (creates gaps)
    - Follows "Olympic medal" ranking style
  - #### DENSE_RANK() Function:
    - Assigns a unique rank to each distinct row
    - Never skips ranks (no gaps after ties)
    - Follows "sequential numbering" style
### NTILE(n):
  - Divides the result set into n buckets and assigns a bucket number to each row.
  - ```sql
    SELECT 
    employee_id,
    salary,
    NTILE(4) OVER (ORDER BY salary DESC) AS salary_quartile
    FROM employees;

    ```
 ### LAG() and LEAD():
  - LAG() accesses data from a previous row.
  - LEAD() accesses data from a following row.
  - Use case: Compare current salary with previous and next employee's salary
  - ```sql
    SELECT 
    employee_id, 
    salary,
    LAG(salary, 1) OVER (ORDER BY hire_date) AS prev_salary,
    LEAD(salary, 1) OVER (ORDER BY hire_date) AS next_salary
    FROM employees;

    ```
 ### FIRST_VALUE() and LAST_VALUE():
  - FIRST_VALUE() returns the first value in the window.
  - LAST_VALUE() returns the last value in the window.
  - ```sql
    SELECT 
    employee_id,
    salary,
    FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date) AS first_salary,
    LAST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_salary
    FROM employees;
    ```
 ### Aggregate Functions as Window Functions
  - Any aggregate function (like SUM, AVG, COUNT, MIN, MAX) can be used as a window function.
  - ```sql
    SELECT 
    employee_id,
    department_id,
    salary,
    AVG(salary) OVER (PARTITION BY department_id) AS avg_dept_salary,
    SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT       ROW) AS running_total
    FROM employees;

    ```
 ### Window Frame Clauses
   - ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
   - ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
   - RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
   - ```sql
     SELECT 
    employee_id,
    salary,
    SUM(salary) OVER (
        ORDER BY hire_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_sum
    FROM employees;

     ```
---

## 🔹 Indexes in MySQL

Indexes in MySQL are used to speed up the retrieval of rows from a table. They help optimize queries that use `SELECT`, `JOIN`, `WHERE`, `ORDER BY`, and `GROUP BY`. While indexes improve read performance, they can slightly degrade write performance (like `INSERT`, `UPDATE`, `DELETE`) due to the overhead of maintaining the index structure.

---

### Why Use Indexes?
- Improve the speed of data retrieval
- Enhance performance of queries with conditions and joins
- Enforce uniqueness using `UNIQUE` or `PRIMARY KEY`
- Reduce disk I/O for large tables

> ⚠️ **Note:** Overusing indexes can negatively impact write operations.

---

###  Creating and Dropping Indexes

###  Create an Index

```sql
-- Single-column index
CREATE INDEX index_name ON table_name(column_name);

-- Composite index (multiple columns)
CREATE INDEX index_name ON table_name(col1, col2, ...);

-- Unique index (enforce uniqueness)
CREATE UNIQUE INDEX index_name ON table_name(column_name);

-- Full-text index (for text-based searches)
CREATE FULLTEXT INDEX index_name ON table_name(text_column);
```
- Use composite indexes for queries filtering/sorting on multiple columns

- Unique indexes prevent duplicate values in the indexed column(s)

- Full-text indexes are optimized for ```MATCH() AGAINST()``` queries on large text fields

- Index order matters in composite indexes (leftmost prefix principle)
  
###  Create an Index
```sql
-- Standard index
DROP INDEX index_name ON table_name;

-- Primary Key index (requires ALTER TABLE)
ALTER TABLE table_name DROP PRIMARY KEY;
```
- Dropping indexes frees up storage space and reduces write overhead

- Primary Key indexes cannot be dropped with DROP INDEX (use ALTER TABLE)

- Dropping a non-existent index throws an error (check existence first)
  
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



