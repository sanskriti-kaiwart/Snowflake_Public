# Create Table and Task

## Use following command to create Table

	USE DATABASE "DEMO_DB";
	CREATE TABLE EMPLOYEES(EMPLOYEE_ID INTEGER AUTOINCREMENT START = 1 INCREMENT = 1,
	                       EMPLOYEE_NAME VARCHAR DEFAULT 'SANSKRITI',
	                       LOAD_TIME DATE);

## Use following command to create task

	CREATE OR REPLACE TASK EMPLOYEES_TASK
	  WAREHOUSE = COMPUTE_WH
	  SCHEDULE = '1 MINUTE'
	AS
	 INSERT INTO EMPLOYEES(LOAD_TIME) VALUES(CURRENT_TIMESTAMP);

the task here adds a row in interval of every 1 minute.

## Use following command to reveiw task 

	SHOW TASKS;

## Use following comand to resume the task

	ALTER TASK EMPLOYEES_TASK RESUME;

## Use following command to suspend the task

	ALTER TASK EMPLOYEES_TASK SUSPEND;
