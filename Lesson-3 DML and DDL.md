## Data Manipulation Language (DML)

DML is a subset of SQL commands used to interact with the data within database objects. These commands allow you to manipulate the data stored in tables. Some common DML commands include:

### INSERT

Used to add new rows to a table.

sql

INSERT INTO employees (employee_id, first_name, last_name, hire_date) 
VALUES (1, 'John', 'Doe', '2025-02-22');

### UPDATE

Used to modify existing data in a table.

sql

UPDATE employees 
SET email = 'john.doe@example.com' 
WHERE employee_id = 1;

### DELETE

Used to remove rows from a table.

sql

DELETE FROM employees 
WHERE employee_id = 1;

### SELECT

Used to query and retrieve data from a table.

sql

SELECT * FROM employees;

## Data Definition Language (DDL)

DDL is a subset of SQL commands used to define and manage the structure of database objects, like tables, indexes, and views. DDL commands include:

### CREATE

Creates new database objects such as tables, indexes, and views.

sql

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE
);

### ALTER

Modifies the structure of existing database objects.

sql

ALTER TABLE employees ADD email VARCHAR(100);

### DROP

Deletes existing database objects.

sql

DROP TABLE employees;

### TRUNCATE

Removes all rows from a table but retains its structure.

sql

TRUNCATE TABLE employees;

### RENAME

Renames existing database objects.

sql

ALTER TABLE employees RENAME TO staff;

### COMMENT

Adds comments to database objects.

sql

COMMENT ON TABLE employees IS 'Table storing employee data';