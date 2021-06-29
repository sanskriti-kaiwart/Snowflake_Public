# Alter Database, Schema and Tables

## Use the following command to ALTER DATABASE

	ALTER DATABASE IF EXISTS COMPANY RENAME TO PRODUCTION;
	ALTER DATABASE IF EXISTS PRODUCTION RENAME TO COMPANY;

## Use the following command to ALTER SCHEMA

	ALTER SCHEMA IF EXISTS EMPLOYEE RENAME TO EMPL;

	create or replace schema employee;
	create schema if not exists department;
	create or replace table demo_db.employee.empl_details(empl_id number);
	create or replace table demo_db.department.dept_details(dept_id number);

	ALTER SCHEMA IF EXISTS DEMO_DB.DEPARTMENT SWAP WITH DEMO_DB.EMPLOYEE;

	ALTER SCHEMA IF EXISTS DEMO_DB.DEPARTMENT
	SET DATA_RETENTION_TIME_IN_DAYS = 2,
    	COMMENT = 'DEPARTMENT DETAILS SCHEMA';
    
	SELECT * FROM "DEMO_DB"."INFORMATION_SCHEMA"."SCHEMATA"
	WHERE SCHEMA_NAME = 'DEPARTMENT';
    
## Use the following command to view schemas

	show schemas;
    
## Use the following command to ALTER TABLE

	ALTER TABLE IF EXISTS DEPARTMENT.DEPT_DETAILS RENAME TO DEPARTMENT_DETAILS;

	ALTER TABLE IF EXISTS EMPLOYEE.DEPARTMENT_DETAILS RENAME TO DEPARTMENT.DEPARTMENT_DETAILS;

	create or replace table demo_db.department.department_employees(dept_id number,
                                                               empl_name varchar(40));
                                                               
	ALTER TABLE IF EXISTS DEMO_DB.DEPARTMENT.DEPARTMENT_DETAILS SWAP WITH DEMO_DB.DEPARTMENT.DEPARTMENT_EMPLOYEES;

# Insert, Update and delete in Table

### INSERT – is used to insert data into a table.
### UPDATE – is used to update existing data within a table.
### DELETE – is used to delete records from a table.

## Use the following command to create tables

	create or replace table A(id number,
               name varchar(20));
               
	create or replace table B(id number,
               name varchar(20));            
               
	select * from A;
	select * from B;

## Use the following command to insert in tables

	insert into A values(1,'Aman'),
        	         (2,'Bhavesh'),
                    	(3,'Carolyn'),
                    	(5,'David');
                    
	insert into B values(4,'Fulos'),
                    (9,'Bhavesh'),
                    (1,'Shanaya'),
                    (2,'Bhavesh'),
                    (10,'Pawan');
                    
	insert into B (select * from A);

	select * from B;

## Use the following command to add SUBQUERY in DELETE statement

	delete from B 
	where name = (select 
            name from A 
            where name = 'Bhavesh');
              
	select * from B;

## Use the following command to add SUBQUERY in UPDATE statement

	insert into B values(4,'Fulos'),
                    (9,'Bhavesh'),
                    (1,'Shanaya'),
                    (2,'Bhavesh'),
                    (10,'Pawan');
                    
	select * from B;

	update B
	set name = 'Rohit'
	where name in (select distinct name
              from A
              where name like 'B%');
              
	select * from B;

	insert into B values(1,NULL);

	update B
	set name = 'SANSKRITI'
	where name is NULL;

