# 🔤 String Manipulation and 🔄 Data Wrangling in Data Analysis

String manipulation and data wrangling are essential in data analysis for cleaning, transforming, and preparing data for insights.

## 🔤 **String Manipulation**
String manipulation involves modifying, parsing, or extracting information from text data.

### **Common SQL String Functions:**
- `CONCAT()`: Combines multiple strings.
- `SUBSTRING()`: Extracts part of a string.
- `LENGTH()`: Returns the length of a string.
- `LOWER()` / `UPPER()`: Converts text to lowercase or uppercase.
- `TRIM()`: Removes leading/trailing spaces.
- `REPLACE()`: Replaces occurrences of a substring.

### **Example: Extract Initials and Standardize Case**
```sql
SELECT first_name, last_name,
       CONCAT(UPPER(SUBSTRING(first_name, 1, 1)), '.', UPPER(SUBSTRING(last_name, 1, 1))) AS initials
FROM practice.duplicate_employees;
```

**Output:**
| first_name | last_name | initials |
|------------|-----------|----------|
| John       | Doe       | J.D.     |
| Alice      | Smith     | A.S.     |

---

## 🔄 **Data Wrangling**
Data wrangling involves cleaning, reshaping, and transforming raw data into a usable format.

### **Common SQL Techniques:**
- **Filtering**: Using `WHERE` and `HAVING`.
- **Aggregation**: Using `SUM()`, `AVG()`, `COUNT()`.
- **Handling Missing Data**: Using `COALESCE()` to fill null values.
- **Data Merging**: Using `JOIN` to combine data from different tables.

### **Example: Handling Missing Salaries and Grouping**
```sql
SELECT department_id,
       COUNT(*) AS employee_count,
       COALESCE(AVG(salary), 0) AS avg_salary
FROM practice.duplicate_employees
GROUP BY department_id;
```

**Output:**
| department_id | employee_count | avg_salary |
|---------------|---------------|------------|
| 101           | 5             | 45000      |
| 102           | 3             | 0          |

---

This guide provides essential SQL functions and techniques to help with efficient data preparation and analysis.