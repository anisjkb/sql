# SQL Advanced Functions: A Comprehensive Guide

In today's discussion, we explored powerful SQL techniques—commonly known as window or analytic functions—that let you perform calculations across a set of related rows while still retaining each row's details. This guide covers:

- **Basic Window Functions**: `ROW_NUMBER()`, `LAG()`, and running totals using `SUM()`
- **Additional Advanced Functions**:  
  - Ranking functions: `RANK()` and `DENSE_RANK()`
  - Directional function: `LEAD()`
  - Bucketing rows: `NTILE(n)`
  - Extremum retrieval: `FIRST_VALUE()` and `LAST_VALUE()`
- **Detailed Scenarios**:  
  - Moving averages  
  - Percentile ranks using `PERCENT_RANK()`  
  - Combining multiple OVER clauses in one query

---

## Table of Contents

1. [Introduction](#introduction)
2. [Window Functions Overview](#window-functions-overview)
3. [Basic Window Function Examples](#basic-window-function-examples)
   - [ROW_NUMBER() Example](#rownumber-example)
   - [LAG() Example](#lag-example)
   - [Running Totals Using SUM() Example](#running-totals-using-sum-example)
4. [Additional Advanced Functions](#additional-advanced-functions)
   - [RANK() and DENSE_RANK() Example](#ranking-functions)
   - [LEAD() Example](#lead-example)
   - [NTILE(n) Example](#ntile-example)
   - [FIRST_VALUE() and LAST_VALUE() Example](#first-value-and-last-value)
5. [Detailed Analytical Scenarios](#detailed-analytical-scenarios)
   - [Moving Averages](#moving-averages)
   - [Percentile Ranks Using PERCENT_RANK()](#percentile-ranks)
   - [Combining Multiple OVER Clauses](#combining-multiple-over-clauses)
6. [Wrapping Up](#wrapping-up)

---

## 1. [Introduction](#introduction)

SQL advanced (window) functions allow you to analyze data deeply—calculating rankings, running totals, and moving averages—without losing row-level details. Unlike traditional aggregate functions that collapse data into summary rows, these functions let you “look around” at neighboring rows and perform calculations on the fly.

---

## 2. Window Functions Overview

When using window functions, the `OVER` clause defines:
- **PARTITION BY**: How to group rows for the function.
- **ORDER BY**: The order of rows within each partition.
- **Frame Specification**: (Optional) Defines the range of rows—for example, all rows from the start of the partition up to the current row.

---

## 3. Basic Window Function Examples

### Sample Table: Employees

Consider the following `Employees` table:

| id | department | salary |
|----|------------|--------|
| 1  | Sales      | 90000  |
| 2  | Sales      | 90000  |
| 3  | Sales      | 70000  |
| 4  | Marketing  | 95000  |
| 5  | Marketing  | 85000  |
| 6  | Marketing  | 85000  |
| 7  | Marketing  | 80000  |

---

### ROW_NUMBER() Example {#rownumber-example}

Assign a sequential number to each employee **within their department ordered by salary (highest first)**:

```sql
SELECT 
  id, 
  department, 
  salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num
FROM Employees;
```
## Expected Output:

| id | department | salary | row_num |
|----|------------|--------|---------|
| 1  | Sales      | 90000  | 1       |
| 2  | Sales      | 90000  | 2       |
| 3  | Sales      | 70000  | 3       |
| 4  | Marketing  | 95000  | 1       |
| 5  | Marketing  | 85000  | 2       |
| 6  | Marketing  | 85000  | 3       |
| 7  | Marketing  | 80000  | 4       |

# SQL Window Functions

## LAG() Example
Fetch each employee’s salary along with the salary of the previous employee (based on ascending order within each department):

```sql
SELECT 
  id, 
  department, 
  salary,
  LAG(salary) OVER (PARTITION BY department ORDER BY salary) AS prev_salary
FROM Employees;
```

### Expected Output:

| id | department | salary | prev_salary |
|----|------------|--------|-------------|
| 3  | Sales      | 70000  | NULL        |
| 1  | Sales      | 90000  | 70000       |
| 2  | Sales      | 90000  | 90000       |
| 7  | Marketing  | 80000  | NULL        |
| 5  | Marketing  | 85000  | 80000       |
| 6  | Marketing  | 85000  | 85000       |
| 4  | Marketing  | 95000  | 85000       |

---

## Running Totals Using SUM()
Compute a running total of salaries within each department:

```sql
SELECT 
  id, 
  department, 
  salary,
  SUM(salary) OVER (
    PARTITION BY department 
    ORDER BY salary 
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
  ) AS running_total
FROM Employees;
```

### Expected Output (for Sales, as an example):

| id | department | salary | running_total |
|----|------------|--------|---------------|
| 3  | Sales      | 70000  | 70000         |
| 1  | Sales      | 90000  | 160000        |
| 2  | Sales      | 90000  | 250000        |

Marketing rows would be computed similarly.

---
# 4. Additional Advanced Functions

## Ranking Functions: RANK() and DENSE_RANK()
Both functions assign a ranking value based on an order (e.g., salary within a department), but they address ties differently:

- **RANK()**: Tied rows receive the same rank; the next rank in order is skipped.
- **DENSE_RANK()**: Tied rows receive the same rank, and the next rank increments by one without gaps.

```sql
SELECT 
  id,
  department,
  salary,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_salary,
  DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank_salary
FROM Employees;
```

### Expected Output:

| id | department | salary | rank_salary | dense_rank_salary |
|----|------------|--------|-------------|--------------------|
| 1  | Sales      | 90000  | 1           | 1                  |
| 2  | Sales      | 90000  | 1           | 1                  |
| 3  | Sales      | 70000  | 3           | 2                  |
| 4  | Marketing  | 95000  | 1           | 1                  |
| 5  | Marketing  | 85000  | 2           | 2                  |
| 6  | Marketing  | 85000  | 2           | 2                  |
| 7  | Marketing  | 80000  | 4           | 3                  |

---
# Directional Function: LEAD()

**LEAD()** returns the value from the next row in the specified order. For example, to capture each employee’s salary along with the next salary in a descending order:

```sql
SELECT 
  id, 
  department, 
  salary,
  LEAD(salary) OVER (PARTITION BY department ORDER BY salary DESC) AS next_salary
FROM Employees;
```

### Expected Output:

| id | department | salary | next_salary |
|----|------------|--------|-------------|
| 1  | Sales      | 90000  | 90000       |
| 2  | Sales      | 90000  | 70000       |
| 3  | Sales      | 70000  | NULL        |
| 4  | Marketing  | 95000  | 85000       |
| 5  | Marketing  | 85000  | 85000       |
| 6  | Marketing  | 85000  | 80000       |
| 7  | Marketing  | 80000  | NULL        |

---

## Bucketing Rows with NTILE(n)

**NTILE(n)** splits rows within a partition into `n` buckets. This can help with percentile or quantile analyses.

```sql
SELECT 
  id,
  department,
  salary,
  NTILE(3) OVER (PARTITION BY department ORDER BY salary DESC) AS bucket
FROM Employees;
```

### Expected Output:

| id | department | salary | bucket |
|----|------------|--------|--------|
| 1  | Sales      | 90000  | 1      |
| 2  | Sales      | 90000  | 2      |
| 3  | Sales      | 70000  | 3      |
| 4  | Marketing  | 95000  | 1      |
| 5  | Marketing  | 85000  | 1      |
| 6  | Marketing  | 85000  | 2      |
| 7  | Marketing  | 80000  | 3      |

---

## Extremum Retrieval with FIRST_VALUE() and LAST_VALUE()
These functions fetch the first or the last value from your window frame. Be especially careful with `LAST_VALUE()`—the default frame may not yield the expected result unless you define it precisely.

```sql
SELECT 
  id, 
  department, 
  salary,
  FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC) AS highest_salary,
  LAST_VALUE(salary) OVER (
    PARTITION BY department 
    ORDER BY salary DESC 
    ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
  ) AS lowest_salary
FROM Employees;
```

### Expected Output:

| id | department | salary | highest_salary | lowest_salary |
|----|------------|--------|----------------|---------------|
| 1  | Sales      | 90000  | 90000          | 70000        |
| 2  | Sales      | 90000  | 90000          | 70000        |
| 3  | Sales      | 70000  | 90000          | 70000        |
| 4  | Marketing  | 95000  | 95000          | 80000        |
| 5  | Marketing  | 85000  | 95000          | 80000        |
| 6  | Marketing  | 85000  | 95000          | 80000        |
| 7  | Marketing  | 80000  | 95000          | 80000        |

---

# SQL Window Functions

## Percentile Ranks Using PERCENT_RANK()
PERCENT_RANK() computes the relative standing of a row within its partition, returning a value between 0 and 1 (where the lowest value is 0 and the highest value approaches 1).

```sql
SELECT
  id,
  department,
  salary,
  PERCENT_RANK() OVER (PARTITION BY department ORDER BY salary) AS percentile_rank
FROM Employees;
```

### Expected Output (Conceptual):
For the Sales department:

The lowest salary (70000) will have a percentile rank of 0.0.

The salaries of 90000 will have a higher rank—values close to 1.0 depending on tie treatment.

| id | department | salary | percentile_rank |
|----|------------|--------|----------------|
| 3  | Sales      | 70000  | 0.0            |
| 1  | Sales      | 90000  | 0.5            |
| 2  | Sales      | 90000  | 0.5 or 1.0     |

## Combining Multiple OVER Clauses
A single query can combine several window functions to extract deep insights. For instance, you might want to retrieve for each employee:

- Their rank within the department (using descending salary)
- The previous salary (using ascending order)
- A moving average that considers the row preceding and following the current row

```sql
SELECT
  id,
  department,
  salary,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_salary,
  LAG(salary) OVER (PARTITION BY department ORDER BY salary) AS previous_salary,
  AVG(salary) OVER (
    PARTITION BY department 
    ORDER BY salary 
    ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
  ) AS surrounding_avg
FROM Employees;
```

### Expected Outcome:

| id | department | salary | rank_salary | previous_salary | surrounding_avg |
|----|------------|--------|-------------|-----------------|-----------------|
| 3  | Sales      | 70000  | 3           | NULL            | 80000           |
| 1  | Sales      | 90000  | 1           | 70000           | 83333           |
| 2  | Sales      | 90000  | 1           | 90000           | 90000           |

*Note: Actual values depend on the data ordering and partitioning.*

## 6. Wrapping Up
In this guide, we’ve covered:

- **Basic Window Functions:** Learn how to use ROW_NUMBER(), LAG(), and running totals to retain row-level detail while performing group-based calculations.
- **Additional Advanced Functions:** Use RANK()/DENSE_RANK() to handle ties, LEAD() for forward-looking comparisons, NTILE(n) for bucketing rows, and FIRST_VALUE()/LAST_VALUE() to retrieve extreme values in a partition.
- **Detailed Analytical Scenarios:** Explore moving averages to smooth time-series data, compute percentile ranks with PERCENT_RANK(), or even combine several analytical functions in one query for in-depth insights.

These techniques are invaluable for making your SQL queries both expressive and efficient. Whether you’re generating reports, analyzing trends, or segmenting data, mastering these tools can transform how you work with data.
