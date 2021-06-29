# How to Create Roles and Users and Grant them Priviledges

## Use the following command to create the role

    CREATE OR REPLACE ROLE TEST_ROLE;
    CREATE ROLE IF NOT EXISTS TEST_ROLE;

## Use the following command to grant priviledges to the role

    GRANT ALL PRIVILEGES ON DATABASE DEMO_DB TO ROLE TEST_ROLE;
    GRANT ALL PRIVILEGES ON SCHEMA EMPLOYEE TO ROLE TEST_ROLE;
    GRANT ALL PRIVILEGES ON TABLE A TO ROLE TEST_ROLE;
    GRANT ALL PRIVILEGES ON TABLE EMPLOYEE.A TO ROLE TEST_ROLE;

## Use the following command to use the role

    USE ROLE TEST_ROLE;

## Use the following command to grant role to the User (Username: sanskritikaiwart)

    GRANT ROLE TEST_ROLE TO USER SANSKRITIKAIWART;

## Use the following command to create a new user

    CREATE OR REPLACE USER TEST_USER PASSWORD = 'ABC123' DEFAULT_ROLE = 'PUBLIC' MUST_CHANGE_PASSWORD = TRUE;

## Use the following command to show the grants

    SHOW GRANTS;
