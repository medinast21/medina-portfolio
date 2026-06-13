# Technical Reference

This document was created at the request of a director who wanted a holistic picture of how the team's tools and systems integrated with one another. It was written for an internal audience of both technical and non-technical business stakeholders who needed a shared reference for how data flows through the team's Snowflake-based infrastructure.

!!! note "A note on terminology"
        Velocity is the internal name for the company's loan servicing platform.

        All references to specific database names and URLs in this document have been anonymized for portfolio purposes.

## What is Snowflake?

Snowflake is a cloud-based data warehouse that imports data from a collection of databases that exist in PostgreSQL (Postgres) using AWS Glue and a tool called Rivery. The Velocity platform is comprised of various microservices that all communicate using various APIs, and each microservice contains a collection of tables. Velocity has approximately 60 different microservices and corresponding databases in Postgres, which Snowflake can pull from to query and present data.

The Data Availability Team (DAT) uses a tool called DBT to create different "views" within Snowflake. Each view is a SQL query structured by the DAT to display various data elements in ways that are useful for analysis. There are different types of Snowflake views, each with associated characteristics including how often the underlying tables are refreshed with new data.

Snowflake also serves as a data warehouse for entities beyond Velocity, meaning it contains data not maintained by the DAT. Users looking for information in those areas will need to contact the owner of the specific space.

!!! info "Data before Snowflake"
        Velocity launched in 2019 and Snowflake became the preferred data warehouse in 2021. 

        Due to this timeline, approximately 1.5 years of Velocity data does not exist in Snowflake. 
        
        An older RDS environment (Redshift Postgres) that acted as a temporary solution prior to adopting Snowflake is still active and houses this earlier data. The earliest records in Snowflake have a timestamp of around March 2021.

## What is DBT?

DBT is the development environment used by the Data Availability Team to create and manage Snowflake views. Each view used by Nelnet users in Snowflake is developed in DBT, which interfaces with GitHub to provide version control and collaboration features.

DBT also acts as a data transformation tool, allowing users to take source data and model it in ways that materialize different views and tables. Its functionality goes beyond simply creating views. It also serves as the central location for all of the DAT's column-level documentation, which is added as the team works on new tickets and projects.

To learn more about DBT, visit the [DBT website](https://docs.getdbt.com/).

## What is the Data Dictionary?

The Data Dictionary provides descriptions for all of the various data elements that exist within Velocity. For example, if a user queries one of the Velocity APIs and receives a JSON payload, the Data Dictionary can be used to look up each data element name and its description.

The Data Dictionary currently exists in Excel spreadsheets, but efforts are underway to move it into a DBT repository. Doing so would centralize it for all users, allow descriptions to be updated in a live database, make it accessible directly within Snowflake, and allow DBT users to see the relationships between data elements and the views they appear in, or their lineage.

## Read-only vs. full access users

For security and licensing purposes, there are two Snowflake instances a user can log in to:

* Read-only
* Full access

The role and permissions assigned to each user determine which instance they will primarily use. The main difference is that the full access instance makes more tables visible. Because full access licenses are more expensive, most users are given read-only access.

The URLs for each instance are:

* Read-only: `https://xxxxxxxx.snowflakecomputing.com/`
* Full access: `https://app.snowflake.com/us-west/xxxxxxxxx/`

## Snowflake views and table refresh times

Not all data in Velocity is available in Snowflake. This is either because older data elements were never added to views, or because newly developed elements have not yet been included.

!!! info
        The views in Snowflake are non-materialized, meaning each view re-runs the SQL query that generates it and displays current data when called. 
        
        Views are necessary because users need access to the data without direct access to the underlying tables.

The different types of views currently available in Snowflake for the XYZ database are:

| View Type / Name | Scheduled table refresh times | Description |
|------------------|-------------------------------|-------------|
| **(VW)** View | Financial data updates in near real-time.<br>Non-financial data updates during the scheduled refresh.<br><br>**Starts**: 5:00a UTC / 11:00p CST<br>**Ends**: 11:20a UTC / 5:20a CST | Queries pulling data in near real-time as it changes in Velocity. Non-financial data changes (address, phone number, e-correspondence/SCRA status, etc.) only update during the morning refresh cycle. |
| **(DS)** Daily snapshot | **Starts**: 5:00a UTC / 11:00p CST<br>**Ends**: 11:20a UTC / 5:20a CST | Queries that snapshot data elements at a scheduled time. Data is accurate as of the last refresh. Built from AWS Glue (and Rivery) tables brought over nightly. |
| **(RP)** Reports | **Starts**: 5:00a UTC / 11:00p CST<br>**Ends**: 11:20a UTC / 5:20a CST | Reporting views built on top of VW and DS views. |
| **(CRP)** Citizens reports | **Starts**: 5:00a UTC / 11:00p CST<br>**Ends**: 11:20a UTC / 5:20a CST | Same as RP views, but specific to *Client*. |

The DAT is currently working on consolidating all table refresh times to a single refresh at 7:00a UTC using Rivery.

Within the XYZ database there are two types of tables:

* One that refreshes in near real-time as new data comes into Velocity (a single broad table)
* All other tables, which refresh on a schedule starting at 11:00p CST and completing by 5:20a CST

## Touch events

Non-financial data changes, such as address updates, phone number changes, and boolean status changes, do not update automatically in VW views when changes are made in Velocity. These changes only appear after the morning refresh cycle.

For situations where views need to be updated before the next scheduled refresh, special requests can be made to the Data Availability Team to push a Touch Event, which manually refreshes the relevant views.

Scenarios that most commonly require touch events:

* **Investor transfers** — A loan or group of loans has been transferred to a new investor and the views need to be manually refreshed.
* **Loans with a $0 balance still in repayment status** — A known issue caused by the API occasionally failing to update these loan records appropriately. This touch event will be automated using Rivery and will eventually not require manual intervention.

## The databases

The two primary databases used in Snowflake are ABC_DB and ANALYTICS_DB, which contain all raw data. XYZ_DB is a separate database containing the views that sit on top of the raw data tables.

### ABC_DB and XYZ_DB

Raw Velocity data coming into Snowflake is stored in ABC_DB, while Snowflake views are stored in XYZ_DB. XYZ_DB has two schemas:

* **LENDER** — for Velocity Servicing
* **ORIGINATIONS** — for Velocity Originations

The LENDER schema pulls data directly from the Postgres database. ORIGINATIONS views use a source file created by the Originations system, which is loaded into an AWS S3 bucket and picked up by Snowflake via the Snowpipe service. Changes are then loaded into the ORIGINATION_SOURCE table, which is used to create the Originations views.

### ANALYTICS_DB

The ANALYTICS_DB database is used by the Analytics team and the Business for reporting purposes. A list of available reports can be found in the Reports folder of the ANALYTICS_DB database in Snowflake.

## External data sources

Snowflake Marketplace provides users with access to hundreds of data providers and thousands of ready-to-use data resources. Velocity and Analytics use a calendar data source to support date-driven reporting, specifically "Calendars for Financial and Analytics" provided by Mondo Analytics.

This data source is installed on the four main Snowflake accounts:

* XYXYXCHCH (dev and train)
* YZYZWEWEW (prod)
* HGHGHGXXX (analytics reader train)
* FV12DCH (analytics reader prod)

The calendar data is located in the FINANCIAL_CALENDARS database, PUBLIC schema, across all accounts.
