--SQL SYNTAX IS NOT CASE INSENSITIVE
--"" --> BIND

drop table if exists practice.employee;
drop table if exists practice.employees;


create table 
practice.employees
(
emp_no serial not null, --not null CONSTRAINT
birth_date Date,
first_name varchar,
last_name varchar,
gender varchar,
hire_date DATE,
primary KEY(emp_no) --DUNPLICATE not ALLOWED, NULL not ALLOWED
);

alter table practice.employees
add manager int;

truncate practice.employees ;

delete from practice.employees 
where gender not in ('M','F');

alter table practice.employees
add constraint emp_check_gender
CHECK(gender in ('M','F'));


create schema stage;

select 
distinct 
emp.gender 
from 
stage.employees as emp ;

update 
stage.employees emps
set gender  = 'F'
where emps.gender = 'Female';

insert into 
practice.employees
select 
* from 
stage.employees e 
--limit 5;



/*
''
0
NULL
*/

--CTRL + SHIFT +/
--CTRL+/

--drop table 
--practice.employees

create table 
if not EXISTS
practice.employees
(
emp_no serial not null, --not null CONSTRAINT
birth_date Date,
first_name varchar,
last_name varchar,
gender varchar check(gender ='F'),
hire_date DATE,
nid bigint ,
primary KEY(emp_no) --DUNPLICATE not ALLOWED, NULL not ALLOWED
unique (nid)
);

select 
* from 
practice.employees e ;


update 
practice.employees e  
set dep = NULL
where e.emp_no =10001;

alter table 
practice.employees 
add constraint emp_dep_fk
foreign key(dep)
references practice.department(dept_no)
;

-- there is no unique constraint matching given keys for referenced table "department"
 
 alter table practice.department 
 add constraint dep_pk
 primary key (dept_no);
 
update 
practice.employees e  
set dep = 'd7987'
where e.emp_no =10001;

SQL Error [23503]: ERROR: insert or update on table "employees" violates 
foreign key constraint "emp_dep_fk"
  Detail: Key (dep)=(d7987) is not present in table "department".

--DATA TYPE MUST BE SAME
  --DATA SHOULD EXIST IN MAIN?REFERRED TABLE
 
  --DDL 
--  	created_at 
--  	is_deleted
  	
--alter table 
--practice.department 
--add created_at timestamp

alter table 
practice.department 
add is_deleted bool 
default FALSE;
--T/F

alter table practice.department 
drop created_at;


alter table 
practice.department 
add created_at timestamp
default now();

update 
practice.department d 
set is_deleted=true 
where d.dept_name ='Research';

select 
* 
from 
practice.department d 
where is_deleted = false;