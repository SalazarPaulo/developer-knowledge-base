# Oracle SQL and PL/SQL

Documentation and practical study notes about **Oracle Database**, **SQL**, and **PL/SQL**.

This module focuses on database fundamentals, SQL commands, PL/SQL programming, stored procedures, functions, triggers, cursors, transactions, database objects, and common technical interview topics.

## Objective

The objective of this module is to understand how to work with Oracle Database using SQL and PL/SQL for:

- Data definition
- Data manipulation
- Querying and reporting
- Transaction control
- Procedural programming
- Data validation
- Automation inside the database
- Technical interview preparation

By the end of this module, the reader should be able to:

- Create and modify database tables.
- Insert, update, delete, and query data.
- Use joins, subqueries, grouping, and aggregate functions.
- Manage transactions using `COMMIT`, `ROLLBACK`, and `SAVEPOINT`.
- Write PL/SQL blocks with variables, conditions, loops, and exceptions.
- Create procedures, functions, triggers, packages, cursors, sequences, views, and indexes.
- Understand how Oracle SQL and PL/SQL are used in real-world applications.
- Prepare for basic and intermediate Oracle SQL / PL/SQL technical interviews.

---

## Table of Contents

1. [Oracle SQL vs PL/SQL](#oracle-sql-vs-plsql)
2. [Basic Environment Commands](#basic-environment-commands)
3. [Oracle Data Types](#oracle-data-types)
4. [DDL Commands](#ddl-commands)
5. [Constraints](#constraints)
6. [DML Commands](#dml-commands)
7. [SELECT Queries](#select-queries)
8. [Joins](#joins)
9. [Subqueries](#subqueries)
10. [The DUAL Table](#the-dual-table)
11. [Oracle Functions](#oracle-functions)
12. [Transactions](#transactions)
13. [PL/SQL Block Structure](#plsql-block-structure)
14. [Variables in PL/SQL](#variables-in-plsql)
15. [Conditional Statements](#conditional-statements)
16. [Loops](#loops)
17. [SELECT INTO](#select-into)
18. [Exceptions](#exceptions)
19. [Cursors](#cursors)
20. [Procedures](#procedures)
21. [Functions](#functions)
22. [Packages](#packages)
23. [Triggers](#triggers)
24. [Sequences and Identity Columns](#sequences-and-identity-columns)
25. [Views](#views)
26. [Indexes](#indexes)
27. [Synonyms](#synonyms)
28. [Users, Roles, and Privileges](#users-roles-and-privileges)
29. [Dynamic SQL](#dynamic-sql)
30. [Collections](#collections)
31. [Bulk Processing](#bulk-processing)
32. [Data Dictionary Queries](#data-dictionary-queries)
33. [SQL Developer and SQL*Plus Useful Commands](#sql-developer-and-sqlplus-useful-commands)
34. [Mini Practice Project](#mini-practice-project)
35. [Main Command Summary](#main-command-summary)
36. [Common Errors](#common-errors)
37. [Technical Interview Questions](#technical-interview-questions)
38. [Recommended Study Order](#recommended-study-order)
39. [Study Checklist](#study-checklist)
40. [Recommended Resources](#recommended-resources)

---

## Sample Database Used in the Examples

The following sample tables are used to explain how each command affects the final result.

### `departments`

| department_id | department_name |
|---:|---|
| 1 | Technology |
| 2 | Finance |
| 3 | Human Resources |
| 4 | Operations |

### `employees`

| employee_id | first_name | last_name | email | salary | hire_date | department_id |
|---:|---|---|---|---:|---|---:|
| 101 | Ana | Torres | ana.torres@example.com | 1500 | 2024-01-10 | 1 |
| 102 | Luis | Perez | luis.perez@example.com | 1800 | 2024-02-15 | 1 |
| 103 | Marta | Gomez | marta.gomez@example.com | 1300 | 2024-03-20 | 2 |
| 104 | Carlos | Diaz | carlos.diaz@example.com | 2200 | 2024-04-05 | 2 |
| 105 | Pedro | Salazar | pedro.salazar@example.com | 3000 | 2024-05-12 | 3 |

### Base setup script

```sql
CREATE TABLE departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100) NOT NULL
);

CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    email VARCHAR2(100) UNIQUE,
    salary NUMBER(10,2),
    hire_date DATE,
    department_id NUMBER,
    CONSTRAINT fk_emp_dept
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);

INSERT INTO departments VALUES (1, 'Technology');
INSERT INTO departments VALUES (2, 'Finance');
INSERT INTO departments VALUES (3, 'Human Resources');
INSERT INTO departments VALUES (4, 'Operations');

INSERT INTO employees VALUES (101, 'Ana', 'Torres', 'ana.torres@example.com', 1500, DATE '2024-01-10', 1);
INSERT INTO employees VALUES (102, 'Luis', 'Perez', 'luis.perez@example.com', 1800, DATE '2024-02-15', 1);
INSERT INTO employees VALUES (103, 'Marta', 'Gomez', 'marta.gomez@example.com', 1300, DATE '2024-03-20', 2);
INSERT INTO employees VALUES (104, 'Carlos', 'Diaz', 'carlos.diaz@example.com', 2200, DATE '2024-04-05', 2);
INSERT INTO employees VALUES (105, 'Pedro', 'Salazar', 'pedro.salazar@example.com', 3000, DATE '2024-05-12', 3);

COMMIT;
```

These tables make it easier to understand the expected output of `SELECT`, `JOIN`, `GROUP BY`, subqueries, procedures, functions, and triggers.


---

## Oracle SQL vs PL/SQL

### SQL

**SQL** is used to interact with data stored in the database.

Common SQL tasks include:

- Creating tables.
- Inserting records.
- Updating records.
- Deleting records.
- Querying data.
- Joining tables.
- Managing transactions.

Example:

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 1000;
```

### PL/SQL

**PL/SQL** is Oracle's procedural extension for SQL. It allows developers to write programming logic inside the database.

PL/SQL is used for:

- Stored procedures.
- Functions.
- Triggers.
- Packages.
- Cursors.
- Exception handling.
- Loops and conditions.
- Database automation.

Example:

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello from PL/SQL');
END;
/
```



### Expected result example

Using the sample `employees` table:

```sql
DESC employees;
```

Expected structure:

| Column | Type | Nullable |
|---|---|---|
| EMPLOYEE_ID | NUMBER | No |
| FIRST_NAME | VARCHAR2(50) | No |
| LAST_NAME | VARCHAR2(50) | No |
| EMAIL | VARCHAR2(100) | Yes |
| SALARY | NUMBER(10,2) | Yes |
| HIRE_DATE | DATE | Yes |
| DEPARTMENT_ID | NUMBER | Yes |

If `SET SERVEROUTPUT ON` is enabled, this block:

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello from PL/SQL');
END;
/
```

Expected output:

```text
Hello from PL/SQL
```


---

## Basic Environment Commands

These commands are commonly used in SQL Developer, SQL*Plus, or Oracle command-line environments.

### Enable output

```sql
SET SERVEROUTPUT ON;
```

This allows messages printed with `DBMS_OUTPUT.PUT_LINE` to be displayed.

### Show current user

```sql
SHOW USER;
```

### Describe table structure

```sql
DESC employees;
```

### Execute a SQL script

```sql
@script.sql
```



### Expected result example

After creating `example_types`, insert a sample row:

```sql
INSERT INTO example_types (
    id,
    name,
    code,
    created_at,
    description
)
VALUES (
    1,
    'Oracle Example',
    'A1001',
    SYSDATE,
    'This is a CLOB text example.'
);
```

Query:

```sql
SELECT id, name, code, description
FROM example_types;
```

Expected result:

| id | name | code | description |
|---:|---|---|---|
| 1 | Oracle Example | A1001 | This is a CLOB text example. |

The result shows how each data type stores a different kind of value: numbers, text, fixed-size codes, dates, and long text.


---

## Oracle Data Types

| Data Type | Description | Example |
|---|---|---|
| `NUMBER` | Numeric values | `salary NUMBER(10,2)` |
| `VARCHAR2(n)` | Variable-length text | `name VARCHAR2(100)` |
| `CHAR(n)` | Fixed-length text | `gender CHAR(1)` |
| `DATE` | Date and time | `created_at DATE` |
| `TIMESTAMP` | Date and time with fractional seconds | `event_time TIMESTAMP` |
| `CLOB` | Large text data | `description CLOB` |
| `BLOB` | Binary data | `file_data BLOB` |
| `BOOLEAN` | True or false | Only available in PL/SQL, not SQL table columns |

Example:

```sql
CREATE TABLE example_types (
    id          NUMBER,
    name        VARCHAR2(100),
    code        CHAR(5),
    created_at  DATE,
    description CLOB
);
```

PL/SQL example:

```sql
DECLARE
    v_id        NUMBER := 1;
    v_name      VARCHAR2(100) := 'Ana';
    v_date      DATE := SYSDATE;
    v_active    BOOLEAN := TRUE;
BEGIN
    DBMS_OUTPUT.PUT_LINE('ID: ' || v_id);
    DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Date: ' || v_date);
END;
/
```



### Expected result examples

#### After `CREATE TABLE departments`

Query:

```sql
SELECT table_name
FROM user_tables
WHERE table_name = 'DEPARTMENTS';
```

Expected result:

| table_name |
|---|
| DEPARTMENTS |

This confirms that the table was created.

#### After `ALTER TABLE employees ADD phone_number`

Query:

```sql
DESC employees;
```

Expected additional column:

| Column | Type |
|---|---|
| PHONE_NUMBER | VARCHAR2(20) |

#### After `ALTER TABLE employees RENAME COLUMN phone_number TO phone`

Expected structure change:

| Old column | New column |
|---|---|
| PHONE_NUMBER | PHONE |

#### After `TRUNCATE TABLE company_employees`

Query:

```sql
SELECT COUNT(*) AS total_rows
FROM company_employees;
```

Expected result:

| total_rows |
|---:|
| 0 |

The table still exists, but all rows were removed.

#### After `DROP TABLE company_employees`

Query:

```sql
SELECT *
FROM company_employees;
```

Expected result:

```text
ORA-00942: table or view does not exist
```

The table no longer exists in the schema.


---

## DDL Commands

DDL means **Data Definition Language**. These commands define or modify database structures.

Common DDL commands:

- `CREATE`
- `ALTER`
- `DROP`
- `TRUNCATE`
- `RENAME`
- `COMMENT`

### Create table

```sql
CREATE TABLE departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100) NOT NULL
);
```

### Create table with multiple columns

```sql
CREATE TABLE employees (
    employee_id   NUMBER PRIMARY KEY,
    first_name    VARCHAR2(50) NOT NULL,
    last_name     VARCHAR2(50) NOT NULL,
    email         VARCHAR2(100) UNIQUE,
    salary        NUMBER(10,2),
    hire_date     DATE DEFAULT SYSDATE,
    department_id NUMBER
);
```

### Add a column

```sql
ALTER TABLE employees
ADD phone_number VARCHAR2(20);
```

### Modify a column

```sql
ALTER TABLE employees
MODIFY salary NUMBER(12,2);
```

### Rename a column

```sql
ALTER TABLE employees
RENAME COLUMN phone_number TO phone;
```

### Drop a column

```sql
ALTER TABLE employees
DROP COLUMN phone;
```

### Rename table

```sql
RENAME employees TO company_employees;
```

### Truncate table

```sql
TRUNCATE TABLE company_employees;
```

`TRUNCATE` removes all rows from a table but keeps the table structure.

### Drop table

```sql
DROP TABLE company_employees;
```

`DROP` removes the table structure and its data.



### Expected result examples

#### Primary key violation

If `customer_id = 1` already exists:

```sql
INSERT INTO customers (customer_id, full_name)
VALUES (1, 'Duplicate Customer');
```

Expected result:

```text
ORA-00001: unique constraint violated
```

The primary key prevents duplicate identifiers.

#### Foreign key violation

If `customer_id = 999` does not exist in `customers`:

```sql
INSERT INTO orders (order_id, customer_id)
VALUES (10, 999);
```

Expected result:

```text
ORA-02291: integrity constraint violated - parent key not found
```

The foreign key prevents inserting an order for a customer that does not exist.

#### Check constraint violation

If the salary must be greater than or equal to zero:

```sql
UPDATE employees
SET salary = -500
WHERE employee_id = 101;
```

Expected result:

```text
ORA-02290: check constraint violated
```

The check constraint protects the table from invalid salary values.


---

## Constraints

Constraints are rules applied to table columns to maintain data integrity.

Common constraints:

| Constraint | Purpose |
|---|---|
| `PRIMARY KEY` | Uniquely identifies each row |
| `FOREIGN KEY` | Creates a relationship between tables |
| `NOT NULL` | Prevents null values |
| `UNIQUE` | Prevents duplicate values |
| `CHECK` | Validates a condition |
| `DEFAULT` | Sets a default value |

### Primary key

```sql
CREATE TABLE customers (
    customer_id NUMBER PRIMARY KEY,
    full_name VARCHAR2(100) NOT NULL
);
```

### Foreign key

```sql
CREATE TABLE orders (
    order_id NUMBER PRIMARY KEY,
    customer_id NUMBER,
    order_date DATE DEFAULT SYSDATE,
    CONSTRAINT fk_orders_customers
        FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

### Check constraint

```sql
ALTER TABLE employees
ADD CONSTRAINT chk_salary_positive
CHECK (salary >= 0);
```

### Unique constraint

```sql
ALTER TABLE employees
ADD CONSTRAINT uq_employee_email
UNIQUE (email);
```



### Expected result examples

#### INSERT result

Before insert:

| department_id | department_name |
|---:|---|
| 1 | Technology |
| 2 | Finance |

Command:

```sql
INSERT INTO departments (department_id, department_name)
VALUES (3, 'Human Resources');
```

After insert:

| department_id | department_name |
|---:|---|
| 1 | Technology |
| 2 | Finance |
| 3 | Human Resources |

#### UPDATE result

Before update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |
| 102 | Luis | 1800 |

Command:

```sql
UPDATE employees
SET salary = salary + 200
WHERE employee_id = 101;
```

After update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1700 |
| 102 | Luis | 1800 |

Only Ana is affected because she matches the `WHERE` condition.

#### DELETE result

Before delete:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |
| 102 | Luis | 1800 |
| 103 | Marta | 1300 |

Command:

```sql
DELETE FROM employees
WHERE employee_id = 103;
```

After delete:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |
| 102 | Luis | 1800 |

Marta is removed from the table.

#### MERGE result

Before `MERGE`:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

Command logic:

```sql
MERGE INTO employees e
USING (
    SELECT 101 AS employee_id,
           'Ana' AS first_name,
           'Torres' AS last_name,
           'ana.torres@example.com' AS email,
           1800 AS salary,
           1 AS department_id
    FROM dual
) src
ON (e.employee_id = src.employee_id)
WHEN MATCHED THEN
    UPDATE SET e.salary = src.salary
WHEN NOT MATCHED THEN
    INSERT (employee_id, first_name, last_name, email, salary, department_id)
    VALUES (src.employee_id, src.first_name, src.last_name, src.email, src.salary, src.department_id);
```

After `MERGE`:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1800 |

Because `employee_id = 101` already existed, Oracle updated the salary instead of inserting a new row.


---

## DML Commands

DML means **Data Manipulation Language**. These commands work with the data stored in tables.

Common DML commands:

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`
- `MERGE`

### Insert data

```sql
INSERT INTO departments (department_id, department_name)
VALUES (1, 'Technology');
```

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    email,
    salary,
    department_id
)
VALUES (
    101,
    'Ana',
    'Torres',
    'ana.torres@example.com',
    1500,
    1
);
```

### Update data

```sql
UPDATE employees
SET salary = salary + 200
WHERE employee_id = 101;
```

### Delete data

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

Always use `WHERE` carefully. Without `WHERE`, all rows may be affected.

### Merge data

`MERGE` is used to insert data when it does not exist or update it when it already exists.

```sql
MERGE INTO employees e
USING (
    SELECT 101 AS employee_id,
           'Ana' AS first_name,
           'Torres' AS last_name,
           'ana.torres@example.com' AS email,
           1800 AS salary,
           1 AS department_id
    FROM dual
) src
ON (e.employee_id = src.employee_id)
WHEN MATCHED THEN
    UPDATE SET e.salary = src.salary
WHEN NOT MATCHED THEN
    INSERT (employee_id, first_name, last_name, email, salary, department_id)
    VALUES (src.employee_id, src.first_name, src.last_name, src.email, src.salary, src.department_id);
```



### Expected result examples

Using the sample `employees` table:

#### `SELECT *`

```sql
SELECT *
FROM employees;
```

Expected result:

| employee_id | first_name | last_name | salary | department_id |
|---:|---|---|---:|---:|
| 101 | Ana | Torres | 1500 | 1 |
| 102 | Luis | Perez | 1800 | 1 |
| 103 | Marta | Gomez | 1300 | 2 |
| 104 | Carlos | Diaz | 2200 | 2 |
| 105 | Pedro | Salazar | 3000 | 3 |

#### `WHERE`

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 1800;
```

Expected result:

| first_name | salary |
|---|---:|
| Carlos | 2200 |
| Pedro | 3000 |

Only employees with salary greater than `1800` are returned.

#### `ORDER BY`

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary DESC;
```

Expected result:

| first_name | salary |
|---|---:|
| Pedro | 3000 |
| Carlos | 2200 |
| Luis | 1800 |
| Ana | 1500 |
| Marta | 1300 |

The rows are sorted from highest salary to lowest salary.

#### `DISTINCT`

```sql
SELECT DISTINCT department_id
FROM employees;
```

Expected result:

| department_id |
|---:|
| 1 |
| 2 |
| 3 |

Repeated department values are shown only once.

#### Aggregate functions

```sql
SELECT COUNT(*) AS total_employees,
       AVG(salary) AS average_salary,
       MIN(salary) AS minimum_salary,
       MAX(salary) AS maximum_salary,
       SUM(salary) AS total_salary
FROM employees;
```

Expected result:

| total_employees | average_salary | minimum_salary | maximum_salary | total_salary |
|---:|---:|---:|---:|---:|
| 5 | 1960 | 1300 | 3000 | 9800 |

#### `GROUP BY`

```sql
SELECT department_id,
       COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

Expected result:

| department_id | total_employees |
|---:|---:|
| 1 | 2 |
| 2 | 2 |
| 3 | 1 |

#### `HAVING`

```sql
SELECT department_id,
       COUNT(*) AS total_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 1;
```

Expected result:

| department_id | total_employees |
|---:|---:|
| 1 | 2 |
| 2 | 2 |

Department `3` is not shown because it has only one employee.


---

## SELECT Queries

### Select all columns

```sql
SELECT *
FROM employees;
```

### Select specific columns

```sql
SELECT first_name, last_name, salary
FROM employees;
```

### WHERE condition

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 1000;
```

### ORDER BY

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary DESC;
```

`ASC` means ascending order.  
`DESC` means descending order.

### DISTINCT

```sql
SELECT DISTINCT department_id
FROM employees;
```

### Column aliases

```sql
SELECT first_name AS name,
       salary AS monthly_salary
FROM employees;
```

### Aggregate functions

```sql
SELECT COUNT(*) AS total_employees,
       AVG(salary) AS average_salary,
       MIN(salary) AS minimum_salary,
       MAX(salary) AS maximum_salary,
       SUM(salary) AS total_salary
FROM employees;
```

### GROUP BY

```sql
SELECT department_id,
       COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

### HAVING

```sql
SELECT department_id,
       COUNT(*) AS total_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 2;
```

`WHERE` filters rows before grouping.  
`HAVING` filters groups after grouping.



### Expected result examples

Using the sample tables:

**departments**

| department_id | department_name |
|---:|---|
| 1 | Technology |
| 2 | Finance |
| 3 | Human Resources |
| 4 | Operations |

**employees**

| employee_id | first_name | department_id |
|---:|---|---:|
| 101 | Ana | 1 |
| 102 | Luis | 1 |
| 103 | Marta | 2 |
| 104 | Carlos | 2 |
| 105 | Pedro | 3 |

#### INNER JOIN

```sql
SELECT e.first_name,
       d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

Expected result:

| first_name | department_name |
|---|---|
| Ana | Technology |
| Luis | Technology |
| Marta | Finance |
| Carlos | Finance |
| Pedro | Human Resources |

Only rows with matching `department_id` in both tables are returned.

#### LEFT JOIN

```sql
SELECT e.first_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

Expected result:

| first_name | department_name |
|---|---|
| Ana | Technology |
| Luis | Technology |
| Marta | Finance |
| Carlos | Finance |
| Pedro | Human Resources |

All employees are returned. If an employee had no department, the department name would be `NULL`.

#### RIGHT JOIN

```sql
SELECT e.first_name,
       d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

Expected result:

| first_name | department_name |
|---|---|
| Ana | Technology |
| Luis | Technology |
| Marta | Finance |
| Carlos | Finance |
| Pedro | Human Resources |
| NULL | Operations |

The `Operations` department appears even though it has no employees.

#### FULL OUTER JOIN

```sql
SELECT e.first_name,
       d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.department_id;
```

Expected result:

| first_name | department_name |
|---|---|
| Ana | Technology |
| Luis | Technology |
| Marta | Finance |
| Carlos | Finance |
| Pedro | Human Resources |
| NULL | Operations |

A full outer join returns matched rows plus unmatched rows from both tables.


---

## Joins

Joins are used to combine rows from two or more tables.

### INNER JOIN

Returns only matching records from both tables.

```sql
SELECT e.first_name,
       e.last_name,
       d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

### LEFT JOIN

Returns all records from the left table and matching records from the right table.

```sql
SELECT e.first_name,
       e.last_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

### RIGHT JOIN

Returns all records from the right table and matching records from the left table.

```sql
SELECT e.first_name,
       e.last_name,
       d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

### FULL OUTER JOIN

Returns records when there is a match in either table.

```sql
SELECT e.first_name,
       d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.department_id;
```



### Expected result examples

#### Subquery in `WHERE`

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Inner query result:

| average_salary |
|---:|
| 1960 |

Final expected result:

| first_name | salary |
|---|---:|
| Carlos | 2200 |
| Pedro | 3000 |

The inner query calculates the average salary. The outer query returns employees whose salary is greater than that average.

#### Subquery in `FROM`

```sql
SELECT department_id, average_salary
FROM (
    SELECT department_id,
           AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
);
```

Inner query temporary result:

| department_id | average_salary |
|---:|---:|
| 1 | 1650 |
| 2 | 1750 |
| 3 | 3000 |

Final expected result:

| department_id | average_salary |
|---:|---:|
| 1 | 1650 |
| 2 | 1750 |
| 3 | 3000 |

The subquery in the `FROM` clause behaves like a temporary table.

#### Subquery in `FROM` with `JOIN`

```sql
SELECT d.department_name,
       avg_data.average_salary
FROM departments d
JOIN (
    SELECT department_id,
           AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
) avg_data
ON d.department_id = avg_data.department_id;
```

Expected result:

| department_name | average_salary |
|---|---:|
| Technology | 1650 |
| Finance | 1750 |
| Human Resources | 3000 |

This example shows how a calculated temporary result can be joined with another table.

#### `EXISTS`

```sql
SELECT department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

Expected result:

| department_name |
|---|
| Technology |
| Finance |
| Human Resources |

`Operations` is not returned because it has no employees.


---

## Subqueries

A subquery is a query inside another query.

### Subquery in WHERE

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Subquery in FROM

```sql
SELECT department_id, average_salary
FROM (
    SELECT department_id,
           AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
);
```

### EXISTS

```sql
SELECT department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```



### Expected result examples

```sql
SELECT 10 + 5 AS result
FROM dual;
```

Expected result:

| result |
|---:|
| 15 |

```sql
SELECT UPPER('oracle database') AS text_result
FROM dual;
```

Expected result:

| text_result |
|---|
| ORACLE DATABASE |

`DUAL` is useful when you need Oracle to calculate or return something that does not come from a real user table.


---

## The DUAL Table

`DUAL` is a special one-row table in Oracle commonly used to evaluate expressions, constants, or functions when no real table is needed.

### Example

```sql
SELECT SYSDATE
FROM dual;
```

### More examples

```sql
SELECT 10 + 5 AS result
FROM dual;

SELECT USER
FROM dual;

SELECT UPPER('oracle database') AS text_result
FROM dual;
```



### Expected result examples

#### Text functions

```sql
SELECT UPPER('oracle') AS upper_text,
       LOWER('ORACLE') AS lower_text,
       INITCAP('oracle database') AS initcap_text,
       LENGTH('Oracle') AS text_length,
       SUBSTR('Oracle Database', 1, 6) AS substring_text
FROM dual;
```

Expected result:

| upper_text | lower_text | initcap_text | text_length | substring_text |
|---|---|---|---:|---|
| ORACLE | oracle | Oracle Database | 6 | Oracle |

#### Numeric functions

```sql
SELECT ROUND(15.678, 2) AS rounded_value,
       TRUNC(15.678, 2) AS truncated_value,
       MOD(10, 3) AS remainder
FROM dual;
```

Expected result:

| rounded_value | truncated_value | remainder |
|---:|---:|---:|
| 15.68 | 15.67 | 1 |

#### NULL functions

```sql
SELECT NVL(NULL, 'Default value') AS nvl_result,
       COALESCE(NULL, NULL, 'Final value') AS coalesce_result
FROM dual;
```

Expected result:

| nvl_result | coalesce_result |
|---|---|
| Default value | Final value |

#### CASE expression

```sql
SELECT first_name,
       salary,
       CASE
           WHEN salary >= 3000 THEN 'High'
           WHEN salary >= 1500 THEN 'Medium'
           ELSE 'Low'
       END AS salary_level
FROM employees;
```

Expected result:

| first_name | salary | salary_level |
|---|---:|---|
| Ana | 1500 | Medium |
| Luis | 1800 | Medium |
| Marta | 1300 | Low |
| Carlos | 2200 | Medium |
| Pedro | 3000 | High |


---

## Oracle Functions

### Text functions

```sql
SELECT UPPER('oracle') AS upper_text FROM dual;
SELECT LOWER('ORACLE') AS lower_text FROM dual;
SELECT INITCAP('oracle database') AS initcap_text FROM dual;
SELECT LENGTH('Oracle') AS text_length FROM dual;
SELECT SUBSTR('Oracle Database', 1, 6) AS substring_text FROM dual;
```

| Function | Purpose |
|---|---|
| `UPPER` | Converts text to uppercase |
| `LOWER` | Converts text to lowercase |
| `INITCAP` | Capitalizes the first letter of each word |
| `LENGTH` | Returns text length |
| `SUBSTR` | Extracts part of a string |

### Numeric functions

```sql
SELECT ROUND(15.678, 2) FROM dual;
SELECT TRUNC(15.678, 2) FROM dual;
SELECT MOD(10, 3) FROM dual;
```

### Date functions

```sql
SELECT SYSDATE FROM dual;
SELECT ADD_MONTHS(SYSDATE, 3) FROM dual;
SELECT MONTHS_BETWEEN(SYSDATE, DATE '2024-01-01') FROM dual;
```

| Function | Purpose |
|---|---|
| `SYSDATE` | Returns current date and time |
| `ADD_MONTHS` | Adds months to a date |
| `MONTHS_BETWEEN` | Calculates months between two dates |

### NULL functions

```sql
SELECT NVL(NULL, 'Default value') FROM dual;
SELECT COALESCE(NULL, NULL, 'Final value') FROM dual;
```

| Function | Purpose |
|---|---|
| `NVL(value, replacement)` | Replaces `NULL` with a value |
| `COALESCE(value1, value2, ...)` | Returns the first non-null value |

### CASE expression

```sql
SELECT first_name,
       salary,
       CASE
           WHEN salary >= 3000 THEN 'High'
           WHEN salary >= 1500 THEN 'Medium'
           ELSE 'Low'
       END AS salary_level
FROM employees;
```



### Expected result examples

#### `COMMIT`

Before insert:

| department_id | department_name |
|---:|---|
| 1 | Technology |

Command:

```sql
INSERT INTO departments (department_id, department_name)
VALUES (2, 'Finance');

COMMIT;
```

After `COMMIT`:

| department_id | department_name |
|---:|---|
| 1 | Technology |
| 2 | Finance |

The new row is permanently saved.

#### `ROLLBACK`

Before update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

Command:

```sql
UPDATE employees
SET salary = 9999
WHERE employee_id = 101;

ROLLBACK;
```

After `ROLLBACK`:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

The update is undone because it was not committed.

#### `SAVEPOINT`

Initial state:

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 101 | Ana | 1 | 1500 |
| 103 | Marta | 2 | 1300 |

After first update:

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 101 | Ana | 1 | 1600 |
| 103 | Marta | 2 | 1300 |

After second update before rollback:

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 101 | Ana | 1 | 1600 |
| 103 | Marta | 2 | 1800 |

After `ROLLBACK TO salary_update_1`:

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 101 | Ana | 1 | 1600 |
| 103 | Marta | 2 | 1300 |

Only the second update is undone.


---

## Transactions

Transactions allow multiple database operations to be treated as one logical unit.

### COMMIT

```sql
INSERT INTO departments (department_id, department_name)
VALUES (2, 'Finance');

COMMIT;
```

`COMMIT` permanently saves the changes.

### ROLLBACK

```sql
UPDATE employees
SET salary = 9999;

ROLLBACK;
```

`ROLLBACK` cancels uncommitted changes.

### SAVEPOINT

```sql
UPDATE employees
SET salary = salary + 100
WHERE department_id = 1;

SAVEPOINT salary_update_1;

UPDATE employees
SET salary = salary + 500
WHERE department_id = 2;

ROLLBACK TO salary_update_1;

COMMIT;
```

A `SAVEPOINT` allows returning to a specific point inside a transaction.



### Expected output examples

#### Basic block

```sql
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello from PL/SQL');
END;
/
```

Expected output:

```text
Hello from PL/SQL
```

#### Complete block

```sql
DECLARE
    v_name VARCHAR2(50);
    v_age NUMBER;
BEGIN
    v_name := 'Pedro';
    v_age := 25;

    DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Age: ' || v_age);
END;
/
```

Expected output:

```text
Name: Pedro
Age: 25
```

The result appears in the DBMS Output panel when `SET SERVEROUTPUT ON` is enabled.


---

## PL/SQL Block Structure

A PL/SQL block has three main sections:

```sql
DECLARE
    -- Declaration section
BEGIN
    -- Executable section
EXCEPTION
    -- Exception handling section
END;
/
```

Only the `BEGIN ... END;` section is mandatory.

### Basic block

```sql
SET SERVEROUTPUT ON;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello from PL/SQL');
END;
/
```

### Complete block

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_name VARCHAR2(50);
    v_age NUMBER;
BEGIN
    v_name := 'Pedro';
    v_age := 25;

    DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Age: ' || v_age);
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

Explanation:

- `DECLARE`: optional section for variables, constants, cursors, and types.
- `BEGIN`: starts the executable section.
- `DBMS_OUTPUT.PUT_LINE`: prints messages.
- `EXCEPTION`: handles errors.
- `END;`: closes the block.
- `/`: executes the PL/SQL block in SQL Developer or SQL*Plus.



### Expected output examples

#### Basic variable

```sql
DECLARE
    v_salary NUMBER := 1000;
BEGIN
    v_salary := v_salary + 200;
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

Expected output:

```text
Salary: 1200
```

#### Constant

```sql
DECLARE
    c_tax CONSTANT NUMBER := 0.07;
    v_price NUMBER := 100;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Total: ' || (v_price + v_price * c_tax));
END;
/
```

Expected output:

```text
Total: 107
```

#### `%TYPE`

If employee `101` is Ana:

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_first_name
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_first_name);
END;
/
```

Expected output:

```text
Ana
```

#### `%ROWTYPE`

```sql
DECLARE
    v_employee employees%ROWTYPE;
BEGIN
    SELECT *
    INTO v_employee
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_employee.first_name || ' ' || v_employee.last_name);
END;
/
```

Expected output:

```text
Ana Torres
```


---

## Variables in PL/SQL

### Basic variables

```sql
DECLARE
    v_salary NUMBER := 1000;
BEGIN
    v_salary := v_salary + 200;
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

### Constants

```sql
DECLARE
    c_tax CONSTANT NUMBER := 0.07;
    v_price NUMBER := 100;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Total: ' || (v_price + v_price * c_tax));
END;
/
```

`CONSTANT` prevents a value from being modified after declaration.

### `%TYPE`

`%TYPE` copies the data type of a table column.

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_first_name
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_first_name);
END;
/
```

### `%ROWTYPE`

`%ROWTYPE` copies the structure of a table row.

```sql
DECLARE
    v_employee employees%ROWTYPE;
BEGIN
    SELECT *
    INTO v_employee
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE(v_employee.first_name || ' ' || v_employee.last_name);
END;
/
```



### Expected output examples

#### `IF`

```sql
DECLARE
    v_salary NUMBER := 1800;
BEGIN
    IF v_salary >= 3000 THEN
        DBMS_OUTPUT.PUT_LINE('High salary');
    ELSIF v_salary >= 1500 THEN
        DBMS_OUTPUT.PUT_LINE('Medium salary');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Low salary');
    END IF;
END;
/
```

Expected output:

```text
Medium salary
```

The value `1800` satisfies the second condition.

#### `CASE`

```sql
DECLARE
    v_status CHAR(1) := 'A';
BEGIN
    CASE v_status
        WHEN 'A' THEN
            DBMS_OUTPUT.PUT_LINE('Active');
        WHEN 'I' THEN
            DBMS_OUTPUT.PUT_LINE('Inactive');
        ELSE
            DBMS_OUTPUT.PUT_LINE('Unknown status');
    END CASE;
END;
/
```

Expected output:

```text
Active
```


---

## Conditional Statements

### IF statement

```sql
DECLARE
    v_salary NUMBER := 1800;
BEGIN
    IF v_salary >= 3000 THEN
        DBMS_OUTPUT.PUT_LINE('High salary');
    ELSIF v_salary >= 1500 THEN
        DBMS_OUTPUT.PUT_LINE('Medium salary');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Low salary');
    END IF;
END;
/
```

### CASE statement

```sql
DECLARE
    v_status CHAR(1) := 'A';
BEGIN
    CASE v_status
        WHEN 'A' THEN
            DBMS_OUTPUT.PUT_LINE('Active');
        WHEN 'I' THEN
            DBMS_OUTPUT.PUT_LINE('Inactive');
        ELSE
            DBMS_OUTPUT.PUT_LINE('Unknown status');
    END CASE;
END;
/
```



### Expected output examples

#### Basic `LOOP`

Expected output:

```text
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
```

#### `WHILE LOOP`

Expected output:

```text
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
```

#### `FOR LOOP`

Expected output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

#### Reverse `FOR LOOP`

Expected output:

```text
Number: 5
Number: 4
Number: 3
Number: 2
Number: 1
```


---

## Loops

### Basic LOOP

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN
    LOOP
        DBMS_OUTPUT.PUT_LINE('Counter: ' || v_counter);
        v_counter := v_counter + 1;

        EXIT WHEN v_counter > 5;
    END LOOP;
END;
/
```

### WHILE LOOP

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN
    WHILE v_counter <= 5 LOOP
        DBMS_OUTPUT.PUT_LINE('Counter: ' || v_counter);
        v_counter := v_counter + 1;
    END LOOP;
END;
/
```

### FOR LOOP

```sql
BEGIN
    FOR i IN 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE('Number: ' || i);
    END LOOP;
END;
/
```

### Reverse FOR LOOP

```sql
BEGIN
    FOR i IN REVERSE 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE('Number: ' || i);
    END LOOP;
END;
/
```



### Expected output example

For employee `101`:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

Block:

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
    v_salary employees.salary%TYPE;
BEGIN
    SELECT first_name, salary
    INTO v_first_name, v_salary
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_first_name);
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

Expected output:

```text
Employee: Ana
Salary: 1500
```

If the query returns no rows, the expected result is:

```text
ORA-01403: no data found
```

If the query returns more than one row, the expected result is:

```text
ORA-01422: exact fetch returns more than requested number of rows
```


---

## SELECT INTO

In PL/SQL, `SELECT INTO` is used to store query results into variables.

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
    v_salary employees.salary%TYPE;
BEGIN
    SELECT first_name, salary
    INTO v_first_name, v_salary
    FROM employees
    WHERE employee_id = 101;

    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_first_name);
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

Important exceptions:

- `NO_DATA_FOUND`: no row was returned.
- `TOO_MANY_ROWS`: more than one row was returned.



### Expected output examples

#### `NO_DATA_FOUND`

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_first_name
    FROM employees
    WHERE employee_id = 999;

    DBMS_OUTPUT.PUT_LINE(v_first_name);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');
END;
/
```

Expected output:

```text
Employee not found
```

The query does not find employee `999`, so the exception block is executed.

#### Custom error

```sql
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'Salary cannot be negative');
END;
/
```

Expected result:

```text
ORA-20001: Salary cannot be negative
```


---

## Exceptions

### Basic exception handling

```sql
DECLARE
    v_first_name employees.first_name%TYPE;
BEGIN
    SELECT first_name
    INTO v_first_name
    FROM employees
    WHERE employee_id = 999;

    DBMS_OUTPUT.PUT_LINE(v_first_name);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('The query returned too many rows');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

### Raise custom error

```sql
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'Salary cannot be negative');
END;
/
```

Custom application errors usually use codes between `-20000` and `-20999`.



### Expected output examples

#### Implicit cursor

If two employees belong to department `1`:

```sql
BEGIN
    UPDATE employees
    SET salary = salary + 100
    WHERE department_id = 1;

    DBMS_OUTPUT.PUT_LINE('Rows updated: ' || SQL%ROWCOUNT);
END;
/
```

Expected output:

```text
Rows updated: 2
```

#### Explicit cursor

Expected output using the sample table:

```text
Ana earns 1500
Luis earns 1800
Marta earns 1300
Carlos earns 2200
Pedro earns 3000
```

#### Cursor `FOR LOOP`

Expected output:

```text
Ana - 1500
Luis - 1800
Marta - 1300
Carlos - 2200
Pedro - 3000
```

The cursor processes one row at a time.


---

## Cursors

Cursors are used to process query results row by row.

### Implicit cursor

```sql
BEGIN
    UPDATE employees
    SET salary = salary + 100
    WHERE department_id = 1;

    DBMS_OUTPUT.PUT_LINE('Rows updated: ' || SQL%ROWCOUNT);
END;
/
```

Useful implicit cursor attributes:

| Attribute | Meaning |
|---|---|
| `SQL%ROWCOUNT` | Number of affected rows |
| `SQL%FOUND` | Returns true if a row was affected |
| `SQL%NOTFOUND` | Returns true if no row was affected |
| `SQL%ISOPEN` | Usually false for implicit cursors |

### Explicit cursor

```sql
DECLARE
    CURSOR c_employees IS
        SELECT first_name, salary
        FROM employees;

    v_first_name employees.first_name%TYPE;
    v_salary employees.salary%TYPE;
BEGIN
    OPEN c_employees;

    LOOP
        FETCH c_employees INTO v_first_name, v_salary;
        EXIT WHEN c_employees%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(v_first_name || ' earns ' || v_salary);
    END LOOP;

    CLOSE c_employees;
END;
/
```

### Cursor FOR LOOP

```sql
BEGIN
    FOR emp IN (
        SELECT first_name, salary
        FROM employees
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(emp.first_name || ' - ' || emp.salary);
    END LOOP;
END;
/
```

Cursor `FOR LOOP` automatically opens, fetches, and closes the cursor.



### Expected result examples

#### `increase_salary`

Before procedure execution:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

Execution:

```sql
BEGIN
    increase_salary(101, 300);
END;
/
```

After procedure execution:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1800 |

The procedure updates Ana's salary by adding `300`.

#### Procedure with `OUT` parameter

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    get_salary(101, v_salary);
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

Expected output:

```text
Salary: 1800
```

The value is returned through the `OUT` parameter.


---

## Procedures

Procedures are stored programs that execute actions in the database.

### Create procedure

```sql
CREATE OR REPLACE PROCEDURE increase_salary (
    p_employee_id IN NUMBER,
    p_amount      IN NUMBER
)
IS
BEGIN
    UPDATE employees
    SET salary = salary + p_amount
    WHERE employee_id = p_employee_id;

    IF SQL%ROWCOUNT = 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Employee not found');
    END IF;
END;
/
```

### Execute procedure

```sql
BEGIN
    increase_salary(101, 300);
END;
/
```

Or:

```sql
EXEC increase_salary(101, 300);
```

### Procedure with OUT parameter

```sql
CREATE OR REPLACE PROCEDURE get_salary (
    p_employee_id IN NUMBER,
    p_salary OUT NUMBER
)
IS
BEGIN
    SELECT salary
    INTO p_salary
    FROM employees
    WHERE employee_id = p_employee_id;
END;
/
```

Execute:

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    get_salary(101, v_salary);
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/
```

### Parameter modes

| Mode | Purpose |
|---|---|
| `IN` | Sends a value into the procedure |
| `OUT` | Returns a value from the procedure |
| `IN OUT` | Sends and returns a value |



### Expected result examples

#### Function in PL/SQL

```sql
DECLARE
    v_bonus NUMBER;
BEGIN
    v_bonus := calculate_bonus(1500);
    DBMS_OUTPUT.PUT_LINE('Bonus: ' || v_bonus);
END;
/
```

Expected output:

```text
Bonus: 150
```

#### Function in SQL

```sql
SELECT first_name,
       salary,
       calculate_bonus(salary) AS bonus
FROM employees;
```

Expected result:

| first_name | salary | bonus |
|---|---:|---:|
| Ana | 1500 | 150 |
| Luis | 1800 | 180 |
| Marta | 1300 | 130 |
| Carlos | 2200 | 220 |
| Pedro | 3000 | 300 |

The function calculates 10% of each salary.


---

## Functions

Functions return a value.

### Create function

```sql
CREATE OR REPLACE FUNCTION calculate_bonus (
    p_salary IN NUMBER
)
RETURN NUMBER
IS
    v_bonus NUMBER;
BEGIN
    v_bonus := p_salary * 0.10;
    RETURN v_bonus;
END;
/
```

### Use function in PL/SQL

```sql
DECLARE
    v_bonus NUMBER;
BEGIN
    v_bonus := calculate_bonus(1500);
    DBMS_OUTPUT.PUT_LINE('Bonus: ' || v_bonus);
END;
/
```

### Use function in SQL

```sql
SELECT first_name,
       salary,
       calculate_bonus(salary) AS bonus
FROM employees;
```

### Procedure vs Function

| Feature | Procedure | Function |
|---|---|---|
| Main purpose | Execute an action | Return a value |
| Return value | Optional through `OUT` parameters | Required with `RETURN` |
| Use in SQL query | Usually no | Yes, if valid for SQL usage |
| Common use | Business operations | Calculations or reusable expressions |



### Expected result examples

After creating `employee_pkg`, execute:

```sql
BEGIN
    employee_pkg.increase_salary(101, 200);
    DBMS_OUTPUT.PUT_LINE(employee_pkg.calculate_bonus(1500));
END;
/
```

Expected output:

```text
150
```

Expected salary effect:

| employee_id | first_name | old_salary | new_salary |
|---:|---|---:|---:|
| 101 | Ana | 1500 | 1700 |

The procedure inside the package modifies the salary, while the function returns the bonus value.


---

## Packages

Packages group related procedures, functions, variables, and types.

A package usually has two parts:

- **Package specification**: public interface.
- **Package body**: implementation.

### Package specification

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS
    PROCEDURE increase_salary(
        p_employee_id IN NUMBER,
        p_amount IN NUMBER
    );

    FUNCTION calculate_bonus(
        p_salary IN NUMBER
    ) RETURN NUMBER;
END employee_pkg;
/
```

### Package body

```sql
CREATE OR REPLACE PACKAGE BODY employee_pkg AS

    PROCEDURE increase_salary(
        p_employee_id IN NUMBER,
        p_amount IN NUMBER
    )
    IS
    BEGIN
        UPDATE employees
        SET salary = salary + p_amount
        WHERE employee_id = p_employee_id;
    END increase_salary;

    FUNCTION calculate_bonus(
        p_salary IN NUMBER
    )
    RETURN NUMBER
    IS
    BEGIN
        RETURN p_salary * 0.10;
    END calculate_bonus;

END employee_pkg;
/
```

### Use package

```sql
BEGIN
    employee_pkg.increase_salary(101, 200);
    DBMS_OUTPUT.PUT_LINE(employee_pkg.calculate_bonus(1500));
END;
/
```



### Expected result examples

#### Validation trigger

If the trigger `trg_validate_salary` exists:

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    email,
    salary,
    department_id
)
VALUES (
    200,
    'Invalid',
    'User',
    'invalid.user@example.com',
    -100,
    1
);
```

Expected result:

```text
ORA-20002: Salary cannot be negative
```

The trigger prevents invalid salary values from being inserted or updated.

#### Audit trigger

Before update:

| employee_id | salary |
|---:|---:|
| 101 | 1500 |

Command:

```sql
UPDATE employees
SET salary = 1800
WHERE employee_id = 101;
```

Expected row in `employee_salary_audit`:

| employee_id | old_salary | new_salary |
|---:|---:|---:|
| 101 | 1500 | 1800 |

The trigger automatically stores the salary change.


---

## Triggers

Triggers execute automatically when a database event occurs.

Common trigger events:

- `INSERT`
- `UPDATE`
- `DELETE`

### Before insert or update trigger

```sql
CREATE OR REPLACE TRIGGER trg_validate_salary
BEFORE INSERT OR UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Salary cannot be negative');
    END IF;
END;
/
```

### Audit trigger

```sql
CREATE TABLE employee_salary_audit (
    audit_id NUMBER GENERATED BY DEFAULT AS IDENTITY,
    employee_id NUMBER,
    old_salary NUMBER,
    new_salary NUMBER,
    change_date DATE DEFAULT SYSDATE
);
```

```sql
CREATE OR REPLACE TRIGGER trg_audit_salary
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_salary_audit (
        employee_id,
        old_salary,
        new_salary
    )
    VALUES (
        :OLD.employee_id,
        :OLD.salary,
        :NEW.salary
    );
END;
/
```

`OLD` stores the previous value and `NEW` stores the new value.



### Expected result examples

#### Sequence

If the current sequence value starts at `1`:

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    email,
    salary,
    department_id
)
VALUES (
    employee_seq.NEXTVAL,
    'Laura',
    'Gomez',
    'laura.gomez@example.com',
    1300,
    1
);
```

Expected inserted row:

| employee_id | first_name | last_name | salary |
|---:|---|---|---:|
| 1 | Laura | Gomez | 1300 |

The ID is generated automatically by the sequence.

#### Identity column

```sql
INSERT INTO clients (full_name)
VALUES ('Maria Lopez');
```

Expected result:

| client_id | full_name |
|---:|---|
| 1 | Maria Lopez |

Oracle automatically generates the identity value.


---

## Sequences and Identity Columns

### Create sequence

```sql
CREATE SEQUENCE employee_seq
START WITH 1
INCREMENT BY 1
NOCACHE;
```

### Use sequence

```sql
INSERT INTO employees (
    employee_id,
    first_name,
    last_name,
    email,
    salary,
    department_id
)
VALUES (
    employee_seq.NEXTVAL,
    'Laura',
    'Gomez',
    'laura.gomez@example.com',
    1300,
    1
);
```

### NEXTVAL and CURRVAL

| Pseudocolumn | Purpose |
|---|---|
| `NEXTVAL` | Gets the next sequence value |
| `CURRVAL` | Gets the current sequence value after `NEXTVAL` has been used |

### Identity column

```sql
CREATE TABLE clients (
    client_id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    full_name VARCHAR2(100)
);
```

Insert without providing the ID:

```sql
INSERT INTO clients (full_name)
VALUES ('Maria Lopez');
```



### Expected result example

After creating `vw_employee_departments`:

```sql
SELECT *
FROM vw_employee_departments;
```

Expected result:

| employee_id | first_name | last_name | salary | department_name |
|---:|---|---|---:|---|
| 101 | Ana | Torres | 1500 | Technology |
| 102 | Luis | Perez | 1800 | Technology |
| 103 | Marta | Gomez | 1300 | Finance |
| 104 | Carlos | Diaz | 2200 | Finance |
| 105 | Pedro | Salazar | 3000 | Human Resources |

The view hides the join logic and lets the user query the result as if it were a table.


---

## Views

A view is a stored query that behaves like a virtual table.

```sql
CREATE OR REPLACE VIEW vw_employee_departments AS
SELECT e.employee_id,
       e.first_name,
       e.last_name,
       e.salary,
       d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

Query the view:

```sql
SELECT *
FROM vw_employee_departments;
```

Common uses:

- Simplify complex queries.
- Hide sensitive columns.
- Provide controlled access to data.
- Improve query readability.



### Expected result example

After creating this index:

```sql
CREATE INDEX idx_employees_last_name
ON employees(last_name);
```

Query:

```sql
SELECT index_name,
       table_name
FROM user_indexes
WHERE index_name = 'IDX_EMPLOYEES_LAST_NAME';
```

Expected result:

| index_name | table_name |
|---|---|
| IDX_EMPLOYEES_LAST_NAME | EMPLOYEES |

The visible data in the table does not change. The index affects how Oracle can search the data internally.


---

## Indexes

Indexes improve query performance but may slow down inserts, updates, and deletes if overused.

### Create index

```sql
CREATE INDEX idx_employees_last_name
ON employees(last_name);
```

### Create unique index

```sql
CREATE UNIQUE INDEX idx_employees_email
ON employees(email);
```

Use indexes when:

- A column is frequently used in `WHERE`.
- A column is frequently used in joins.
- A column is frequently used for sorting.
- The table has many rows.

Avoid unnecessary indexes when:

- The table is small.
- The column changes frequently.
- The column has very low selectivity.



### Expected result example

After creating the synonym:

```sql
CREATE SYNONYM emp FOR employees;
```

Both queries return the same result:

```sql
SELECT first_name, salary
FROM employees;
```

```sql
SELECT first_name, salary
FROM emp;
```

Expected result:

| first_name | salary |
|---|---:|
| Ana | 1500 |
| Luis | 1800 |
| Marta | 1300 |
| Carlos | 2200 |
| Pedro | 3000 |

The synonym works as an alias for the original table.


---

## Synonyms

A synonym is an alias for a database object.

```sql
CREATE SYNONYM emp FOR employees;
```

Usage:

```sql
SELECT *
FROM emp;
```



### Expected result examples

#### Create user

```sql
CREATE USER app_user
IDENTIFIED BY StrongPassword123;
```

Expected result:

```text
User created.
```

#### Grant privilege

```sql
GRANT CREATE SESSION TO app_user;
```

Expected result:

```text
Grant succeeded.
```

#### Revoke privilege

```sql
REVOKE INSERT ON employees FROM app_user;
```

Expected result:

```text
Revoke succeeded.
```

If `app_user` tries to insert after the privilege is revoked:

```sql
INSERT INTO employees (employee_id, first_name, last_name)
VALUES (999, 'Test', 'User');
```

Expected result:

```text
ORA-01031: insufficient privileges
```


---

## Users, Roles, and Privileges

### Create user

```sql
CREATE USER app_user
IDENTIFIED BY StrongPassword123;
```

### Grant connection privilege

```sql
GRANT CREATE SESSION TO app_user;
```

### Grant object privileges

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON employees TO app_user;
```

### Create role

```sql
CREATE ROLE app_readonly_role;
```

### Grant privileges to a role

```sql
GRANT SELECT ON employees TO app_readonly_role;
```

### Grant role to a user

```sql
GRANT app_readonly_role TO app_user;
```

### Revoke privilege

```sql
REVOKE INSERT ON employees FROM app_user;
```



### Expected result examples

#### Dynamic table creation

```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'CREATE TABLE dynamic_test (id NUMBER, name VARCHAR2(50))';
    EXECUTE IMMEDIATE v_sql;
END;
/
```

Check result:

```sql
SELECT table_name
FROM user_tables
WHERE table_name = 'DYNAMIC_TEST';
```

Expected result:

| table_name |
|---|
| DYNAMIC_TEST |

#### Dynamic update with parameters

Before dynamic update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |

Block:

```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'UPDATE employees SET salary = salary + :1 WHERE employee_id = :2';
    EXECUTE IMMEDIATE v_sql USING 200, 101;
END;
/
```

After dynamic update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1700 |


---

## Dynamic SQL

Dynamic SQL allows SQL statements to be built and executed at runtime.

### Execute immediate

```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'CREATE TABLE dynamic_test (id NUMBER, name VARCHAR2(50))';
    EXECUTE IMMEDIATE v_sql;
END;
/
```

### Dynamic SQL with parameters

```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'UPDATE employees SET salary = salary + :1 WHERE employee_id = :2';
    EXECUTE IMMEDIATE v_sql USING 200, 101;
END;
/
```

Using parameters is safer than concatenating values directly.



### Expected output examples

#### VARRAY

```sql
DECLARE
    TYPE name_array IS VARRAY(3) OF VARCHAR2(50);
    v_names name_array := name_array('Ana', 'Luis', 'Pedro');
BEGIN
    FOR i IN 1..v_names.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_names(i));
    END LOOP;
END;
/
```

Expected output:

```text
Ana
Luis
Pedro
```

#### Associative array

```sql
DECLARE
    TYPE salary_table IS TABLE OF NUMBER INDEX BY VARCHAR2(50);
    v_salaries salary_table;
BEGIN
    v_salaries('Ana') := 1200;
    v_salaries('Luis') := 1500;

    DBMS_OUTPUT.PUT_LINE('Ana salary: ' || v_salaries('Ana'));
END;
/
```

Expected output:

```text
Ana salary: 1200
```


---

## Collections

Collections allow storing multiple values in one PL/SQL variable.

### VARRAY

```sql
DECLARE
    TYPE name_array IS VARRAY(3) OF VARCHAR2(50);
    v_names name_array := name_array('Ana', 'Luis', 'Pedro');
BEGIN
    FOR i IN 1..v_names.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_names(i));
    END LOOP;
END;
/
```

### Associative array

```sql
DECLARE
    TYPE salary_table IS TABLE OF NUMBER INDEX BY VARCHAR2(50);
    v_salaries salary_table;
BEGIN
    v_salaries('Ana') := 1200;
    v_salaries('Luis') := 1500;

    DBMS_OUTPUT.PUT_LINE('Ana salary: ' || v_salaries('Ana'));
END;
/
```



### Expected result examples

#### `BULK COLLECT`

Using the sample table:

```sql
SELECT first_name
BULK COLLECT INTO v_names
FROM employees;
```

Expected collection content:

| Position | Value |
|---:|---|
| 1 | Ana |
| 2 | Luis |
| 3 | Marta |
| 4 | Carlos |
| 5 | Pedro |

Expected output from the loop:

```text
Ana
Luis
Marta
Carlos
Pedro
```

#### `FORALL`

Before `FORALL`:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1500 |
| 102 | Luis | 1800 |
| 103 | Marta | 1300 |

After `FORALL` update:

| employee_id | first_name | salary |
|---:|---|---:|
| 101 | Ana | 1600 |
| 102 | Luis | 1900 |
| 103 | Marta | 1400 |

`FORALL` updates all IDs in the collection efficiently.


---

## Bulk Processing

Bulk processing improves performance when working with many rows.

### BULK COLLECT

```sql
DECLARE
    TYPE name_table IS TABLE OF employees.first_name%TYPE;
    v_names name_table;
BEGIN
    SELECT first_name
    BULK COLLECT INTO v_names
    FROM employees;

    FOR i IN 1..v_names.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_names(i));
    END LOOP;
END;
/
```

### FORALL

```sql
DECLARE
    TYPE id_table IS TABLE OF employees.employee_id%TYPE;
    v_ids id_table := id_table(101, 102, 103);
BEGIN
    FORALL i IN 1..v_ids.COUNT
        UPDATE employees
        SET salary = salary + 100
        WHERE employee_id = v_ids(i);
END;
/
```



### Expected result examples

#### Show user tables

```sql
SELECT table_name
FROM user_tables;
```

Expected result example:

| table_name |
|---|
| DEPARTMENTS |
| EMPLOYEES |
| EMPLOYEE_SALARY_AUDIT |

#### Show table columns

```sql
SELECT column_name,
       data_type,
       data_length
FROM user_tab_columns
WHERE table_name = 'EMPLOYEES';
```

Expected result example:

| column_name | data_type | data_length |
|---|---|---:|
| EMPLOYEE_ID | NUMBER | 22 |
| FIRST_NAME | VARCHAR2 | 50 |
| LAST_NAME | VARCHAR2 | 50 |
| EMAIL | VARCHAR2 | 100 |
| SALARY | NUMBER | 22 |
| HIRE_DATE | DATE | 7 |
| DEPARTMENT_ID | NUMBER | 22 |

#### Show objects

```sql
SELECT object_name,
       object_type,
       status
FROM user_objects;
```

Expected result example:

| object_name | object_type | status |
|---|---|---|
| EMPLOYEES | TABLE | VALID |
| EMPLOYEE_PKG | PACKAGE | VALID |
| PR_INCREASE_SALARY | PROCEDURE | VALID |
| FN_BONUS | FUNCTION | VALID |


---

## Data Dictionary Queries

Oracle provides dictionary views to inspect database objects.

### Show user tables

```sql
SELECT table_name
FROM user_tables;
```

### Show table columns

```sql
SELECT column_name,
       data_type,
       data_length
FROM user_tab_columns
WHERE table_name = 'EMPLOYEES';
```

### Show user objects

```sql
SELECT object_name,
       object_type,
       status
FROM user_objects;
```

### Show source code

```sql
SELECT line, text
FROM user_source
WHERE name = 'EMPLOYEE_PKG'
ORDER BY line;
```

### Show constraints

```sql
SELECT constraint_name,
       constraint_type,
       table_name,
       status
FROM user_constraints
WHERE table_name = 'EMPLOYEES';
```

### Show indexes

```sql
SELECT index_name,
       table_name,
       uniqueness
FROM user_indexes
WHERE table_name = 'EMPLOYEES';
```



### Expected result examples

#### `SHOW USER`

```sql
SHOW USER;
```

Expected output example:

```text
USER is "HR"
```

#### `DESC employees`

Expected output example:

| Name | Null? | Type |
|---|---|---|
| EMPLOYEE_ID | NOT NULL | NUMBER |
| FIRST_NAME | NOT NULL | VARCHAR2(50) |
| LAST_NAME | NOT NULL | VARCHAR2(50) |
| EMAIL |  | VARCHAR2(100) |
| SALARY |  | NUMBER(10,2) |
| HIRE_DATE |  | DATE |
| DEPARTMENT_ID |  | NUMBER |

#### `CLEAR SCREEN`

Expected result:

```text
The SQL*Plus screen is cleared.
```


---

## SQL Developer and SQL*Plus Useful Commands

### Enable DBMS output

```sql
SET SERVEROUTPUT ON;
```

### Show current user

```sql
SHOW USER;
```

### Describe table

```sql
DESC employees;
```

### Execute script

```sql
@script.sql
```

### Clear screen

```sql
CLEAR SCREEN;
```



### Expected final project result

After completing all the project steps, this query:

```sql
SELECT e.employee_id,
       e.first_name,
       e.last_name,
       e.salary,
       fn_bonus(e.salary) AS bonus,
       d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

Expected result before executing the salary increase procedure:

| employee_id | first_name | last_name | salary | bonus | department_name |
|---:|---|---|---:|---:|---|
| 1 | Ana | Torres | 1500 | 150 | Technology |
| 2 | Luis | Perez | 1800 | 180 | Technology |
| 3 | Marta | Gomez | 1300 | 130 | Finance |

After executing:

```sql
BEGIN
    pr_increase_salary(1, 15);
END;
/
```

Expected result for employee `1`:

| employee_id | first_name | old_salary | new_salary |
|---:|---|---:|---:|
| 1 | Ana | 1500 | 1725 |

The function calculates the bonus, the procedure updates the salary, and the trigger protects the table from invalid salary values.


---

## Mini Practice Project

This project creates a small employee management database using Oracle SQL and PL/SQL.

### Step 1: Create tables

```sql
CREATE TABLE departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100) NOT NULL
);

CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    email VARCHAR2(100) UNIQUE,
    salary NUMBER(10,2) NOT NULL,
    hire_date DATE DEFAULT SYSDATE,
    department_id NUMBER,
    CONSTRAINT fk_emp_dept
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id),
    CONSTRAINT chk_emp_salary
        CHECK (salary >= 0)
);
```

### Step 2: Create sequence

```sql
CREATE SEQUENCE employee_seq
START WITH 1
INCREMENT BY 1;
```

### Step 3: Insert data

```sql
INSERT INTO departments VALUES (1, 'Technology');
INSERT INTO departments VALUES (2, 'Finance');
INSERT INTO departments VALUES (3, 'Human Resources');

INSERT INTO employees VALUES (
    employee_seq.NEXTVAL,
    'Ana',
    'Torres',
    'ana.torres@example.com',
    1500,
    SYSDATE,
    1
);

INSERT INTO employees VALUES (
    employee_seq.NEXTVAL,
    'Luis',
    'Perez',
    'luis.perez@example.com',
    1800,
    SYSDATE,
    1
);

INSERT INTO employees VALUES (
    employee_seq.NEXTVAL,
    'Marta',
    'Gomez',
    'marta.gomez@example.com',
    1300,
    SYSDATE,
    2
);

COMMIT;
```

### Step 4: Create function

```sql
CREATE OR REPLACE FUNCTION fn_bonus (
    p_salary NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN p_salary * 0.10;
END;
/
```

### Step 5: Create procedure

```sql
CREATE OR REPLACE PROCEDURE pr_increase_salary (
    p_employee_id NUMBER,
    p_percentage NUMBER
)
IS
BEGIN
    UPDATE employees
    SET salary = salary + (salary * p_percentage / 100)
    WHERE employee_id = p_employee_id;

    IF SQL%ROWCOUNT = 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Employee not found');
    END IF;

    COMMIT;
END;
/
```

### Step 6: Create trigger

```sql
CREATE OR REPLACE TRIGGER trg_validate_employee_salary
BEFORE INSERT OR UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Salary cannot be negative');
    END IF;
END;
/
```

### Step 7: Query data

```sql
SELECT e.employee_id,
       e.first_name,
       e.last_name,
       e.salary,
       fn_bonus(e.salary) AS bonus,
       d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

### Step 8: Execute procedure

```sql
BEGIN
    pr_increase_salary(1, 15);
END;
/
```

---

## Main Command Summary

| Category | Commands |
|---|---|
| DDL | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`, `COMMENT` |
| DML | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE` |
| TCL | `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION` |
| DCL | `GRANT`, `REVOKE` |
| PL/SQL | `DECLARE`, `BEGIN`, `END`, `IF`, `CASE`, `LOOP`, `WHILE`, `FOR`, `EXCEPTION` |
| PL/SQL Objects | `PROCEDURE`, `FUNCTION`, `PACKAGE`, `TRIGGER` |
| Cursors | `CURSOR`, `OPEN`, `FETCH`, `CLOSE`, `%ROWCOUNT`, `%FOUND`, `%NOTFOUND`, `%ISOPEN` |
| Dynamic SQL | `EXECUTE IMMEDIATE` |
| Bulk Processing | `BULK COLLECT`, `FORALL` |
| Security | `CREATE USER`, `CREATE ROLE`, `GRANT`, `REVOKE` |
| Data Dictionary | `USER_TABLES`, `USER_OBJECTS`, `USER_SOURCE`, `USER_TAB_COLUMNS`, `USER_CONSTRAINTS`, `USER_INDEXES` |

---

## Common Errors

| Error | Cause | Possible Solution |
|---|---|---|
| `ORA-00942` | Table or view does not exist | Check table name, schema, or privileges |
| `ORA-00904` | Invalid identifier | Check column name or alias |
| `ORA-00001` | Unique constraint violated | Avoid duplicate values in unique columns |
| `ORA-01403` | No data found | Handle with `NO_DATA_FOUND` exception |
| `ORA-01422` | Exact fetch returns more than requested number of rows | Make `SELECT INTO` return only one row |
| `ORA-06550` | PL/SQL compilation error | Review syntax and line number |
| `ORA-02291` | Foreign key parent key not found | Insert parent record first |
| `ORA-02292` | Child record exists | Delete child records before parent |
| `PLS-00103` | PL/SQL syntax error | Check missing keywords, semicolons, or block structure |

---

## Technical Interview Questions

### SQL Questions

1. What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?
2. What is the difference between `WHERE` and `HAVING`?
3. What is a primary key?
4. What is a foreign key?
5. What is the difference between `INNER JOIN` and `LEFT JOIN`?
6. What is a subquery?
7. What is an index and when should it be used?
8. What is normalization?
9. What is the difference between `UNIQUE` and `PRIMARY KEY`?
10. What is the purpose of `GROUP BY`?
11. What is the difference between `COUNT(*)` and `COUNT(column_name)`?
12. What is the purpose of `MERGE`?
13. What is the `DUAL` table?
14. What is a view?
15. What is a synonym?

### PL/SQL Questions

1. What is PL/SQL?
2. What is the structure of a PL/SQL block?
3. What is the difference between a procedure and a function?
4. What is a cursor?
5. What is the difference between an implicit and explicit cursor?
6. What is a trigger?
7. What are `:OLD` and `:NEW` in triggers?
8. What is exception handling?
9. What is `%TYPE` used for?
10. What is `%ROWTYPE` used for?
11. What is a package?
12. What is dynamic SQL?
13. What is `BULK COLLECT`?
14. What is `FORALL`?
15. What is the purpose of `RAISE_APPLICATION_ERROR`?
16. What is the difference between `IN`, `OUT`, and `IN OUT` parameters?
17. What is the difference between `NEXTVAL` and `CURRVAL`?
18. Why should unnecessary `COMMIT` statements be avoided inside reusable procedures?

---

## Recommended Study Order

1. `SELECT`, `WHERE`, `ORDER BY`
2. `INSERT`, `UPDATE`, `DELETE`
3. `COMMIT`, `ROLLBACK`, `SAVEPOINT`
4. `CREATE TABLE`, constraints, and relationships
5. Joins and subqueries
6. Oracle functions and the `DUAL` table
7. PL/SQL blocks
8. Variables, `%TYPE`, and `%ROWTYPE`
9. Conditional statements and loops
10. Exceptions
11. Cursors
12. Procedures
13. Functions
14. Packages
15. Triggers
16. Sequences and identity columns
17. Views and indexes
18. Users, roles, and privileges
19. Dynamic SQL
20. Collections and bulk processing
21. Mini practice project

---

## Study Checklist

### SQL Basics

- [ ] Understand tables, rows, and columns.
- [ ] Use `SELECT` queries.
- [ ] Filter data with `WHERE`.
- [ ] Sort data with `ORDER BY`.
- [ ] Use aggregate functions.
- [ ] Group data with `GROUP BY`.
- [ ] Filter groups with `HAVING`.
- [ ] Use the `DUAL` table.

### Database Structure

- [ ] Create tables.
- [ ] Modify tables.
- [ ] Drop tables.
- [ ] Define primary keys.
- [ ] Define foreign keys.
- [ ] Use constraints.
- [ ] Create views.
- [ ] Create indexes.
- [ ] Create synonyms.

### Data Manipulation

- [ ] Insert records.
- [ ] Update records.
- [ ] Delete records.
- [ ] Use `MERGE`.

### Transactions

- [ ] Use `COMMIT`.
- [ ] Use `ROLLBACK`.
- [ ] Use `SAVEPOINT`.

### PL/SQL

- [ ] Write anonymous blocks.
- [ ] Declare variables.
- [ ] Use constants.
- [ ] Use `%TYPE`.
- [ ] Use `%ROWTYPE`.
- [ ] Use `IF` statements.
- [ ] Use `CASE` statements.
- [ ] Use loops.
- [ ] Use `SELECT INTO`.
- [ ] Handle exceptions.

### Advanced PL/SQL

- [ ] Create procedures.
- [ ] Create functions.
- [ ] Create packages.
- [ ] Create triggers.
- [ ] Use cursors.
- [ ] Use dynamic SQL.
- [ ] Use collections.
- [ ] Use bulk processing.

### Security and Administration Basics

- [ ] Create users.
- [ ] Create roles.
- [ ] Grant privileges.
- [ ] Revoke privileges.
- [ ] Query data dictionary views.

---

## Recommended Resources

- Oracle Database Documentation: https://docs.oracle.com/en/database/
- Oracle SQL Language Reference: https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/
- Oracle PL/SQL Language Reference: https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/
- Oracle Live SQL: https://livesql.oracle.com/
- Oracle SQL Developer: https://www.oracle.com/database/sqldeveloper/

---

## Notes

This README is part of the `developer-knowledge-base` repository.

Recommended file path:

```text
databases/oracle-plsql/README.md
```
