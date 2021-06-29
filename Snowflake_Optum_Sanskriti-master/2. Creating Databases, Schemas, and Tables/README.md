# Create virtual warehouse

Warehouse sizes are similar to T-SHIRT sizes from XS to 4X-LARGE
Provide warehouse NAME as it is mandatory 
Below are optional parameters that CREATE WAREHOUSE statement
objectProperties :
WAREHOUSE_SIZE = XSMALL | SMALL | MEDIUM | LARGE | XLARGE | XXLARGE | XXXLARGE | X4LARGE
MAX_CLUSTER_COUNT = <num>
MIN_CLUSTER_COUNT = <num>
SCALING_POLICY = STANDARD | ECONOMY
AUTO_SUSPEND = <num> | NULL
AUTO_RESUME = TRUE | FALSE
INITIALLY_SUSPENDED = TRUE | FALSE
RESOURCE_MONITOR = <monitor_name>
COMMENT = '<string_literal>'*/

## Use the following command to create standard/xs virtual warehouse

     create warehouse test;           

## Use the following command to create a medium sized warehouse

      create warehouse my_wh WAREHOUSE_SIZE = MEDIUM;

## Use the following command to suspend a warehouse

      alter warehouse test suspend;

## if warehouse is in suspended state Use the following command to resume it

      alter warehouse test resume;

## Use the following command to list warehouses

      show warehouses;

## Use the following command to use the warehouse

      use warehouse compute_wh;
      use warehouse test;

# Create Databases, Schema and Table

## Use the following command to create database

      create database company;

      create or replace database company;

## Use the following command to view the database

      show databases like 'c%';

## Use the following command to use the database

      use database demo_db;


## Use the following command to create schema

      create or replace schema employee;
      create schema if not exists department;
      drop schema if exists department;

## Use the following command to show the Schema

      show schemas like 'e%';

## Use the following command to use the Schema

      use schema information_schema;
      use schema public;


## Use the following command to create table

      create table EMPLOYEE.A(id number,
               name varchar(20));

      create or replace table B(id number(32,5),
               salary number,
               department varchar(10));
