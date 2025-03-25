# MySQL Basics with Examples

This project demonstrates basic and essential SQL commands using MySQL. It includes a sample schema with two tables: `departments` and `employees`, and shows usage of various SQL clauses and keywords.

## Database and Tables

- **CREATE DATABASE**: Creates a new database `sample_db`.
- **CREATE TABLE**: Creates two tables:
  - `departments` with `dept_id` as `PRIMARY KEY`
  - `employees` with `emp_id` as `PRIMARY KEY` and `dept_id` as a `FOREIGN KEY`

## Data Manipulation

- **INSERT INTO**: Adds records to both `departments` and `employees` tables.
- **UPDATE**: Increases salary for an employee.

## SQL Clauses and Keywords

### SELECT
Used to query data from the table.
```sql
SELECT * FROM employees;
```

### WHERE
Filters the rows based on condition.
```sql
SELECT * FROM employees WHERE age > 30;
```

### AND / OR
Combines multiple conditions.
```sql
SELECT * FROM employees WHERE age > 25 AND salary > 50000;
SELECT * FROM employees WHERE age < 30 OR salary < 50000;
```

### LIKE
Pattern matching.
```sql
SELECT * FROM employees WHERE emp_name LIKE 'A%';
```

### IN
Checks if value exists in a list.
```sql
SELECT * FROM employees WHERE dept_id IN (1, 3);
```

### EXISTS
Checks for the existence of a subquery result.
```sql
SELECT * FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.dept_id = e.dept_id);
```

### ORDER BY
Sorts the results.
```sql
SELECT * FROM employees ORDER BY salary DESC;
```

### GROUP BY
Groups rows with the same values and performs aggregate functions.
```sql
SELECT dept_id, COUNT(*) AS total_employees, AVG(salary) AS avg_salary FROM employees GROUP BY dept_id;
```

### CASE
Implements conditional logic.
```sql
SELECT emp_name, salary,
  CASE
    WHEN salary >= 60000 THEN 'High'
    WHEN salary >= 50000 THEN 'Medium'
    ELSE 'Low'
  END AS salary_band
FROM employees;
```

## Schema Modifications

### ALTER
Adds a new column.
```sql
ALTER TABLE employees ADD COLUMN join_date DATE;
```

## Constraints

- **PRIMARY KEY**: Ensures uniqueness for each record.
- **FOREIGN KEY**: Maintains referential integrity.

---

