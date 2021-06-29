# Create the Table, File Format and Stage

## Use the following command to Create the database
	CREATE DATABASE COVID_VACCINATION;

## Use the following command to Create table according to your dataset
	CREATE TABLE "COVID_VACCINATION"."PUBLIC"."COVID_VACCINE_MANUFACTURER_DEMO" 
	(	"LOCATION" STRING, 
		"DATE" STRING, 
		"VACCINE" STRING, 
		"Total Vaccinated" INTEGER);

## Use the following command to Create your file format (CSV in this case)
	CREATE OR REPLACE FILE FORMAT COVID_VACCINATION.PUBLIC.CSV
	TYPE = CSV FIELD_DELIMITER = ',' SKIP_HEADER = 1;

## Use the following command to Create stage object

	create or replace stage COVID_VACCINATION.PUBLIC.MYSTAGE
  	file_format = COVID_VACCINATION.PUBLIC.CSV;

# Instructions to follow for loading the data

### Go to Databases on top left panel in Snowflake interface.
### Click on the hyperlink of your databas (COVID_VACCINATION)
### Click on the hyperlink of your table (COVID_VACCINE_MANUFACTURER_DEMO)
### Click on Load Table option
### Select your Warehouse (COMPUTE_WH), and click next
### Click on Select Files and browse to the file in your system, and click next
### Select your file format (CSV) and then click load

your data has been loaded in the table.
