## SQL Scripts

### Create Table Sales
```sql
insert into schema.table
--(columns)
--values ()

insert into sales.students 
(name,students_id)
values 
('abcd',64564),
('Mr Abul',65456);

insert into sales.students 
values 
(645,'uyf'),
(6545,'Mr bablu');

## DDL Operations

```sql

create table practice.departments
(department_id serial,
department_name varchar,
manager_id int,
location_id int,
is_active bool);

alter table practice.departments 
add created_at 
timestamp 
--null 
default now();

create table practice.departments_copy 
as 
select
manager_id, 
department_name 
from practice.departments 
limit 0;

## Insert Data

```sql

insert into practice.departments_copy 
select
manager_id, 
department_name 
from practice.departments limit 1;

select
manager_id, 
department_name 
into practice.departments_copy 
from practice.departments limit 2;

insert into practice.departments_copy 
select
manager_id, 
department_name 
from practice.departments limit 2;

insert into practice.departments_copy 
select 
99999,'MY DEPARTMENT';

insert into practice.departments_copy 
VALUES
(88888,'MY DEPARTMENT2');

## Delete Data

```sql

delete 
from 
practice.departments_copy ;

delete 
from 
practice.departments_copy 
where manager_id =99999;

delete 
from 
practice.departments_copy 
where manager_id =200;

delete 
from 
practice.departments_copy 
where TRUE;

truncate table 
practice.departments_copy ;

delete 
from 
practice.departments_copy 
where manager_id =99999;

insert into 
practice.departments_copy 
select * from backup_removed_robiul.departments_copy_manager_id_99999_20230901_10_2pm ;

## Update Data

update 
practice.departments_copy 
set department_name='DEPT999'
where  manager_id=99999;

update 
practice.departments_copy 
set 
department_name='DEPTNEW'
,manager_id =878787
where  manager_id=99999;

## Select and Insert Data

select * 
into practice.new_dept 
from practice.departments 
limit 5;

delete 
from 
practice.departments_copy 
where manager_id =99999;

## Aliasing

select 
department_id as ID, department_name as name 
from 
practice.departments;

select 
dept.department_id, dept.department_name 
from 
practice.departments AS dept;

select 
dept.department_id, dept.department_name 
from 
practice.departments dept;

select 
department_id  as id, department_name  name 
from 
practice.departments dept;

## Complex Operations

delete from 
practice.departments dpt  
USING 
practice.new_dept as new_dpt  
where dpt.department_id = new_dpt.department_id 
and new_dpt.department_id >40;

update 
practice.departments dpt  
set department_id = dpt.department_id+1000
from 
practice.new_dept newd
where 
newd.department_id <> dpt.department_id 
and newd.department_id <40;

update 
practice.departments dpt  
set department_id = dpt.department_id+99
from 
(select newd.department_id
from practice.new_dept newd
where 
newd.department_id <40) newdpt
where newdpt.department_id= dpt.department_id ;



