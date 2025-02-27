## Data Definition Language (DDL)

DDL is a subset of SQL commands used to define, modify, and manage the structure of database objects. The primary purpose of DDL is to create and organize the schema (structure) of the database, which includes tables, indexes, views, schemas, and other database objects.

Here are some common DDL commands:

### CREATE: This command is used to create new database objects such as tables, indexes, and views.

sql

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE
);
### ALTER: This command is used to modify the structure of existing database objects. For example, adding or dropping columns in a table.

sql

ALTER TABLE employees ADD email VARCHAR(100);

### DROP: This command is used to delete existing database objects. Use it with caution, as it permanently removes the object and all data within it.


sql

DROP TABLE employees;
TRUNCATE: This command removes all rows from a table but retains the table structure for future use.

sql

TRUNCATE TABLE employees;
RENAME: This command is used to rename existing database objects.

sql

ALTER TABLE employees RENAME TO staff;

##  Key Points

###  Non-DML: Unlike DML (Data Manipulation Language), which deals with data manipulation (e.g., INSERT, UPDATE, DELETE), DDL focuses on defining and modifying the structure of database objects.

### RENAME: Used to rename existing database objects like tables, columns, indexes, etc.

sql

ALTER TABLE employees RENAME TO staff;

### COMMENT: Used to add comments to database objects for better documentation.

sql

COMMENT ON TABLE employees IS 'Table storing employee data';

### Autocommit: DDL commands are autocommitted, meaning they take effect immediately and cannot be rolled back.

DDL is essential for setting up and managing the overall structure of a database. By defining tables, indexes, and other objects, it ensures that the database is well-organized and can efficiently store and retrieve data.

## Summary of DDL Commands

Here is a quick summary of the DDL commands and their primary uses:

| Command  | Description                                                         |
|----------|---------------------------------------------------------------------|
| CREATE   | Create new database objects (tables, indexes, views, schemas, etc.) |
| ALTER    | Modify existing database objects (add/drop columns, change data types, etc.) |
| DROP     | Delete existing database objects (tables, views, indexes, schemas, etc.) |
| TRUNCATE | Remove all rows from a table but retain its structure               |
| RENAME   | Rename existing database objects (tables, columns, indexes, etc.)   |
| COMMENT  | Add comments to database objects                                    |