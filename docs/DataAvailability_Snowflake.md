# Data Availability via Snowflake

## Overview
Snowflake is a cloud-based data warehouse that pulls in data from the various Velocity microservice databases. The Data Availability Team (DAT) uses a software called DBT as a tool to create different "views" within Snowflake. Each view in Snowflake is a SQL query that has been structured by the DAT to display various data elements in specific ways that are helpful for users when analyzing data. 

Currently, views within Snowflake display data from the DBT_VELOCITY_DB database. However, there are other databases that exist outside of Velocity that are maintained by other teams (such as Analytics, Ops Analysts, etc.). There are different types of Snowflake views and each type has associated characteristics, including how often the underlying tables are refreshed with new data. 

The Data Dictionary is a comprehensive database of the various data elements (also known as "fields") that live in Velocity and Snowflake with a description of each data element. The Data Dictionary currently exists as an Excel spreadsheet, but efforts are being made to move the Data Dictionary onto the DBT platform for various quality of life reasons, with the main benefit being the ability to access the Data Dictionary field descriptions within Snowflake directly.

## What is Snowflake?
Snowflake is a cloud-based data warehouse that imports data from a collection of databases that exist in PostgreSQL (Postgres) using AWS Glue and a new tool called Rivery. The Velocity platform is comprised of various microservices that all communicate using various APIs and each microservice contains a collection of tables (or small databases). Velocity has approximately 60 different microservices and corresponding databases in Postgres, which Snowflake can pull from to query different data and then present different data based on the queries.

Additionally, Snowflake currently serves as a data warehouse for entities other than just Velocity, meaning it contains a lot of other data that is not maintained specifically by the DAT. If users wish to find data or information about the other areas and tables within Snowflake, they will need to contact the owner of the specific space.

Velocity launched with Access Loans in 2019 and Snowflake became the preferred data warehouse for Velocity in 2021. Due to this timeline, there is approximately 1.5 years worth of data that exists in Velocity that does not exist in Snowflake at this time. There is an old RDS environment (Redshift Postgres) that acted as a temporary solution prior to adopting Snowflake that is still active and houses this older data as well.

The earliest records in Snowflake have a timestamp of around March of 2021.

## What is DBT?
DBT is the software that the Data Availability Team uses as a development environment. Each view that is created by the DAT, and then used by Nelnet users in Snowflake, is developed in DBT. DBT interfaces with Github, allowing the team to use all of the quality of life features that Github has to offer. 

DBT also acts as a data transformation tool, allowing users to take source data and model it in ways that materialize different views and tables. So, while the DAT uses it primarily for creating and developing different views, its functionality goes beyond simply creating different views.

Additionally, DBT serves as the central location for all of the Data Availability Team's column-level documentation. This documentation is currently being added as the DAT touches each column and/or works on new tickets and projects.

To learn more about DBT, you can visit the DBT website.

## What is the Data Dictionary?
The Data Dictionary is a project with the primary goal of providing helpful descriptions for all of the various data elements that exist within Velocity today. For example, if a user were to query one of the Velocity APIs and was returned a JSON payload full of data, the Data Dictionary could be used to look up the different data element names and their descriptions. 

While the Data Dictionary currently exists in Excel spreadsheets, there are efforts underway to move the existing data element descriptions into a DBT repository. Doing this would centralize the Data Dictionary for all users, allow data element descriptions to be updated in a live database, allow Snowflake users to access the Data Dictionary within Snowflake, and allow DBT users to see the relationships between data elements and the different views they are used in (otherwise known as their lineage).


## Read-only vs. full access users
For security purposes (as well as financial purposes), there are two instances of Snowflake that a user can log in to: 

* Read-only
* Full access
The role and permissions given to each user will determine which instance of Snowflake they will primarily be using.

The URLs for each instance are listed here:

Read-only: https://xb45703.snowflakecomputing.com/
Full access: https://app.snowflake.com/us-west-2/jya94865/
The main difference between the two databases is that the full access database makes more tables visible to the user.

Full-access users are more expensive to add to a license, meaning most users will be given read-only access.

Snowflake views and table refresh times
As Snowflake is a data warehouse containing the collection of Velocity microservice databases, it provides a powerful tool in being able to query data across the entire Velocity platform and present the data in easy-to-read views.

One caveat with Snowflake views is that not all data in Velocity is available in Snowflake. This is often because old data elements never got added to views or because newly developed data elements have yet to be added to views.

Currently, views within Snowflake display data from the DBT_VELOCITY_DB database; however, there are other databases that exist outside of Velocity that might have information that is unrelated to Velocity, or is Velocity-related, but maintained by other teams (such as Analytics, Ops Analysts, etc.).

The views in Snowflake are non-materialized, meaning each view will re-run the SQL query that generates it and display the current data when called. Creating Snowflake views is necessary because people need to see the various pieces of data without having access to the actual tables where the data is housed.

The different types of views currently available in Snowflake for the DBT_VELOCITY_DB database are:

Please reference the Snowflake Table Documentation page if you are looking for the most up-to-date refresh times for specific tables in the DBT_VELOCITY_DB database.

The DAT is currently working on moving all table refresh times to a single refresh that will occur at 7:00a UTC using Rivery, but until they are finished, please use the resources here for reference.

This information is specific to the PROD instance of Snowflake (XB45703).




(VW) View	
Financial data updates as new data comes in from Velocity

Non-financial data updates during the scheduled table refresh times.

Starts: 5:00a UTC / 11:00p CST

Ends: 11:20a UTC / 5:20a CST

Views that have "VW" in their name represent queries that are pulling data in near real-time. The data in these views are updated as the data in Velocity changes. 

Non-financial related data changes (ie. address changes, phone number changes, e-correspondence/SCRA status changes, etc.) do not update automatically when changes to those pieces of data are made in Velocity. Changes to these pieces of information will only update in the Snowflake views during the refresh cycle each morning. 

(DS) Daily snapshot	
Starts: 5:00a UTC / 11:00p CST

Ends: 11:20a UTC / 5:20a CST

Views that have "DS" in their name represent queries that are snapshotting data elements at a scheduled time, meaning the information is going to be accurate as of the time it was last queried. These views are built from AWS Glue (and Rivery) tables brought over nightly.
(RP) Reports	
Starts: 5:00a UTC / 11:00p CST

Ends: 11:20a UTC / 5:20a CST

Views that have "RP" in their name are reporting views based off of "VW" and "DS" views.
(CRP) Citizens reports	
Starts: 5:00a UTC / 11:00p CST

Ends: 11:20a UTC / 5:20a CST

Views that have "CRP" in their name are the same as "RP" views, but are specific to Citizens.
Within DBT_VELOCITY_DB there are two types of tables:

One that refreshes in near real time as new data comes into Velocity (this is a singular table, but is very broad)
All other tables that are refreshed on a schedule (starting at 11:00p CST and refreshing every so often until 5:20a CST)
Touch events
One caveat of the table that refreshes as new data comes into Velocity (near real time) is that non-financial related data changes (that is, address changes, phone number changes, boolean status changes, etc.) do not update automatically when changes to those pieces of data are made in Velocity. Changes to these pieces of information will only update in the Snowflake views during the refresh cycle each morning.

Because of this, special requests can be made to refresh the VW Snowflake views using Touch Events, which are pushed by the DAT team to manually update views with the latest changes to data in the tables.

Scenarios that most commonly require touch events include:

Investor transfers – A loan or group of loans have been transferred to a new investor and the views need to be manually refreshed
Loans with a $0 balance, but are still in repayment status – This is a known issue and is due to the current API occasionally failing to update these loan records appropriately
Note: This touch event will be automated using Rivery at some point and will not be manually conducted once the automation is in place.
The databases
The two primary databases used in Snowflake are VELOCITY_DB and ANALYTICS_DB. These databases contain all of the raw data.

DBT_VELOCITY_DB is a separate database that contains the views that sit on top of the raw data tables. 

The VELOCITY_DB database and DBT_VELOCITY_DB database
Raw Velocity data coming into Snowflake is stored in the VELOCITY_DB database, but Snowflake views are stored in the DBT_VELOCITY_DB database.

DBT_VELOCITY_DB has two different schemas:

"LENDER" for Velocity Servicing
"ORIGINATIONS" for Velocity Originations
One difference between the two schemas is that the Snowflake views for LENDER pull data from the Postgres database, while ORIGINATIONS views use a source file that gets created by the Originations system. This source file gets loaded into an AWS S3 bucket before being picked up by Snowflake (using the Snowpipe service) where changes in data are then loaded into the RAW_ORIGINATION_SOURCE table. It is this table that is used to create the views in Snowflake for Velocity Originations.

The ANALYTICS_DB database
The ANALYTICS_DB database is used by the Consumer Loan Analytics team, as well as The Business for reporting purposes. A list of these reports can be found in Snowflake in the Reports folder of the ANALYTICS_DB database.

External data sources
Snowflake Marketplace connects users to hundreds of data providers, offering thousands of ready-to-use data resources.

Velocity and Analytics use a calendar data source to aid reporting efforts using date driven reports.

The calendars are provided by Mondo Analytics: “Calendars for Financial and Analytics.”

They are accessible via four views, and are installed on the four main Snowflake accounts:

DDAV01VEL (dev and train)
JYA94865 (prod)
SKA48290 (analytics reader train)
XB45703 (analytics reader prod)
The calendar location is in the VELOCITY_FINANCIAL_CALENDARS database, in the PUBLIC schema, on all of the accounts.

Production usage
The XB45703 account has the most frequent usage for these views:

V_CALENDAR_DIM is used by analysts for ad-hoc reporting and for production reporting.
V_CALENDAR_454 is often used by analysts.
V_CALENDAR_445 is rarely used.
V_CALENDAR_544 is not currently used.
Report usage and temporary/transient table usage:


Report usage and temporary/transient table usage data
Temporary and transient table usage:


Temporary and transient table usage data


Processing
Snowflake data purging and backups
To help reduce the size and cost of the Snowflake development account, data in the VELOCITY_DB (dev) tables in the public schema is purged once it becomes greater than 90 days old. Backups of this data are taken prior to the purge.


SQL query for Snowflake 90-day data purge
Snowflake naming conventions
Expand the PDF file below to learn more about naming conventions when using Snowflake.


Snowflake naming conventions
Visibility

The Cease and Desist status view allows Snowflake users to view the historical records of borrowers who have had a cease and desist status, making it easier to respond to audit requests.

The Cellphone Consent History view allows Snowflake users to view the historical records of borrowers who have opted in to cellphone communications, making it easier to respond to audit requests.

Snowflake can display daily loan and borrower counts to aid users in daily reconciliation tasks.

Velocity entity relationships
Use the image here to learn more about the various Velocity entity relationships that exist in Snowflake.


Velocity entity relationship diagram
Planned future functionality

The Data Availability Team has enhancements planned to improve data availability efforts. These enhancements include:

Creating a new catch-all space that would collect any new data elements that have yet to be structured or built into views so that the data is available for use while the Data Availability Team works on structuring the new data elements into views for long-term use.