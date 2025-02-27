# SQL Filtering, Uniqueness, and Aggregation Options

## Introduction
SQL provides powerful features to manipulate and analyze data efficiently. Three key aspects of SQL operations are:

1. **Filtering**: Extracting specific rows from a dataset based on conditions.
2. **Uniqueness**: Ensuring that retrieved or stored data does not contain duplicates.
3. **Aggregation**: Performing calculations on grouped data to obtain summarized results.

This document explains various SQL filtering, uniqueness, and aggregation options along with example queries and their corresponding outputs.

## Filtering Options

### WHERE Clause
Filters rows based on specified conditions.

```sql
SELECT * FROM Employees WHERE department = 'Sales';
```
#### Output:
| employee_id | name  | department | salary |
|-------------|-------|------------|--------|
| 1           | Alice | Sales      | 50000  |
| 2           | Bob   | Sales      | 52000  |

### BETWEEN
Selects values within a specified range.

```sql
SELECT * FROM Employees WHERE salary BETWEEN 40000 AND 60000;
```
#### Output:
| employee_id | name  | department | salary |
|-------------|-------|------------|--------|
| 1           | Alice | Sales      | 50000  |
| 2           | Bob   | Sales      | 52000  |
| 3           | Carol | HR         | 45000  |

### IN
Matches any value in a list.

```sql
SELECT * FROM Employees WHERE department IN ('Sales', 'IT');
```
#### Output:
| employee_id | name  | department | salary |
|-------------|-------|------------|--------|
| 1           | Alice | Sales      | 50000  |
| 2           | Bob   | Sales      | 52000  |
| 4           | Dave  | IT         | 60000  |

### LIKE
Searches for a specified pattern in a column.

```sql
SELECT * FROM Employees WHERE name LIKE 'A%';  -- Names starting with 'A'
```
#### Output:
| employee_id | name  | department | salary |
|-------------|-------|------------|--------|
| 1           | Alice | Sales      | 50000  |

### IS NULL / IS NOT NULL
Filters rows with NULL or non-NULL values.

```sql
SELECT * FROM Employees WHERE manager_id IS NULL;
```
#### Output:
| employee_id | name  | department | salary | manager_id |
|-------------|-------|------------|--------|------------|
| 1           | Alice | Sales      | 50000  | NULL       |

### AND / OR
Combines multiple conditions.

```sql
SELECT * FROM Employees WHERE department = 'Sales' AND salary > 50000;
```
#### Output:
| employee_id | name  | department | salary |
|-------------|-------|------------|--------|
| 2           | Bob   | Sales      | 52000  |

## Uniqueness Options

### DISTINCT
Removes duplicate rows from the result set.

```sql
SELECT DISTINCT department FROM Employees;
```
#### Output:
| department |
|------------|
| Sales      |
| HR         |
| IT         |

### PRIMARY KEY
Ensures that a column (or combination of columns) has unique values.

```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50)
);
```
(No output, table creation)

### UNIQUE Constraint
Ensures all values in a column are unique.

```sql
ALTER TABLE Employees ADD UNIQUE (email);
```
(No output, constraint addition)

## Aggregation Options

### SUM()
Returns the sum of values.

```sql
SELECT department, SUM(salary) AS total_salary FROM Employees GROUP BY department;
```
#### Output:
| department | total_salary |
|------------|-------------|
| Sales      | 102000      |
| HR         | 45000       |
| IT         | 60000       |

### AVG()
Returns the average of values.

```sql
SELECT department, AVG(salary) AS avg_salary FROM Employees GROUP BY department;
```
#### Output:
| department | avg_salary |
|------------|------------|
| Sales      | 51000      |
| HR         | 45000      |
| IT         | 60000      |

### COUNT()
Returns the number of rows or non-NULL values.

```sql
SELECT department, COUNT(*) AS total_employees FROM Employees GROUP BY department;
```
#### Output:
| department | total_employees |
|------------|----------------|
| Sales      | 2              |
| HR         | 1              |
| IT         | 1              |

### MAX()
Returns the maximum value.

```sql
SELECT department, MAX(salary) AS highest_salary FROM Employees GROUP BY department;
```
#### Output:
| department | highest_salary |
|------------|----------------|
| Sales      | 52000          |
| HR         | 45000          |
| IT         | 60000          |

### MIN()
Returns the minimum value.

```sql
SELECT department, MIN(salary) AS lowest_salary FROM Employees GROUP BY department;
```
#### Output:
| department | lowest_salary |
|------------|---------------|
| Sales      | 50000         |
| HR         | 45000         |
| IT         | 60000         |

### GROUP BY
Groups rows that have the same values into summary rows.

```sql
SELECT department, COUNT(*) AS total_employees FROM Employees GROUP BY department;
```
#### Output:
| department | total_employees |
|------------|----------------|
| Sales      | 2              |
| HR         | 1              |
| IT         | 1              |

### HAVING
Filters groups based on specified conditions (used with GROUP BY).

```sql
SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department
HAVING AVG(salary) > 50000;
```
#### Output:
| department | avg_salary |
|------------|------------|
| IT         | 60000      |