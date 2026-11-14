# SQL for Data Analytics — 25-Day Learning Series

A structured, beginner-to-advanced SQL guide for data analytics. This repository contains the concepts, syntax, examples, and practice tasks covered in the 25-day SQL learning series.

> **SQL dialect:** Examples use MySQL 8.0-compatible syntax unless stated otherwise. Small differences may exist in PostgreSQL, SQL Server, and SQLite.

## Table of Contents

1. [Day 1 — Introduction to SQL](#day-1--introduction-to-sql)
2. [Day 2 — Types of SQL Commands](#day-2--types-of-sql-commands)
3. [Day 3 — SELECT, DISTINCT, and Aliases](#day-3--select-distinct-and-aliases)
4. [Day 4 — Filtering and Sorting Data](#day-4--filtering-and-sorting-data)
5. [Day 5 — Aggregate Functions](#day-5--aggregate-functions)
6. [Day 6 — GROUP BY and HAVING](#day-6--group-by-and-having)
7. [Day 7 — Introduction to SQL Joins](#day-7--introduction-to-sql-joins)
8. [Day 8 — Types of SQL Joins](#day-8--types-of-sql-joins)
9. [Day 9 — SQL Subqueries](#day-9--sql-subqueries)
10. [Day 10 — Advanced Subqueries](#day-10--advanced-subqueries)
11. [Day 11 — SQL Constraints](#day-11--sql-constraints)
12. [Day 12 — SQL Data Types](#day-12--sql-data-types)
13. [Day 13 — SQL Operators](#day-13--sql-operators)
14. [Day 14 — NULL Values and NULL Functions](#day-14--null-values-and-null-functions)
15. [Day 15 — CASE Expressions](#day-15--case-expressions)
16. [Day 16 — SQL String Functions](#day-16--sql-string-functions)
17. [Day 17 — SQL Date and Time Functions](#day-17--sql-date-and-time-functions)
18. [Day 18 — SQL Set Operators](#day-18--sql-set-operators)
19. [Day 19 — Common Table Expressions (CTEs)](#day-19--common-table-expressions-ctes)
20. [Day 20 — SQL Views](#day-20--sql-views)
21. [Day 21 — Keys and Table Relationships](#day-21--keys-and-table-relationships)
22. [Day 22 — Window Functions](#day-22--window-functions)
23. [Day 23 — Ranking Functions](#day-23--ranking-functions)
24. [Day 24 — SQL Query Optimization](#day-24--sql-query-optimization)
25. [Day 25 — SQL Interview Questions and Mini-Project](#day-25--sql-interview-questions-and-mini-project)

---

## Day 1 — Introduction to SQL

### What is SQL?

SQL (Structured Query Language) is a standard language used to communicate with relational database management systems. It allows users to create and manage database objects, insert and modify records, retrieve information, and analyze structured data.

### What is a database?

A database is an organized collection of data that can be stored, accessed, managed, and updated efficiently.

### What is an RDBMS?

A Relational Database Management System (RDBMS) stores data in related tables made up of rows and columns. Relationships between tables are commonly established using keys.

Examples include MySQL, PostgreSQL, Microsoft SQL Server, Oracle Database, and SQLite.

### Basic database terms

- **Table:** A structure that stores related data in rows and columns.
- **Row/Record:** One complete entry in a table.
- **Column/Field:** An attribute that describes the data stored in a table.
- **Primary key:** A column or group of columns that uniquely identifies each row.
- **Query:** A command sent to a database to perform an operation.

### Example

```sql
CREATE DATABASE company_db;

USE company_db;

CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10, 2)
);
```

### Why SQL matters in data analytics

- Retrieves data from databases.
- Filters and organizes large datasets.
- Combines information from multiple tables.
- Calculates summaries and business metrics.
- Supports reporting and data-driven decisions.

**Practice:** Explain the difference between a database, a table, a row, and a column.

---

## Day 2 — Types of SQL Commands

SQL commands are commonly grouped according to the type of database operation they perform.

### 1. DDL — Data Definition Language

Defines or changes database structures.

```sql
CREATE TABLE departments (
    department_id INT,
    department_name VARCHAR(50)
);

ALTER TABLE departments
ADD location VARCHAR(50);

TRUNCATE TABLE departments;

DROP TABLE departments;
```

- `CREATE`: Creates database objects.
- `ALTER`: Changes the structure of an object.
- `TRUNCATE`: Removes all rows while keeping the table structure.
- `DROP`: Removes a database object.

### 2. DML — Data Manipulation Language

Adds, modifies, or removes table records.

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary)
VALUES
(1, 'Ananya', 'IT', 65000);

UPDATE employees
SET salary = 70000
WHERE employee_id = 1;

DELETE FROM employees
WHERE employee_id = 1;
```

### 3. DQL — Data Query Language

Retrieves data from tables.

```sql
SELECT * FROM employees;
```

### 4. DCL — Data Control Language

Manages database permissions.

```sql
GRANT SELECT ON company_db.employees TO 'analyst'@'localhost';

REVOKE SELECT ON company_db.employees FROM 'analyst'@'localhost';
```

### 5. TCL — Transaction Control Language

Manages transactions.

```sql
START TRANSACTION;

UPDATE employees
SET salary = salary + 2000
WHERE employee_id = 2;

COMMIT;
-- Use ROLLBACK instead of COMMIT to undo uncommitted changes.
```

**Practice:** Classify `CREATE`, `INSERT`, `SELECT`, `GRANT`, and `COMMIT` into their command groups.

---

## Day 3 — SELECT, DISTINCT, and Aliases

### SELECT

`SELECT` retrieves one or more columns from a table.

```sql
SELECT employee_name, salary
FROM employees;
```

To retrieve every column:

```sql
SELECT *
FROM employees;
```

### DISTINCT

`DISTINCT` removes duplicate combinations from the selected result.

```sql
SELECT DISTINCT department
FROM employees;
```

For more than one column, uniqueness is checked across the selected column combination:

```sql
SELECT DISTINCT department, location
FROM employees;
```

### Column aliases

An alias gives a column or expression a temporary display name in the query result.

```sql
SELECT
    employee_name AS name,
    salary AS monthly_salary
FROM employees;
```

### Calculated columns

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

**Practice:** Display employee names and salaries, list unique departments, and show annual salary using an alias.

---

## Day 4 — Filtering and Sorting Data

### WHERE

`WHERE` filters rows according to a condition.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

### ORDER BY

`ORDER BY` sorts the result by one or more columns.

```sql
-- Lowest salary first
SELECT *
FROM employees
ORDER BY salary ASC;

-- Highest salary first
SELECT *
FROM employees
ORDER BY salary DESC;
```

### LIMIT

`LIMIT` restricts the number of rows returned.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

### Combining filtering and sorting

```sql
SELECT employee_name, department, salary
FROM employees
WHERE department = 'IT'
ORDER BY salary DESC
LIMIT 3;
```

**Remember:** In a basic query, `WHERE` filters rows before the result is sorted by `ORDER BY`.

**Practice:** Find employees earning more than 40,000 and display them from highest to lowest salary.

---

## Day 5 — Aggregate Functions

Aggregate functions calculate a single result from a set of rows.

Assume an `employees` table with a `salary` column.

### COUNT()

Counts rows or non-NULL values, depending on the expression.

```sql
SELECT COUNT(*) AS employee_count
FROM employees;

SELECT COUNT(salary) AS employees_with_salary
FROM employees;
```

### SUM()

Calculates the total of numeric values.

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

### AVG()

Calculates the average of non-NULL numeric values.

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

### MIN() and MAX()

Return the smallest and largest values.

```sql
SELECT
    MIN(salary) AS lowest_salary,
    MAX(salary) AS highest_salary
FROM employees;
```

### Using multiple aggregate functions

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS payroll,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees;
```

**Practice:** Calculate the total number of employees, total salary, and average salary.

---

## Day 6 — GROUP BY and HAVING

### GROUP BY

`GROUP BY` groups rows that share the same values in specified columns. It is commonly used with aggregate functions to calculate summaries for each group.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

### Multiple grouping columns

```sql
SELECT department, location, AVG(salary) AS average_salary
FROM employees
GROUP BY department, location;
```

### HAVING

`HAVING` filters groups after grouping and aggregation.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

### WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters grouped results |
| Applied before grouping | Applied after grouping |
| Usually does not filter aggregate results | Commonly filters using aggregates |

### Combined example

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY employee_count DESC;
```

**Practice:** Find departments with at least three employees and calculate their average salary.

---

## Day 7 — Introduction to SQL Joins

### What is a JOIN?

A JOIN combines related rows from two or more tables using a matching condition. It allows analysts to retrieve connected information stored in separate tables.

Example tables:

**employees**

| employee_id | employee_name | department_id |
|---:|---|---:|
| 1 | Ananya | 10 |
| 2 | Rahul | 20 |
| 3 | Priya | 10 |

**departments**

| department_id | department_name |
|---:|---|
| 10 | IT |
| 20 | HR |
| 30 | Finance |

### Basic JOIN syntax

```sql
SELECT columns
FROM table1
JOIN table2
    ON table1.common_column = table2.common_column;
```

### INNER JOIN

Returns only rows with matching values in both tables.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

**Why joins matter:** Business data is often distributed across customers, orders, products, employees, and other related tables. Joins bring the required information together for analysis.

**Practice:** Display each employee's name along with their department name.

---

## Day 8 — Types of SQL Joins

### 1. INNER JOIN

Returns only matching rows from both tables.

```sql
SELECT e.employee_name, d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

### 2. LEFT JOIN

Returns every row from the left table and matching rows from the right table. If no match exists, right-table columns contain `NULL`.

```sql
SELECT e.employee_name, d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

### 3. RIGHT JOIN

Returns every row from the right table and matching rows from the left table.

```sql
SELECT e.employee_name, d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

### 4. CROSS JOIN

Returns the Cartesian product: every row from the first table paired with every row from the second table.

```sql
SELECT e.employee_name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

### 5. SELF JOIN

Joins a table to itself. Useful for hierarchical data such as employees and their managers.

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

**Note:** MySQL does not support `FULL OUTER JOIN` directly. A full-join-like result can be built using `LEFT JOIN` and `RIGHT JOIN` with `UNION` (or another carefully designed query).

**Practice:** Find all departments, including departments that currently have no employees.

---

## Day 9 — SQL Subqueries

### What is a subquery?

A subquery is a query nested inside another SQL statement. The outer query can use the result returned by the inner query to filter, compare, or construct its result.

### 1. Subquery with WHERE

Find employees whose salary is greater than the overall average salary.

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### 2. Subquery with IN

Find employees who work in departments located in Hyderabad.

```sql
SELECT employee_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location = 'Hyderabad'
);
```

### 3. Subquery in FROM

A subquery in the `FROM` clause creates a derived table that the outer query can read.

```sql
SELECT department_summary.department_id,
       department_summary.average_salary
FROM (
    SELECT department_id, AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
) AS department_summary;
```

### 4. Scalar subquery

A scalar subquery returns one value, such as the average salary in the first example.

**Practice:** Find products priced above the average price in the products table.

---

## Day 10 — Advanced Subqueries

### 1. Correlated subquery

A correlated subquery refers to a column from the outer query. It is logically evaluated in relation to each outer row.

Find employees earning more than the average salary in their own department:

```sql
SELECT e.employee_name, e.department_id, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

### 2. EXISTS

`EXISTS` tests whether a subquery returns at least one row.

```sql
SELECT d.department_id, d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

### 3. NOT EXISTS

Find departments that have no employees.

```sql
SELECT d.department_id, d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

### 4. ANY and ALL

`ANY` compares a value with at least one value returned by a subquery. `ALL` requires the comparison to be true for every returned value.

```sql
-- Greater than at least one salary in department 10
SELECT employee_name, salary
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);

-- Greater than every salary in department 10
SELECT employee_name, salary
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

**Practice:** Use `EXISTS` to find customers who have placed at least one order.

---

## Day 11 — SQL Constraints

### What are SQL constraints?

SQL constraints are rules applied to table columns or tables to restrict the data that can be stored. They help maintain data accuracy, consistency, and integrity.

### 1. NOT NULL

Prevents a column from storing `NULL` values.

```sql
CREATE TABLE students (
    student_id INT,
    student_name VARCHAR(100) NOT NULL
);
```

### 2. UNIQUE

Ensures that values in a column or a combination of columns are unique.

```sql
CREATE TABLE users (
    user_id INT,
    email VARCHAR(150) UNIQUE
);
```

### 3. PRIMARY KEY

Uniquely identifies each row. A primary key must be unique and cannot contain `NULL`.

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);
```

### 4. FOREIGN KEY

Enforces a relationship by requiring values to match a referenced candidate key, usually a primary key, in another table.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

### 5. CHECK

Restricts values according to a condition.

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    price DECIMAL(10, 2) CHECK (price >= 0)
);
```

### 6. DEFAULT

Supplies a value when an insert does not provide one for that column.

```sql
CREATE TABLE tasks (
    task_id INT PRIMARY KEY,
    status VARCHAR(30) DEFAULT 'Pending'
);
```

**Practice:** Create an `orders` table with a primary key, a non-negative amount check, and a default order status.

---

## Day 12 — SQL Data Types

A data type defines what kind of value a column can store. Choosing suitable data types helps preserve meaning, validate input, and use storage efficiently.

### 1. Numeric data types

| Type | Purpose |
|---|---|
| `INT` | Whole numbers |
| `BIGINT` | Large whole numbers |
| `DECIMAL(p,s)` | Exact decimal values, useful for money |
| `FLOAT` | Approximate floating-point values |
| `DOUBLE` | Approximate floating-point values with greater precision than FLOAT |

Example:

```sql
CREATE TABLE products (
    product_id INT,
    price DECIMAL(10, 2),
    stock_quantity INT
);
```

### 2. Character and text types

| Type | Purpose |
|---|---|
| `CHAR(n)` | Fixed-length character strings |
| `VARCHAR(n)` | Variable-length character strings |
| `TEXT` | Longer text content |

### 3. Date and time types

| Type | Purpose |
|---|---|
| `DATE` | Calendar date |
| `TIME` | Time of day |
| `DATETIME` | Date and time |
| `TIMESTAMP` | Timestamp with time-zone conversion behavior based on session settings in MySQL |

### 4. Boolean values

MySQL supports `BOOLEAN` as a synonym for `TINYINT(1)`; commonly, `0` represents false and `1` represents true.

```sql
CREATE TABLE users (
    user_id INT,
    is_active BOOLEAN
);
```

**Practice:** Choose appropriate data types for employee ID, employee name, salary, joining date, and active status.

---

## Day 13 — SQL Operators

Operators perform calculations, comparisons, and logical tests in SQL expressions.

### 1. Arithmetic operators

| Operator | Meaning |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulo/remainder |

```sql
SELECT
    product_name,
    price,
    price * 0.10 AS discount_amount
FROM products;
```

### 2. Comparison operators

| Operator | Meaning |
|---|---|
| `=` | Equal to |
| `<>` or `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

```sql
SELECT *
FROM employees
WHERE salary >= 50000;
```

### 3. Logical operators

```sql
SELECT *
FROM employees
WHERE department = 'IT' AND salary > 50000;

SELECT *
FROM employees
WHERE department = 'IT' OR department = 'HR';

SELECT *
FROM employees
WHERE NOT department = 'Finance';
```

### 4. Special operators

```sql
-- Match any listed department
SELECT * FROM employees
WHERE department IN ('IT', 'HR');

-- Match a range, including both endpoints
SELECT * FROM employees
WHERE salary BETWEEN 30000 AND 60000;

-- Match a pattern
SELECT * FROM employees
WHERE employee_name LIKE 'A%';
```

**Practice:** Find employees in IT or HR whose salary is between 30,000 and 70,000.

---

## Day 14 — NULL Values and NULL Functions

### What is NULL?

`NULL` represents a missing, unknown, or unavailable value. It is not the same as zero, an empty string, or a blank space.

### IS NULL and IS NOT NULL

Do not use `= NULL` to test for missing values. Use `IS NULL` or `IS NOT NULL`.

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;

SELECT *
FROM employees
WHERE manager_id IS NOT NULL;
```

### COALESCE()

Returns the first non-NULL expression in its argument list.

```sql
SELECT
    employee_name,
    COALESCE(commission, 0) AS commission
FROM employees;
```

### IFNULL() — MySQL

Returns an alternative value when the first expression is `NULL`.

```sql
SELECT
    employee_name,
    IFNULL(commission, 0) AS commission
FROM employees;
```

### NULLIF()

Returns `NULL` if its two expressions are equal; otherwise, it returns the first expression.

```sql
SELECT revenue / NULLIF(quantity, 0) AS revenue_per_unit
FROM sales;
```

### Important NULL behavior

- Comparisons with `NULL` generally produce `UNKNOWN`, not `TRUE` or `FALSE`.
- Aggregate functions such as `SUM()` and `AVG()` ignore NULL values.
- `COUNT(*)` counts rows, while `COUNT(column)` counts non-NULL values in that column.

**Practice:** Find employees without a manager and display missing commissions as zero.

---

## Day 15 — CASE Expressions

### What is CASE?

`CASE` is a conditional expression that evaluates conditions in order and returns a result for the first matching condition. It is useful for categorizing data and creating conditional output columns.

### Searched CASE

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 80000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### Simple CASE

```sql
SELECT
    employee_name,
    department,
    CASE department
        WHEN 'IT' THEN 'Technology'
        WHEN 'HR' THEN 'Human Resources'
        WHEN 'Finance' THEN 'Accounts'
        ELSE 'Other'
    END AS department_group
FROM employees;
```

### CASE with aggregation

```sql
SELECT
    SUM(CASE WHEN status = 'Completed' THEN 1 ELSE 0 END)
        AS completed_tasks,
    SUM(CASE WHEN status = 'Pending' THEN 1 ELSE 0 END)
        AS pending_tasks
FROM tasks;
```

**Practice:** Categorize orders as `High Value`, `Medium Value`, or `Low Value` based on their amount.

---

## Day 16 — SQL String Functions

String functions manipulate, inspect, and transform text values.

### UPPER() and LOWER()

Convert text to uppercase or lowercase.

```sql
SELECT UPPER(employee_name) AS uppercase_name,
       LOWER(employee_name) AS lowercase_name
FROM employees;
```

### LENGTH() and CHAR_LENGTH()

In MySQL, `LENGTH()` returns the number of bytes, while `CHAR_LENGTH()` returns the number of characters.

```sql
SELECT
    LENGTH(employee_name) AS byte_length,
    CHAR_LENGTH(employee_name) AS character_count
FROM employees;
```

### CONCAT()

Combines strings.

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

### SUBSTRING()

Extracts part of a string.

```sql
SELECT SUBSTRING(employee_name, 1, 3) AS short_name
FROM employees;
```

### TRIM()

Removes leading and trailing spaces by default.

```sql
SELECT TRIM('   Data Analytics   ') AS cleaned_text;
```

### REPLACE()

Replaces occurrences of one substring with another.

```sql
SELECT REPLACE(phone_number, '-', '') AS cleaned_phone
FROM customers;
```

**Practice:** Convert customer names to uppercase, remove extra spaces, and combine first and last names.

---

## Day 17 — SQL Date and Time Functions

Date and time functions help extract date parts, compare dates, calculate intervals, and prepare time-based reports.

### Current date and time

```sql
SELECT CURDATE() AS current_date;
SELECT NOW() AS current_datetime;
```

### Extract date parts

```sql
SELECT
    YEAR(order_date) AS order_year,
    MONTH(order_date) AS order_month,
    DAY(order_date) AS order_day
FROM orders;
```

### DATE_FORMAT()

Formats a date as text in MySQL.

```sql
SELECT DATE_FORMAT(order_date, '%d-%m-%Y') AS formatted_date
FROM orders;
```

### DATEDIFF()

Returns the number of days between two dates.

```sql
SELECT DATEDIFF(delivery_date, order_date) AS delivery_days
FROM orders;
```

### DATE_ADD() and DATE_SUB()

Add or subtract a time interval.

```sql
SELECT DATE_ADD(order_date, INTERVAL 7 DAY) AS expected_date
FROM orders;

SELECT DATE_SUB(order_date, INTERVAL 1 MONTH) AS previous_month_date
FROM orders;
```

### Filtering a date range

For a timestamp column, a half-open interval is often reliable:

```sql
SELECT *
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date <  '2026-02-01';
```

**Practice:** Calculate delivery time in days and count orders placed in each month.

---

## Day 18 — SQL Set Operators

Set operators combine the results of two or more `SELECT` statements. The queries must return the same number of columns, with compatible data types in corresponding positions.

### UNION

Combines results and removes duplicate rows.

```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers;
```

### UNION ALL

Combines results and keeps duplicates. It is generally more efficient than `UNION` when duplicate removal is not required.

```sql
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

### INTERSECT

Returns rows common to both query results. Supported in MySQL 8.0.31 and later.

```sql
SELECT city FROM customers
INTERSECT
SELECT city FROM suppliers;
```

### EXCEPT

Returns rows from the first query that do not appear in the second. Supported in MySQL 8.0.31 and later.

```sql
SELECT city FROM customers
EXCEPT
SELECT city FROM suppliers;
```

### Important points

- The number of selected columns must match.
- Corresponding columns should have compatible data types.
- Column names in the final result are generally taken from the first `SELECT`.
- `ORDER BY` is applied to the combined result and normally appears at the end.

**Practice:** List all cities from customers and suppliers, first with duplicates and then without duplicates.

---

## Day 19 — Common Table Expressions (CTEs)

### What is a CTE?

A Common Table Expression (CTE) is a named temporary result set defined with the `WITH` clause. It exists only for the duration of the statement and can make complex queries easier to read and maintain.

### Basic CTE

```sql
WITH high_salary_employees AS (
    SELECT employee_id, employee_name, salary
    FROM employees
    WHERE salary > 60000
)
SELECT *
FROM high_salary_employees;
```

### CTE with aggregation

```sql
WITH department_summary AS (
    SELECT
        department_id,
        COUNT(*) AS employee_count,
        AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_summary
WHERE average_salary > 50000;
```

### Multiple CTEs

```sql
WITH department_counts AS (
    SELECT department_id, COUNT(*) AS employee_count
    FROM employees
    GROUP BY department_id
),
large_departments AS (
    SELECT department_id, employee_count
    FROM department_counts
    WHERE employee_count >= 5
)
SELECT *
FROM large_departments;
```

### Recursive CTE

A recursive CTE can work with hierarchical or sequential data.

```sql
WITH RECURSIVE number_series AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1
    FROM number_series
    WHERE n < 5
)
SELECT n
FROM number_series;
```

**Practice:** Use a CTE to calculate department-wise average salary and return departments above the company average.

---

## Day 20 — SQL Views

### What is a View?

A view is a named query that presents data from one or more underlying tables as a virtual table. A regular view stores the query definition rather than storing a separate copy of the result data.

### Create a view

```sql
CREATE VIEW employee_details AS
SELECT
    e.employee_id,
    e.employee_name,
    d.department_name,
    e.salary
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

### Query a view

```sql
SELECT *
FROM employee_details;
```

### Replace a view definition

```sql
CREATE OR REPLACE VIEW high_salary_employees AS
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary > 60000;
```

### Drop a view

```sql
DROP VIEW IF EXISTS high_salary_employees;
```

### Benefits of views

- Simplify repeated complex queries.
- Present selected columns to users.
- Support consistent reporting logic.
- Can help restrict access to underlying columns when combined with suitable permissions.

**Note:** A view is not automatically a performance optimization or a security boundary by itself. Permissions and database design still matter.

**Practice:** Create a view showing employee names and department names, then query it.

---

## Day 21 — Keys and Table Relationships

Keys identify records and connect related tables.

### 1. Primary key

Uniquely identifies each row and cannot contain `NULL`.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100)
);
```

### 2. Foreign key

References a key in another table and enforces referential integrity.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

### 3. Composite key

A key formed from two or more columns.

```sql
CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    PRIMARY KEY (student_id, course_id)
);
```

### 4. Candidate key

A minimal set of column(s) that can uniquely identify a row. One candidate key is selected as the primary key.

### 5. Alternate key

A candidate key that was not selected as the primary key.

### Common table relationships

- **One-to-one:** One row in a table relates to at most one row in another table.
- **One-to-many:** One row in a parent table can relate to multiple rows in a child table.
- **Many-to-many:** Multiple rows in each table can relate to multiple rows in the other; a junction table is commonly used.

### Example: many-to-many relationship

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

**Practice:** Design customer, order, and order-item tables and identify their primary and foreign keys.

---

## Day 22 — Window Functions

### What is a window function?

A window function performs a calculation across a set of related rows while retaining the individual rows in the result. Unlike `GROUP BY`, it does not collapse each group into one output row.

### OVER()

The `OVER()` clause defines the window of rows used by the function.

```sql
SELECT
    employee_name,
    department,
    salary,
    AVG(salary) OVER () AS company_average_salary
FROM employees;
```

### PARTITION BY

Divides rows into groups for the window calculation.

```sql
SELECT
    employee_name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS department_average_salary
FROM employees;
```

### ORDER BY inside a window

```sql
SELECT
    employee_name,
    salary,
    SUM(salary) OVER (
        ORDER BY employee_id
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_salary_total
FROM employees;
```

### LAG() and LEAD()

Access values from a previous or following row within the window order.

```sql
SELECT
    order_date,
    sales_amount,
    LAG(sales_amount) OVER (ORDER BY order_date) AS previous_sales,
    LEAD(sales_amount) OVER (ORDER BY order_date) AS next_sales
FROM daily_sales;
```

**Practice:** Display each employee's salary beside their department's average salary without grouping away individual employees.

---

## Day 23 — Ranking Functions

Ranking functions assign a position to each row within an ordered window.

### ROW_NUMBER()

Assigns a unique sequential number to each row.

```sql
SELECT
    employee_name,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

### RANK()

Assigns the same rank to tied values and leaves gaps after ties.

```sql
SELECT
    employee_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

### DENSE_RANK()

Assigns the same rank to tied values but does not leave gaps.

```sql
SELECT
    employee_name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_salary_rank
FROM employees;
```

### Difference

| Function | Handles ties | Leaves gaps after ties |
|---|---|---|
| `ROW_NUMBER()` | Gives each row a different number | No |
| `RANK()` | Same rank for ties | Yes |
| `DENSE_RANK()` | Same rank for ties | No |

### Find the top three salaries in each department

```sql
WITH ranked_employees AS (
    SELECT
        employee_id,
        employee_name,
        department,
        salary,
        DENSE_RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)
SELECT *
FROM ranked_employees
WHERE salary_rank <= 3;
```

**Practice:** Find the second-highest distinct salary in each department using `DENSE_RANK()`.

---

## Day 24 — SQL Query Optimization

### What is SQL query optimization?

SQL query optimization is the process of improving how a query retrieves or modifies data so it can execute efficiently while returning the correct result.

### 1. Select only the required columns

Avoid retrieving unnecessary columns.

```sql
-- Less efficient when only two columns are needed
SELECT *
FROM employees;

-- Prefer
SELECT employee_id, employee_name
FROM employees;
```

### 2. Filter rows appropriately

```sql
SELECT employee_id, employee_name
FROM employees
WHERE department_id = 10;
```

### 3. Use indexes thoughtfully

Indexes can help the database locate rows faster, especially for columns frequently used in filters and joins. They also require storage and can slow down inserts, updates, and deletes.

```sql
CREATE INDEX idx_employees_department
ON employees(department_id);
```

### 4. Inspect the execution plan

Use `EXPLAIN` to inspect how MySQL plans to execute a query.

```sql
EXPLAIN
SELECT employee_name
FROM employees
WHERE department_id = 10;
```

### 5. Write efficient joins

Join tables using appropriate keys and ensure join columns have compatible data types.

```sql
SELECT e.employee_name, d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.department_id;
```

### 6. Avoid unnecessary operations

- Avoid unnecessary `DISTINCT` if duplicates are not a problem.
- Filter rows as early as practical.
- Avoid applying functions to indexed filter columns when a range condition can express the same logic.
- Use `UNION ALL` when duplicate removal is not required.
- Return only the rows needed.

### 7. Measure before and after

Compare execution plans and query performance on representative data. Optimization should be based on evidence, not assumptions.

**Practice:** Run `EXPLAIN` on a query with and without an appropriate index and compare the execution plans.

---

## Day 25 — SQL Interview Questions and Mini-Project

### Common SQL interview questions

**1. What is SQL?**  
SQL is a language used to define, query, and manipulate data in relational databases.

**2. What is the difference between WHERE and HAVING?**  
`WHERE` filters rows before grouping; `HAVING` filters groups after aggregation.

**3. What is the difference between DELETE, TRUNCATE, and DROP?**  
`DELETE` removes selected rows (or all rows if no `WHERE` is used), `TRUNCATE` removes all rows while keeping the table structure, and `DROP` removes the table itself.

**4. What is a primary key?**  
A primary key uniquely identifies each row in a table and cannot contain `NULL`.

**5. What is a foreign key?**  
A foreign key enforces a relationship by referencing a key in another table (or the same table).

**6. What is the difference between INNER JOIN and LEFT JOIN?**  
`INNER JOIN` returns matching rows; `LEFT JOIN` returns every left-table row and matching right-table data where available.

**7. What is a subquery?**  
A subquery is a query nested inside another SQL statement.

**8. What is a CTE?**  
A CTE is a named temporary result set defined using `WITH` for one SQL statement.

**9. What is the difference between UNION and UNION ALL?**  
`UNION` removes duplicate rows; `UNION ALL` keeps them.

**10. What is an index?**  
An index is a data structure that can speed up data retrieval, at the cost of storage and additional work during data changes.

**11. What is the difference between RANK() and DENSE_RANK()?**  
Both assign the same rank to ties, but `RANK()` leaves gaps and `DENSE_RANK()` does not.

**12. What is a window function?**  
A window function calculates across related rows while keeping individual rows in the result.

**13. How do you find duplicate values?**

```sql
SELECT email, COUNT(*) AS occurrence_count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;
```

**14. How do you find the second-highest distinct salary?**

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```

**15. How do you find employees earning above their department average?**

```sql
SELECT e.employee_name, e.department_id, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

### Mini-Project — Sales Data Analysis

**Objective:** Use SQL to analyze sales, customers, products, and orders and answer practical business questions.

#### Step 1: Create a database

```sql
CREATE DATABASE sales_analysis;
USE sales_analysis;
```

#### Step 2: Create tables

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    city VARCHAR(60),
    signup_date DATE
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(60),
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    status VARCHAR(30) DEFAULT 'Pending',
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10, 2) NOT NULL CHECK (unit_price >= 0),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

#### Step 3: Insert sample data

```sql
INSERT INTO customers VALUES
(1, 'Ananya', 'Hyderabad', '2025-01-10'),
(2, 'Rahul', 'Bengaluru', '2025-02-15'),
(3, 'Priya', 'Hyderabad', '2025-03-20'),
(4, 'Arjun', 'Chennai', '2025-04-05'),
(5, 'Meera', 'Bengaluru', '2025-05-12');

INSERT INTO products VALUES
(101, 'Laptop', 'Electronics', 55000.00),
(102, 'Smartphone', 'Electronics', 25000.00),
(103, 'Desk Chair', 'Furniture', 7000.00),
(104, 'Notebook', 'Stationery', 100.00),
(105, 'Monitor', 'Electronics', 15000.00);

INSERT INTO orders VALUES
(1001, 1, '2025-06-01', 'Completed'),
(1002, 2, '2025-06-03', 'Completed'),
(1003, 1, '2025-06-10', 'Pending'),
(1004, 3, '2025-06-12', 'Completed'),
(1005, 4, '2025-07-01', 'Completed'),
(1006, 5, '2025-07-05', 'Cancelled');

INSERT INTO order_items VALUES
(1, 1001, 101, 1, 55000.00),
(2, 1001, 104, 5, 100.00),
(3, 1002, 102, 1, 25000.00),
(4, 1003, 105, 2, 15000.00),
(5, 1004, 103, 2, 7000.00),
(6, 1005, 104, 10, 100.00),
(7, 1006, 102, 1, 25000.00);
```

#### Step 4: Business questions and SQL solutions

**1. Display all customers.**

```sql
SELECT *
FROM customers;
```

**2. Display completed orders.**

```sql
SELECT *
FROM orders
WHERE status = 'Completed';
```

**3. Calculate the total value of each order.**

```sql
SELECT
    order_id,
    SUM(quantity * unit_price) AS order_value
FROM order_items
GROUP BY order_id;
```

**4. Calculate total revenue from completed orders.**

```sql
SELECT
    SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM orders o
JOIN order_items oi
    ON o.order_id = oi.order_id
WHERE o.status = 'Completed';
```

**5. Find the top-selling products by quantity.**

```sql
SELECT
    p.product_name,
    SUM(oi.quantity) AS units_sold
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id
JOIN orders o
    ON oi.order_id = o.order_id
WHERE o.status = 'Completed'
GROUP BY p.product_id, p.product_name
ORDER BY units_sold DESC;
```

**6. Find revenue by product category.**

```sql
SELECT
    p.category,
    SUM(oi.quantity * oi.unit_price) AS category_revenue
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id
JOIN orders o
    ON oi.order_id = o.order_id
WHERE o.status = 'Completed'
GROUP BY p.category
ORDER BY category_revenue DESC;
```

**7. Find customers who have placed more than one order.**

```sql
SELECT
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) AS order_count
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 1;
```

**8. Find customers who have never placed an order.**

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

**9. Calculate monthly revenue from completed orders.**

```sql
SELECT
    DATE_FORMAT(o.order_date, '%Y-%m') AS sales_month,
    SUM(oi.quantity * oi.unit_price) AS monthly_revenue
FROM orders o
JOIN order_items oi
    ON o.order_id = oi.order_id
WHERE o.status = 'Completed'
GROUP BY DATE_FORMAT(o.order_date, '%Y-%m')
ORDER BY sales_month;
```

**10. Rank products by revenue.**

```sql
WITH product_revenue AS (
    SELECT
        p.product_id,
        p.product_name,
        SUM(oi.quantity * oi.unit_price) AS revenue
    FROM products p
    JOIN order_items oi
        ON p.product_id = oi.product_id
    JOIN orders o
        ON oi.order_id = o.order_id
    WHERE o.status = 'Completed'
    GROUP BY p.product_id, p.product_name
)
SELECT
    product_name,
    revenue,
    DENSE_RANK() OVER (ORDER BY revenue DESC) AS revenue_rank
FROM product_revenue;
```

### Mini-project extension tasks

Try solving these independently:

1. Find the highest-value completed order.
2. Calculate average order value.
3. Find the city generating the highest completed-order revenue.
4. Identify products that have never been ordered.
5. Compare monthly revenue using `LAG()`.
6. Create a view for completed-order details.
7. Use `EXPLAIN` to inspect one of your analysis queries.

### Project learning outcomes

By completing this project, you practise:

- Database and table creation.
- Primary and foreign keys.
- Data insertion and filtering.
- Aggregate functions and grouping.
- Joins and subqueries.
- CTEs and window functions.
- Business-focused sales analysis.

---

## SQL Learning Checklist

- [ ] Understand databases, tables, rows, and columns.
- [ ] Use DDL, DML, DQL, DCL, and TCL commands.
- [ ] Retrieve, filter, sort, and limit records.
- [ ] Apply aggregate functions, `GROUP BY`, and `HAVING`.
- [ ] Combine tables using joins.
- [ ] Write subqueries and correlated subqueries.
- [ ] Apply constraints and choose suitable data types.
- [ ] Use operators, NULL handling, and CASE expressions.
- [ ] Work with string and date/time functions.
- [ ] Combine query results using set operators.
- [ ] Write CTEs and create views.
- [ ] Understand keys and table relationships.
- [ ] Use window and ranking functions.
- [ ] Inspect query plans and apply optimization techniques.
- [ ] Complete the sales-analysis mini-project.

## Conclusion

SQL is an essential skill for data analysts because it enables them to retrieve, clean, transform, combine, and analyze data stored in relational databases. Completing these 25 days builds a foundation for solving practical data problems and preparing for SQL interviews.

**Keep practising: write queries, check the results, and learn by solving real datasets.**