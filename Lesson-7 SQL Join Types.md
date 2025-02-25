# SQL Join Types

## Join Types:
- Inner Join
- Natural Join
- Equi Join
- Theta Join
- Self Join
- Outer Join
  - Left Join
  - Right Join
  - Full Outer Join
- Cross Join

## Natural Join
A natural join automatically joins tables based on columns with the same name and compatible data types.

```sql
SELECT 
--emp.*
dep.*
FROM 
practice.employees emp 
NATURAL JOIN 
practice.department dep;

SELECT 
*
FROM 
practice.employees emp 
NATURAL JOIN 
practice.departments dep;

## Equi Join
An equi join is a specific type of inner join that uses the equality operator to join tables based on matching column values.

SELECT 
*
FROM 
practice.employees e 
INNER JOIN 
practice.departments d 
ON e.manager = d.manager;

SELECT 
*
FROM 
practice.employees e 
INNER JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = d.department_id;

SELECT 
*
FROM 
practice.employees e 
INNER JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = d.department_id::VARCHAR;

SELECT 
*
FROM 
practice.employees e 
INNER JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = d.department_id::character varying;

SELECT 
*
FROM 
practice.employees e 
INNER JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = CAST(d.department_id AS varchar);

SELECT 
*
FROM 
practice.employees e 
JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = CAST(d.department_id AS varchar);

## Theta Join
A theta join allows for comparison operators other than the equality operator, such as <, >, <=, >=, <>, and !=.

SELECT 
*
FROM 
practice.employees e 
JOIN 
practice.departments d
ON e.dep_no <> d.department_id::TEXT;

# Outer Join

Left Join
A left join returns all rows from the left table and the matched rows from the right table. If no match is found, NULL values are returned for columns from the right table.

sql
SELECT 
*
FROM 
practice.employees e 
LEFT OUTER JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;

SELECT 
*
FROM 
practice.employees e 
LEFT JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;
Right Join
A right join returns all rows from the right table and the matched rows from the left table. If no match is found, NULL values are returned for columns from the left table.

sql
SELECT 
*
FROM 
practice.employees e 
RIGHT JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;

SELECT 
*
FROM 
practice.employees e 
RIGHT OUTER JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;
Full Outer Join
A full outer join returns all rows when there is a match in either left or right table. It returns NULL for non-matching rows in either table.

sql
SELECT 
*
FROM 
practice.employees e 
FULL OUTER JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;

SELECT 
*
FROM 
practice.employees e 
FULL JOIN practice.departments d
ON e.dep_no = d.department_id::TEXT;
Cross Join
A cross join returns the Cartesian product of the two tables, i.e., it returns all possible combinations of rows from the two tables.

sql
SELECT 
*
FROM 
practice.employees e 
CROSS JOIN 
practice.departments d;
Self Join
A self join is a regular join, but the table is joined with itself.

sql
SELECT 
* 
FROM 
emp 
JOIN manager 
ON emp.manager = manager.mgr_id;

SELECT 
emp.emp_no,
emp.first_name,
manager.emp_no AS manager_no,
manager.first_name AS manager_first_name
FROM 
practice.employees emp
JOIN practice.employees manager 
ON emp.manager = manager.emp_no;
Example Combining Multiple Joins and Filters
sql
SELECT 
*
FROM 
practice.employees e 
JOIN 
practice.departments d 
ON e.manager = d.manager
AND e.dep_no = d.department_id::VARCHAR
WHERE 
e.salary > 50000;

# Summary

Joins are used to combine rows from two or more tables based on a related column between them. There are different types of joins to suit various needs in querying relational databases.