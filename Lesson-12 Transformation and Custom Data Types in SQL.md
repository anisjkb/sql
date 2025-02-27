# Transformation and Custom Data Types in SQL

## 1. Data Transformation in SQL

Transformation in SQL refers to modifying data to meet specific requirements, including aggregating, formatting, or restructuring data using SQL functions. This is critical for data cleaning, reporting, and ensuring compatibility between systems.

### Common SQL Transformations
- **Aggregation:** Using `SUM()`, `AVG()`, `COUNT()`, `GROUP BY`, etc.
- **Data Formatting:** Using `CAST()`, `CONVERT()`, `SUBSTRING()`, `REPLACE()`, etc.
- **Pivoting & Unpivoting:** Converting rows to columns (`PIVOT`) or vice versa (`UNPIVOT`).
- **Conditional Logic:** Using `CASE WHEN` for conditional transformations.
- **Joins & Subqueries:** Merging and filtering data.

### Common Transformations with Examples

#### **1.1 Data Type Conversion (CAST, CONVERT)**
```sql
SELECT CAST('123' AS INT) AS int_value;
```
**Output:**
| int_value |
|-----------|
| 123       |

#### **1.2 String Transformations (UPPER, LOWER, CONCAT, SUBSTRING)**
```sql
SELECT CONCAT('Hello', ' ', 'World') AS greeting;
```
**Output:**
| greeting    |
|------------|
| Hello World |

#### **1.3 Date/Time Transformations (DATEADD, DATEDIFF, FORMAT)**
```sql
SELECT DATEADD(DAY, 5, '2024-01-01') AS new_date;
```
**Output:**
| new_date  |
|-----------|
| 2024-01-06 |

#### **1.4 Mathematical Transformations (ROUND, ABS)**
```sql
SELECT ROUND(123.456, 2) AS rounded_value;
```
**Output:**
| rounded_value |
|--------------|
| 123.46       |

#### **1.5 Conditional Transformations (CASE)**
```sql
SELECT id, amount,
       CASE
           WHEN amount > 500 THEN 'High'
           ELSE 'Low'
       END AS category
FROM sales;
```

## 2. Pivoting & Unpivoting in SQL

### **Pivoting**
Pivoting transforms rows into columns.

#### **Example: Pivoting Sales Data**
```sql
CREATE TABLE practice.sales (
    id SERIAL,
    year INT,
    amount INT,
    quarter INT
);

INSERT INTO practice.sales (year, quarter, amount) VALUES
(2023, 1, 300),
(2023, 2, 600),
(2023, 3, 400),
(2022, 3, 500);

SELECT * FROM practice.sales;
```

Using `CROSSTAB` for pivoting:
```sql
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT * FROM CROSSTAB(
    'SELECT year, quarter, amount FROM practice.sales ORDER BY 1, 2',
    'SELECT DISTINCT quarter FROM practice.sales ORDER BY 1'
) AS ct (
    year INT, Quarter1 INT, Quarter2 INT, Quarter3 INT
) ORDER BY year DESC;
```

### **Unpivoting**
Unpivoting transforms columns into rows.

```sql
SELECT
    year,
    1 AS quarter,
    Quarter1 AS amount
FROM practice.sales_pivoted
WHERE Quarter1 IS NOT NULL
UNION ALL
SELECT
    year,
    2 AS quarter,
    Quarter2 AS amount
FROM practice.sales_pivoted
WHERE Quarter2 IS NOT NULL
UNION ALL
SELECT
    year,
    3 AS quarter,
    Quarter3 AS amount
FROM practice.sales_pivoted
WHERE Quarter3 IS NOT NULL;
```

## 3. Custom Data Types in SQL

Custom Data Types allow defining new types for better data consistency and management.

### **3.1 Composite Types**
```sql
CREATE TYPE deptinfotype AS (
    dept_name VARCHAR,
    mgr_id INT,
    dept_desc TEXT
);

CREATE TABLE practice.dept (
    id SERIAL,
    dept_info deptinfotype,
    location INT
);
```

**Insert Data:**
```sql
INSERT INTO practice.dept (dept_info, location) VALUES
(ROW('English', 765, 'This dept was created for bla bla bla'), 76547);
```

**Query Composite Data:**
```sql
SELECT * FROM practice.dept;
```

### **3.2 Enumerated (ENUM) Types**
```sql
CREATE TYPE emotion_type AS ENUM ('happy', 'sad', 'neutral');

CREATE TABLE practice.emotions (
    id SERIAL,
    emotion emotion_type,
    human_name VARCHAR
);

INSERT INTO practice.emotions (emotion, human_name) VALUES
('happy', 'Mr X'),
('sad', 'Mr Y');
```

### **3.3 Range Types**
```sql
CREATE TYPE callduration AS RANGE (SUBTYPE = TIME);

CREATE TABLE practice.call_history (
    chid SERIAL,
    duration callduration,
    _user INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **3.4 User-Defined Data Type (UDT) in PostgreSQL**
```sql
CREATE TYPE address AS (
    street VARCHAR(50),
    city VARCHAR(50),
    country VARCHAR(50)
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    home_address address
);

INSERT INTO employees (name, home_address)
VALUES ('Alice', ('123 Main St', 'Dhaka', 'Bangladesh'));
```

### **3.5 User-Defined Object Type in Oracle**
```sql
CREATE OR REPLACE TYPE address AS OBJECT (
    street VARCHAR2(50),
    city VARCHAR2(50),
    country VARCHAR2(50)
);

CREATE TABLE employees (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(50),
    home_address address
);

INSERT INTO employees (id, name, home_address)
VALUES (1, 'Alice', address('123 Main St', 'Dhaka', 'Bangladesh'));
```

## **Conclusion**
- **Transformations** modify and aggregate data for better analysis.
- **Custom Data Types** improve data consistency, validation, and structuring.
- **Use cases** range from defining phone number types to passing table-valued parameters or using ENUMs for status management.