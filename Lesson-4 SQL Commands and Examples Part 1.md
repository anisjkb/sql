# SQL Commands and Examples

## User Management

### Create User
```sql
CREATE USER anis WITH PASSWORD 'abCd' 
VALID UNTIL '2024-09-30';

###Grant and Revoke Permissions

sql

GRANT USAGE ON SCHEMA sales TO anis;
GRANT SELECT ON TABLE sales.students TO anis;
GRANT SELECT ON TABLE sales.orders TO anis;
REVOKE SELECT ON TABLE sales.orders FROM anis;

###Alter User Validity

sql

ALTER USER anis 
VALID UNTIL '2024-10-01';

ALTER USER anis 
VALID UNTIL 'infinity';

##Table Management

###Alter Table Owner

sql

ALTER TABLE sales.students
OWNER TO anis;

###Create and Drop Table

sql

CREATE TABLE sales.new_tbl (id int, name text);
DROP TABLE sales.new_tbl;

###Create Temporary Table

sql

CREATE TEMPORARY TABLE new_tbl (id int, name text);

###Insert Data into Temporary Table

sql

INSERT INTO new_tbl (id, name)
VALUES 
(64, 'ujyfuyf'),
(68, 'jhfuy564');

SELECT * FROM new_tbl;

##Partitioned Table

###Create Partitioned Table

sql

CREATE TABLE new_tbl (id int, name text, creation_date date)
PARTITION BY RANGE (creation_date);

###Insert Data into Partitioned Table

sql

INSERT INTO new_tbl (id, name, creation_date)
VALUES 
(64, 'ujyfuyf', '2024-09-30'),
(68, 'jhfuy564', '2024-09-28');

CREATE TABLE new_tbl_2024_09_30 PARTITION OF new_tbl DEFAULT;

###Select from Information Schema

sql

SELECT * FROM information_schema.tables;

##Index Management

###Create and Drop Index

sql

CREATE INDEX order_id_idx ON sales.orders (order_id);
DROP INDEX order_id_idx;

##Creating Tables with Constraints

###Create Table with Primary Key

sql

CREATE TABLE sales.new_tbl (id int PRIMARY KEY, name text);

###Create Employees and Departments Tables

sql

CREATE TABLE sales.employees (
    id serial PRIMARY KEY,
    emp_name varchar,
    created_at date
);

CREATE TABLE sales.departments (
    id serial PRIMARY KEY,
    dept_name varchar,
    emp_id int,
    created_at date
);

###Add Foreign Key Constraint

sql

ALTER TABLE sales.departments
ADD CONSTRAINT fk_dept_emp
FOREIGN KEY (emp_id)
REFERENCES sales.departments(id);

###Insert Data into Employees Table

sql

INSERT INTO sales.employees (id, emp_name, created_at)
VALUES 
(646, 'ABCd', CURRENT_DATE);

SELECT * FROM sales.employees;

###Create Table from Select

sql

SELECT emp_name, NOW() AS created_at INTO sales.employees_copy2 FROM sales.employees;

CREATE TABLE sales.created_from_emp 
AS 
SELECT * FROM sales.anisdata_test_csv
LIMIT 1;