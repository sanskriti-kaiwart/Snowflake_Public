# Source and Destination setup

## Create the table

### Use the following Command to create the table
	CREATE DATABASE FINANCE;

	USE DATABASE FINANCE;

	CREATE SCHEMA STAGE;

	USE SCHEMA STAGE;

	CREATE TABLE FINANCE.STAGE.BANK_TRANSACTIONS
	(
		TransactionNo INT,
		DateTime DATETIME,
		FromAccount INT,
		ToAccount INT,
		Amount NUMBER,
		TypeOfTransaction VARCHAR(20),
		TranDescription VARCHAR(200),
		Source VARCHAR(100)
	);

## Azure Setup

### Goto Azure, Add your Resource group
### Add you Storage account resource
### Goto resource ( refer previous folder)
### Add your container (banking-data-blob) and load your file in it (BankData1, BankData2, BankData3, BankData4, Bankdata5, add some after creating pipe)
### Go back to Storage account (snowflakesnowpipe1234)
### goto queues and add the queue and give it a name (banking-data-queue)
### Go back to Storage account
### Go to Containers
### Navigate to events
### Click on event subscription and create an event
### Name your event
### Select your Topic Name
### Select End point
### Select your subscription
### Select your Storage account
### Select your Storage Queue
### Click on Create

## Create Notification Integration

### Use the following command to create the notification integration

	CREATE  NOTIFICATION INTEGRATION BANKING_SNOWPIPE_EVENT_NEW
	ENABLED =TRUE
	TYPE=QUEUE
	NOTIFICATION_PROVIDER=AZURE_STORAGE_QUEUE
	AZURE_STORAGE_QUEUE_PRIMARY_URI='https://snowflakesnowpipe1234.queue.core.windows.net/banking-data-queue'
	AZURE_TENANT_ID='42969e25-36f3-4b8c-930f-0df9751a8ce4';

### Paste your storage queue primary uri, for that go to your azure storage account and then to your queues to get the URL
### Paste your tenant id, for that go to your azure active directory and copy your tenant id

## Describe the notification integration and provide access

### Use the following command to see the description
	desc NOTIFICATION INTEGRATION BANKING_SNOWPIPE_EVENT_NEW;
### Copy the AZURE_CONSENT_URL and paste it on your browser
### tick on "consent on behalf of my organization" and click accept.
### Go to Azure Storage account
### Go to acccess controle (IAM)
### Go to Role assignment, add role assignment, select role "Storage Queue Data Contributor", Select your Snowflake application and save

## Create Stage

### Use the following command to create stage
	CREATE OR REPLACE STAGE BANK_TRANSACTIONS_STAGE
	url = 'azure://snowflakesnowpipe1234.blob.core.windows.net/banking-data-blob/'
	credentials = (azure_sas_token=
	'?sv=2020-02-10&ss=bfqt&srt=co&sp=rwlacx&se=2021-06-17T20:26:43Z&st=2021-06-17T12:26:43Z&spr=https&sig=3z2c2Sa23Q%2FfCFLo0fR78yFNQsqe1Qag1tIoVBlh1EQ%3D
	');
### Enter your storage blob service end point in url, for that goto your storage account, go to endpoints, copy "primary blob service endpoint" and paste the URL (take care of the format, replace http from azure keyword) 
### Enter your Blob Service Shared Access Signature (SAS) in credentials, for that go to Shared access signature, set expiry date, click on "generate SAS and connection string", copy Blob Service SAS URL and paste ( take care of the format, from "?" onwards)
### Use following command to preview the data available in staging area
	ls @BANK_TRANSACTIONS_STAGE;

## Create Pipe

### Use the following command to create the pipe
	CREATE OR REPLACE pipe "BANK_TRANSACTIONS_PIPE"
  	auto_ingest = true
  	integration = 'BANKING_SNOWPIPE_EVENT_NEW'
  	as
  	copy into FINANCE.STAGE.BANK_TRANSACTIONS
	(TransactionNo,DateTime,FromAccount,ToAccount,Amount,TypeOfTransaction,TranDescription,Source)
	 from (SELECT $1,TO_DATE($2,'MM/DD/YYYY HH:MI AM'),$3,$4,$5,$6,$7,$8  FROM @BANK_TRANSACTIONS_STAGE )
	  file_format = (type = 'CSV');

### Use the following command to activate the pipe
	ALTER PIPE BANK_TRANSACTIONS_PIPE REFRESH;

### Use the following command to preveiw your data
	select * from "FINANCE"."STAGE"."BANK_TRANSACTIONS"

your data has been loaded into snowflake using snowpipe.
