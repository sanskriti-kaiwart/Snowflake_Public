# Create Azure Storage Account

## Go to portal.azure.com

## Go to Create a Resource

## Search for Storage Account

## Create Your Storage Account

### Select your Subscription
### Name your Resource Group
### Name your unique Storage account name
### Select your region
### select your account kind (v2)
### select replication factor ( here LRS)
### Enable Hierarchical namespace
### Reveiw and Create
### click on goto resouce
### click on containers
### create a container

# Load data on Azure data lake

### go to your Azure account
### go to home and click on your storage account (snowflakeazuredemo1234)
### go to your container (snowflake-demo and snowflake-demo-parquet)
### click on upload, and select your files from the system to be uploaded (Health_pipe.csv and Health.parquet)

# Create Integration and stage objects

## Create Your Table

### Use the following command to CREATE TABLE TO LOAD CSV DATA
	CREATE OR REPLACE TABLE HEALTHCARE_CSV(
    	AVERAGE_COVERED_CHARGES    NUMBER(38,6)  
   	,AVERAGE_TOTAL_PAYMENTS    NUMBER(38,6)  
   	,TOTAL_DISCHARGES    NUMBER(38,0)  
   	,BACHELORORHIGHER    NUMBER(38,1)  
   	,HSGRADORHIGHER    NUMBER(38,1)  
   	,TOTALPAYMENTS    VARCHAR(128)  
   	,REIMBURSEMENT    VARCHAR(128)  
   	,TOTAL_COVERED_CHARGES    VARCHAR(128) 
   	,REFERRALREGION_PROVIDER_NAME    VARCHAR(256)  
   	,REIMBURSEMENTPERCENTAGE    NUMBER(38,9)  
   	,DRG_DEFINITION    VARCHAR(256)  
   	,REFERRAL_REGION    VARCHAR(26)  
   	,INCOME_PER_CAPITA    NUMBER(38,0)  
   	,MEDIAN_EARNINGSBACHELORS    NUMBER(38,0)  
   	,MEDIAN_EARNINGS_GRADUATE    NUMBER(38,0)  
   	,MEDIAN_EARNINGS_HS_GRAD    NUMBER(38,0)  
   	,MEDIAN_EARNINGSLESS_THAN_HS    NUMBER(38,0)  
   	,MEDIAN_FAMILY_INCOME    NUMBER(38,0)  
   	,NUMBER_OF_RECORDS    NUMBER(38,0)  
   	,POP_25_OVER    NUMBER(38,0)  
   	,PROVIDER_CITY    VARCHAR(128)  
   	,PROVIDER_ID    NUMBER(38,0)  
   	,PROVIDER_NAME    VARCHAR(256)  
   	,PROVIDER_STATE    VARCHAR(128)  
   	,PROVIDER_STREET_ADDRESS    VARCHAR(256)  
   	,PROVIDER_ZIP_CODE    NUMBER(38,0)  
	);

## Create Notification Integration

### Use following command to create notification integration
	CREATE STORAGE INTEGRATION AZURE_INT
  	TYPE = EXTERNAL_STAGE
  	STORAGE_PROVIDER = AZURE
  	ENABLED = TRUE
  	AZURE_TENANT_ID = '42969e25-36f3-4b8c-930f-0df9751a8ce4'
	STORAGE_ALLOWED_LOCATIONS = ('azure://snowflakeazuredemo1234.blob.core.windows.net/snowflake-demo/', 'azure://snowflakeazuredemo1234.blob.core.windows.net/snowflake-demo-parquet/');
### Name your Storage Integration (AZURE_INT)
### Enter your Azure tenant Id, to get it go to your azure account, clivk on Azure Active Directory, and copy your tenant id
### Enter your Storage location, use the above syntax, paste your storage account name (snowflakeazuredemo1234) and your container name (snowflake-demo and snowflake-demo-parquet)
### Run the following Description command
	DESC STORAGE INTEGRATION AZURE_INT;
### you will get seven roles, go to AZURE_CONSENT_URL, copy the link and open in browser.
### Tick on consent on behalf of your organization and click accept
### Go back to snowflake, and note the AZURE_MULTI_TENANT_APP_NAME
### Go back to Azure
### Go to Access Control (IAM)
### Click on Role assignments
### click add, Select role of "Storage blob data contributor", Select your AZURE_MULTI_TENANT_APP_NAME and click save.
### Goto Snowflake, and create your file format, use the following command.
	CREATE OR REPLACE FILE FORMAT DEMO_DB.PUBLIC.MY_CSV_FORMAT
	TYPE = CSV FIELD_DELIMITER = '|' SKIP_HEADER = 1;
### Create your stage using the following command
	CREATE OR REPLACE STAGE DEMO_DB.PUBLIC.MY_AZURE_STAGE
	STORAGE_INTEGRATION = AZURE_INT
	URL = 'azure://snowflakeazuredemo1234.blob.core.windows.net/snowflake-demo/health_pipe.csv'
	FILE_FORMAT = MY_CSV_FORMAT;

## Load the CSV file from storage to table

### Use the following command to check the files in stage area
	SELECT $1
	FROM @DEMO_DB.PUBLIC.MY_AZURE_STAGE;

### Use the copy command to load the data
	COPY INTO DEMO_DB.PUBLIC.HEALTHCARE_CSV
	FROM @DEMO_DB.PUBLIC.MY_AZURE_STAGE;
	select * from healthcare_csv; 

your data has been loaded from your azure container to your table.

