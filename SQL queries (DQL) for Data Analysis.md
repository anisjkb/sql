# SQL Data Analysis Queries (DQL) with Examples

This document provides a collection of SQL queries demonstrating various data analysis techniques, including distinct values, grouping, aggregation, and filtering.  It uses a table named `practice.duplicate_employees` as the data source.

# Data Query Language (DQL) in SQL

In SQL, **DQL (Data Query Language)** focuses on fetching data from the database. The main command used in DQL is `SELECT`. While **DML (Data Manipulation Language)** modifies data (using `INSERT`, `UPDATE`, `DELETE`), DQL is purely for querying data and retrieving information.

## 🔍 Main Features of DQL

- Retrieves data from tables or views.
- Supports filtering, sorting, grouping, and aggregation.
- Can be combined with joins, subqueries, and window functions.

## ✅ Key Points to Remember in DQL

- **SELECT**: Retrieves specific columns from a table.
- **WHERE**: Filters data based on conditions.
- **GROUP BY**: Groups data for aggregation functions.
- **HAVING**: Filters grouped data.
- **ORDER BY**: Sorts the result set.

## Base Tables

### Table: `practice.duplicate_employees`

| Column Name   | Data Type    | Description                       |
|--------------|-------------|-----------------------------------|
| employee_id  | INT         | Unique identifier for each employee |
| first_name   | VARCHAR(50) | First name of the employee        |
| last_name    | VARCHAR(50) | Last name of the employee         |
| birth_date   | DATE        | Birth date of the employee        |
| gender       | VARCHAR(10) | Gender of the employee            |
| salary       | INT         | Salary of the employee            |
| department_id| INT         | Identifier for the department     |
| job_title    | VARCHAR(50) | Job title of the employee         |
| manager      | INT         | Manager ID of the employee        |

**Sample Data:**

| employee_id | first_name | last_name | birth_date | gender | salary | department_id | job_title | manager |
|-------------|------------|-----------|------------|--------|--------|---------------|-----------|---------|
| 1           | John       | Doe       | 1990-01-01 | Male   | 60000  | 1             | Manager   | 101     |
| 2           | Jane       | Smith     | 1985-02-15 | Female | 70000  | 2             | Engineer  | 102     |
| 3           | Alice      | Brown     | 1980-03-30 | Female | 80000  | 3             | Analyst   | 103     |
| 4           | Bob        | Johnson   | 1975-04-20 | Male   | 90000  | 4             | Developer | 104     |

### Table: `practice.departments`

| Column Name     | Data Type    | Description                          |
|----------------|-------------|--------------------------------------|
| department_id  | INT         | Unique identifier for each department |
| department_name| VARCHAR(50) | Name of the department               |
| manager        | INT         | Manager ID of the department         |

**Sample Data:**

| department_id | department_name | manager |
|---------------|-----------------|---------|
| 1             | HR              | 101     |
| 2             | IT              | 102     |
| 3             | Finance         | 103     |
| 4             | Marketing       | 104     |

---

## 1. Calculating Aggregate Functions

**Example: Total Number of Employees**

```sql
SELECT COUNT(*) AS total_employees
FROM practice.duplicate_employees;
```

| total_employees |
|-----------------|
| 100             |

**Example: Total Salary**

```sql
SELECT SUM(salary) AS total_salary
FROM practice.duplicate_employees;
```

| total_salary |
|--------------|
| 5000000      |

---

## 2. Grouping Data

**Example: Counting Employees by Gender**

```sql
SELECT gender, COUNT(*) AS count
FROM practice.duplicate_employees
GROUP BY gender;
```

| gender | count |
|--------|-------|
| Male   | 60    |
| Female | 40    |

**Example: Average Salary by Gender**

```sql
SELECT gender, AVG(salary) AS average_salary
FROM practice.duplicate_employees
GROUP BY gender;
```

| gender | average_salary |
|--------|----------------|
| Male   | 55000          |
| Female | 45000          |

---

## 3. Filtering Data with HAVING

**Example: Gender with More Than 10 Employees**

```sql
SELECT gender, COUNT(*) AS count
FROM practice.duplicate_employees
GROUP BY gender
HAVING COUNT(*) > 10;
```

| gender | count |
|--------|-------|
| Male   | 60    |
| Female | 40    |

---

## 4. Using Window Functions

**Example: Running Total of Salaries**

```sql
SELECT first_name, salary, SUM(salary) OVER (ORDER BY salary) AS running_total
FROM practice.duplicate_employees;
```

| first_name | salary | running_total |
|------------|--------|---------------|
| John       | 30000  | 30000         |
| Jane       | 35000  | 65000         |
| Alice      | 40000  | 105000        |
| Bob        | 45000  | 150000        |

---

## 5. Performing Subqueries

**Example: Employees with Salary Greater than Average**

```sql
SELECT first_name, salary
FROM practice.duplicate_employees
WHERE salary > (SELECT AVG(salary) FROM practice.duplicate_employees);
```

| first_name | salary |
|------------|--------|
| John       | 60000  |
| Jane       | 70000  |
| Alice      | 80000  |
| Bob        | 90000  |

---

## 6. Using Common Table Expressions (CTEs)

**Example: Employees Older Than Average Age**

```sql
WITH AvgAge AS (
  SELECT AVG(EXTRACT(YEAR FROM AGE(birth_date))) AS average_age
  FROM practice.duplicate_employees
)
SELECT first_name, EXTRACT(YEAR FROM AGE(birth_date)) AS age
FROM practice.duplicate_employees
WHERE EXTRACT(YEAR FROM AGE(birth_date)) > (SELECT average_age FROM AvgAge);
```

| first_name | age |
|------------|-----|
| John       | 35  |
| Jane       | 40  |
| Alice      | 45  |
| Bob        | 50  |

---

## 7. Joining Tables

**Example: Employee Details with Department Names**

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM practice.duplicate_employees e
JOIN practice.departments d
ON e.department_id = d.department_id;
```

| first_name | last_name | department_name |
|------------|-----------|-----------------|
| John       | Doe       | HR              |
| Jane       | Smith     | IT              |
| Alice      | Brown     | Finance         |
| Bob        | Johnson   | Marketing       |

---

## 8. Using String Functions

**Example: Extracting Initials of Employees**

```sql
SELECT CONCAT(SUBSTRING(first_name, 1, 1), SUBSTRING(last_name, 1, 1)) AS initials
FROM practice.duplicate_employees;
```

| initials |
|----------|
| JD       |
| JS       |
| AB       |
| BJ       |

---

## 9. Applying Date Functions

**Example: Extracting Year of Birth**

```sql
SELECT first_name, EXTRACT(YEAR FROM birth_date) AS birth_year
FROM practice.duplicate_employees;
```

| first_name | birth_year |
|------------|------------|
| John       | 1990       |
| Jane       | 1985       |
| Alice      | 1980       |
| Bob        | 1975       |

---

## 10. Using Mathematical Functions

**Example: Calculating Bonus as 10% of Salary**

```sql
SELECT first_name, salary, salary * 0.10 AS bonus
FROM practice.duplicate_employees;
```

| first_name | salary | bonus |
|------------|--------|-------|
| John       | 60000  | 6000  |
| Jane       | 70000  | 7000  |
| Alice      | 80000  | 8000  |
| Bob        | 90000  | 9000  |


## 7. Using Conditional Statements

**Example: Categorizing Employees by Salary**

```sql
SELECT first_name, salary, 
       CASE 
         WHEN salary > 500000 THEN 'High'
         WHEN salary BETWEEN 200000 AND 500000 THEN 'Medium'
         ELSE 'Low'
       END AS salary_category
FROM practice.duplicate_employees;
```

| first_name | salary | salary_category |
|------------|--------|-----------------|
| John       | 60000  | Low             |
| Jane       | 70000  | Low             |
| Alice      | 80000  | Low             |
| Bob        | 90000  | Low             |

---

## 8. Joining Multiple Tables

**Example: Joining Employees and Departments**

```sql
SELECT e.first_name, e.salary, d.department_name 
FROM practice.duplicate_employees e
JOIN practice.departments d
ON e.department_id = d.department_id;
```

| first_name | salary | department_name |
|------------|--------|-----------------|
| John       | 60000  | HR              |
| Jane       | 70000  | IT              |
| Alice      | 80000  | Finance         |
| Bob        | 90000  | Marketing       |

---

## 9. Using Distinct Values

**Example: Select Distinct Job Titles**

```sql
SELECT DISTINCT job_title 
FROM practice.duplicate_employees;
```

| job_title |
|-----------|
| Manager   |
| Engineer  |
| Analyst   |
| Developer |

---

## 10. Using String Functions

**Example: Concatenating First Name and Last Name**

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM practice.duplicate_employees;
```

| full_name       |
|-----------------|
| John Doe        |
| Jane Smith      |
| Alice Brown     |
| Bob Johnson     |

---

## 11. Using Date Functions

**Example: Extracting the Year of Birth**

```sql
SELECT first_name, EXTRACT(YEAR FROM birth_date) AS birth_year
FROM practice.duplicate_employees;
```

| first_name | birth_year |
|------------|------------|
| John       | 1990       |
| Jane       | 1985       |
| Alice      | 1980       |
| Bob        | 1975       |

---

## 12. Using Mathematical Functions

**Example: Calculating Salary Difference**

```sql
SELECT first_name, salary,
       LAG(salary) OVER (ORDER BY salary) AS previous_salary,
       salary - LAG(salary) OVER (ORDER BY salary) AS salary_difference
FROM practice.duplicate_employees;
```

| first_name | salary | previous_salary | salary_difference |
|------------|--------|----------------|-------------------|
| John       | 60000  | NULL           | NULL              |
| Jane       | 70000  | 60000          | 10000             |
| Alice      | 80000  | 70000          | 10000             |
| Bob        | 90000  | 80000          | 10000             |

---

# SQL Data Analysis Techniques with Examples (Additional)

This document provides a comprehensive guide to SQL queries for data analysis. It covers fundamental and advanced techniques, such as distinct values, grouping, aggregation, filtering, joining tables, and using functions. The examples use the `practice.duplicate_employees` table as the primary data source.

## Base Tables

### Table: `practice.duplicate_employees`

| Column Name    | Data Type   | Description                         |
| -------------- | ----------- | ----------------------------------- |
| employee\_id   | INT         | Unique identifier for each employee |
| first\_name    | VARCHAR(50) | First name of the employee          |
| last\_name     | VARCHAR(50) | Last name of the employee           |
| birth\_date    | DATE        | Birth date of the employee          |
| gender         | VARCHAR(10) | Gender of the employee              |
| salary         | INT         | Salary of the employee              |
| department\_id | INT         | Identifier for the department       |
| job\_title     | VARCHAR(50) | Job title of the employee           |
| manager        | INT         | Manager ID of the employee          |

### Table: `practice.departments`

| Column Name      | Data Type   | Description                           |
| ---------------- | ----------- | ------------------------------------- |
| department\_id   | INT         | Unique identifier for each department |
| department\_name | VARCHAR(50) | Name of the department                |
| manager          | INT         | Manager ID of the department          |

---

## 1. Calculating Aggregate Functions

Aggregate functions perform calculations on a set of values and return a single value. Common functions include `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.

**Example: Total Number of Employees**

```sql
SELECT COUNT(*) AS total_employees
FROM practice.duplicate_employees;
```

| total_employees |
| --------------- |
| 100             |

**Example: Total Salary**

```sql
SELECT SUM(salary) AS total_salary
FROM practice.duplicate_employees;
```

| total_salary |
| ------------ |
| 5000000      |

---

## 2. Grouping Data with GROUP BY

The `GROUP BY` clause groups rows that have the same values into summary rows, often used with aggregate functions.

**Example: Counting Employees by Gender**

```sql
SELECT gender, COUNT(*) AS count
FROM practice.duplicate_employees
GROUP BY gender;
```

| gender | count |
| ------ | ----- |
| Male   | 60    |
| Female | 40    |

**Example: Average Salary by Gender**

```sql
SELECT gender, AVG(salary) AS average_salary
FROM practice.duplicate_employees
GROUP BY gender;
```

| gender | average_salary |
| ------ | -------------- |
| Male   | 55000          |
| Female | 48000          |

---

## 3. Filtering Aggregates with HAVING

The `HAVING` clause filters results after grouping data, unlike `WHERE`, which filters before grouping.

**Example: Genders with More Than 10 Employees**

```sql
SELECT gender, COUNT(*) AS count
FROM practice.duplicate_employees
GROUP BY gender
HAVING COUNT(*) > 10;
```

| gender | count |
| ------ | ----- |
| Male   | 60    |
| Female | 40    |

---

## 4. Using Window Functions

Window functions perform calculations across a set of table rows related to the current row.

**Example: Running Total of Salaries**

```sql
SELECT first_name, salary, SUM(salary) OVER (ORDER BY salary) AS running_total
FROM practice.duplicate_employees;
```

| first_name | salary | running_total |
| ---------- | ------ | ------------- |
| Alice      | 30000  | 30000         |
| Bob        | 40000  | 70000         |
| Carol      | 50000  | 120000        |

**Example: Salary Difference Between Consecutive Employees**

```sql
SELECT first_name, salary,
       LAG(salary) OVER (ORDER BY salary) AS previous_salary,
       salary - LAG(salary) OVER (ORDER BY salary) AS salary_difference
FROM practice.duplicate_employees;
```

| first_name | salary | previous_salary | salary_difference |
| ---------- | ------ | --------------- | ----------------- |
| Alice      | 30000  | NULL            | NULL              |
| Bob        | 40000  | 30000           | 10000             |
| Carol      | 50000  | 40000           | 10000             |

---

## 5. Performing Subqueries

A subquery is a query nested inside another SQL query.

**Example: Employees Earning Above the Average Salary**

```sql
SELECT first_name, salary
FROM practice.duplicate_employees
WHERE salary > (SELECT AVG(salary) FROM practice.duplicate_employees);
```

| first_name | salary |
| ---------- | ------ |
| David      | 60000  |
| Emma       | 70000  |

---

## 6. Using Common Table Expressions (CTEs)

CTEs allow temporary result sets that can be referenced within the main query.

**Example: Employees Older Than Average Age**

```sql
WITH AvgAge AS (
  SELECT AVG(EXTRACT(YEAR FROM AGE(birth_date))) AS average_age
  FROM practice.duplicate_employees
)
SELECT first_name, EXTRACT(YEAR FROM AGE(birth_date)) AS age
FROM practice.duplicate_employees
WHERE EXTRACT(YEAR FROM AGE(birth_date)) > (SELECT average_age FROM AvgAge);
```

| first_name | age |
| ---------- | --- |
| John       | 45  |
| Sarah      | 50  |

---

## 7. Joining Tables

Joins combine rows from two or more tables based on a related column.

**Example: Employee Details with Department Names**

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM practice.duplicate_employees e
JOIN practice.departments d
ON e.department_id = d.department_id;
```

| first_name | last_name | department_name |
| ---------- | --------- | --------------- |
| Alice      | Johnson   | HR              |
| Bob        | Smith     | IT              |
| Carol      | Brown     | Finance         |

---

## 8. Using String Functions

SQL provides various functions for manipulating strings.

**Example: Extracting Initials of Employees**

```sql
SELECT CONCAT(SUBSTRING(first_name, 1, 1), SUBSTRING(last_name, 1, 1)) AS initials
FROM practice.duplicate_employees;
```

| initials |
| -------- |
| AJ       |
| BS       |
| CB       |

**Example: Concatenating First Name and Last Name**

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM practice.duplicate_employees;
```

| full_name       |
| --------------- |
| Alice Johnson   |
| Bob Smith       |
| Carol Brown     |

---

## 9. Applying Date Functions

Date functions allow manipulation and extraction of date components.

**Example: Extracting Year of Birth**

```sql
SELECT first_name, EXTRACT(YEAR FROM birth_date) AS birth_year
FROM practice.duplicate_employees;
```

| first_name | birth_year |
| ---------- | ---------- |
| Alice      | 1985       |
| Bob        | 1990       |
| Carol      | 1988       |

---

## 10. Using Mathematical Functions

Mathematical functions perform operations on numeric data.

**Example: Calculating Bonus as 10% of Salary**

```sql
SELECT first_name, salary, salary * 0.10 AS bonus
FROM practice.duplicate_employees;
```

| first_name | salary | bonus |
| ---------- | ------ | ----- |
| Alice      | 30000  | 3000  |
| Bob        | 40000  | 4000  |
| Carol      | 50000  | 5000  |

---

## 11. Using Conditional Statements

Conditional statements allow you to return different values based on specific conditions.

**Example: Categorizing Employees by Salary**

```sql
SELECT first_name, salary,
       CASE
         WHEN salary > 500000 THEN 'High'
         WHEN salary BETWEEN 200000 AND 500000 THEN 'Medium'
         ELSE 'Low'
       END AS salary_category
FROM practice.duplicate_employees;
```

| first_name | salary | salary_category |
| ---------- | ------ | --------------- |
| Alice      | 30000  | Low             |
| Bob        | 40000  | Low             |
| Carol      | 250000 | Medium          |

---

## 12. Using DISTINCT Values

The `DISTINCT` keyword removes duplicate values from the result set.

**Example: Select Distinct Job Titles**

```sql
SELECT DISTINCT job_title
FROM practice.duplicate_employees;
```

| job_title      |
| -------------- |
| Manager        |
| Developer      |
| Analyst        |

---

This document offers a solid foundation for SQL data analysis, demonstrating essential querying techniques with practical examples and result tables for easy comparison.