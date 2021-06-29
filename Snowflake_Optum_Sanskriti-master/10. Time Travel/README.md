# Time Travel

## SET DATA_RETENTION_TIME_IN_DAYS PROPERTY FOR TIME TRAVEL

### Use following command to create a table

	USE DATABASE DEMO_DB;
	
	USE SCHEMA EMPLOYEE_PERM;
	
	create or replace table employees(employee_id number,
	                     salary number,
	                     manager_id number)
	                   data_retention_time_in_days=90;
	                     
	SHOW TABLES;
                     
	create or replace table employees_test(employee_id number,
	                     salary number,
	                     manager_id number)
	                     data_retention_time_in_days=95;

### Use following command to set the data retention time
                     
	alter table employees set data_retention_time_in_days=30;

If retention period is changed for account or individual objects then retention period will be changed for all lower level objects as well unless explicitly set

### Use following comand to create transient table

	create or replace transient table employees_transient(employee_id number,
	                     salary number,
	                     manager_id number);

### Use following comand to create temporary table
                     
	create or replace temporary table employees_temp(employee_id number,
	                     salary number,
	                     manager_id number);
                     
### Use following comand to alter schema

	ALTER SCHEMA DEMO_DB.EMPLOYEE_PERM set data_retention_time_in_days=55;

	SHOW SCHEMAS;

	SELECT * FROM DEMO_DB.INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'EMPLOYEE_PERM';

## QUERY HISTORICAL DATA

### query selects historical data from a table as of the date and time represented by the specified timestamp:

	select current_timestamp();

	ALTER SESSION SET TIMEZONE = 'UTC';

	select * from employees before(timestamp => '2021-06-28 06:28:45.986 -0700'::timestamp);

Use the timestamp obtained by running the first command

### query selects historical data from a table as of 5 minutes ago:

	select * from employees at(offset => -60*7);

## query selects historical data from a table up to, but not including any changes made by the specified statement:

	select * from employees before(statement => '019d3ae9-0000-1534-0001-262200016242');

## CLONE HISTORICAL OBJECTS

### create a duplicate of the object at a specified point in the object’s history

	select current_timestamp();

### following CREATE TABLE command creates a clone of a table as of the date and time represented by the specified timestamp:

	create table restored_table clone employees
	  at(timestamp => '2021-06-28 13:32:17.691 +0000'::timestamp);
  
### following CREATE SCHEMA command creates a clone of a schema and all its objects as they existed 1 hour before the current time:

	create schema restored_schema clone employees_perm at(offset => -600);

### following CREATE DATABASE command creates a clone of a database and all its objects as they existed prior to the completion of the specified statement:

	create database restored_db clone demo_db
	  before(statement => '019d3ae9-0000-1534-0001-26220001623e');
