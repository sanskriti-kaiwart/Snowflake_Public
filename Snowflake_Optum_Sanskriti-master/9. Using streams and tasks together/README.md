# Create Streams

## Use the following command Create a stream to track changes to data in the COVID_VACCINE_MANUFACTURER_DEMO table

	create or replace stream covid_stream on table "COVID_VACCINATION"."PUBLIC"."COVID_VACCINE_MANUFACTURER_DEMO";

## Use the following command to reveiw the stream

	SHOW STREAMS;

## Use the following command to see the stream description

	DESCRIBE STREAM covid_stream;

## Use the following command to access the time stamp

	SELECT SYSTEM$STREAM_GET_TABLE_TIMESTAMP('covid_stream');

## Use the following command to convert the time stamp

	SELECT to_timestamp(SYSTEM$STREAM_GET_TABLE_TIMESTAMP('covid_stream')) as stream_offset;

## Use the following command to insert values in employees table

	insert into "COVID_VACCINATION"."PUBLIC"."COVID_VACCINE_MANUFACTURER_DEMO" values('India', 2021-01-08,'Covid', '240');
                                 
## Use the following command to reveiw the stream records

	select * from covid_stream;


## Use the following command to consume the stream in new table COVID_VACCINE_MANUFACTURER_DEMO_CONCUME

	create or replace table COVID_VACCINE_MANUFACTURER_DEMO_CONCUME (
	  "LOCATION" STRING, 
	  "VACCINE" STRING );
                     
	insert into COVID_VACCINE_MANUFACTURER_DEMO_CONCUME select location, vaccine from covid_stream;

## Use the following command to reveiw the target table

	select * from COVID_VACCINE_MANUFACTURER_DEMO_CONCUME;


# Create Tasks

## Use the following command to create a task

	create or replace task covid_vaccination.public.covid_task
	    warehouse = compute_wh 
	    schedule  = '1 minute'
	  when
	    system$stream_has_data('covid_vaccination.public.covid_stream')
	  as
	    insert into COVID_VACCINE_MANUFACTURER_DEMO_CONCUME select location, vaccine from covid_stream;
    
## Use the following command fo Description of Task

	desc task covid_vaccination.public.covid_task;

## Use the following command to Resuem The Task

	alter task covid_vaccination.public.covid_task resume;

## Use the following command to do change in Data

	insert into "COVID_VACCINATION"."PUBLIC"."COVID_VACCINE_MANUFACTURER_DEMO" values('China', 2021-01-08,'Covin Sheild', '430');

## Use the following command to Check the stream

	select * from covid_stream;

after a few moments, the changes in source table will automatically reflect in target table

## Use the following command to Suspend the task

	alter task covid_vaccination.public.covid_task suspend;
