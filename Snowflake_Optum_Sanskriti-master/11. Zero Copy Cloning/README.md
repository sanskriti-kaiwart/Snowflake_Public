# Clone Database, Pipes, Streams, Tasks, and Stages

## Creating temporary table to show that cloning just works for Permanent & Transient tables and not temporary

	create or replace TEMPORARY table EMPLOYEES_TEMP(employee_id number,
	                     empl_join_date date,
	                     dept varchar(10),
	                     salary number,
	                     manager_id number);
                                          
	insert into EMPLOYEES_TEMP values(8,'2014-10-01','HR',40000,4),
	                                 (12,'2014-09-01','Tech',50000,9),
	                                 (3,'2018-09-01','Marketing',30000,5),
	                                 (4,'2017-09-01','HR',10000,5),
	                                 (25,'2019-09-01','HR',35000,9),
	                                 (12,'2014-09-01','Tech',50000,9),
	                                 (86,'2015-09-01','Tech',90000,4),
	                                 (73,'2016-09-01','Marketing',20000,1);


	update employees set employee_id = 73 where employee_id = 37;

## Use following commands to list stages, pipes, streams, and tasks

	show stages;
	list @EXT_CSV_STAGE;
	
	show pipes;

	show streams;
	select * from EMPLOYEES_STREAM;

	show tasks;


## Use following ci=ommands to Clone a database and all objects within the database at its current state:

	create or replace database demo_db_clone clone demo_db;

	use demo_db_clone;

## Clone a schema and all objects within the schema at its current state:

	create schema employee_perm_clone clone employee_perm;

## Clone a table at its current state:

	create table employees_clone clone demo_db.public.employees;

	create table employees_transient_clone clone demo_db.employee_perm.employees_transient;

	create transient table employees_transient_clone clone demo_db.employee_perm.employees_transient;

	create table employees_temp_clone clone demo_db.employee_perm.employees_temp;

	create temporary table employees_temp_clone clone demo_db.employee_perm.employees_temp;

	create transient table employees_temp_clone_trans clone demo_db.employee_perm.employees_temp;
