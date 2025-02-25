# SQL Concepts

## DDL (Data Definition Language)
- **Commands:** `CREATE`, `ALTER`, `DROP`
- **Purpose:** Architecture

## DML (Data Manipulation Language)
- **Commands:** `INSERT`, `UPDATE`, `DELETE`
- **Purpose:** Data Layer

## DCL/TCL (Data/Transaction Control Language)
- **Commands:** `GRANT`, `REVOKE`
- **Purpose:** Permission Management

## DTL (Data Transformation Language)
- **Commands:** `COMMIT`, `ROLLBACK`
- **Purpose:** Flow Complete / Revert

## DQL (Data Query Language)
- **Commands:** `SELECT`
- **Purpose:** Analytics, Reporting

# SQL Query Writing Sequence

1. **SELECT**: Columns we want to see
2. **FROM**: Table names, joins
3. **WHERE**: Rows we want to see/filter
4. **GROUP BY**: Grouping some columns
5. **HAVING**: Filter on groups
6. **ORDER BY**: Sorting
7. **LIMIT**: How many rows we want to see

# Example Queries

## Creating Orders Table
```sql
CREATE TABLE sales.orders (
    order_id serial,
    cust_name varchar,
    amount float
);