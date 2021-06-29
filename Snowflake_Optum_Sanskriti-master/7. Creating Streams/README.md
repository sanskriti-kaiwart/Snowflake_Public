# Create Database, Tables and Streams

## Use the following command to create the database and the table

	CREATE DATABASE "LIBRARY_CARD_CATALOG";
	USE DATABASE "LIBRARY_CARD_CATALOG";

	create or replace table employees(employee_id number,
	                    salary number,
	                     manager_id number);

# Create a stream to track changes to data in the EMPLOYEES table

## Use the following command to create a stream

	create or replace stream employees_stream on table employees;

## Use the following command to view the streams

	SHOW STREAMS;

## Use the following command to see the description of the stream

	DESCRIBE STREAM employees_stream;

## Use the following command to get the time at which the streams activated

	SELECT SYSTEM$STREAM_GET_TABLE_TIMESTAMP('employees_stream');

## Use the following command to convert the timestamp

	SELECT to_timestamp(SYSTEM$STREAM_GET_TABLE_TIMESTAMP('employees_stream')) as stream_offset;

## Use the following command to insert values in employees table

	insert into employees values(8,40000,4),
                                 (12,50000,9),
                                 (3,30000,5),
                                 (4,10000,5),
                                 (25,35000,9);
                                 
## Use the following command to the stream records

	select * from employees_stream;


## Use the following command to Consume the stream

	create or replace table employees_consumer(employee_id number,
	                     salary number);
                     
	insert into employees_consumer select employee_id, salary from employees_stream;

## Use the following command to reveiw the target table
	select * from employees_consumer;
