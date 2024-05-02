# Snowflake Query Profile SpotApp

SpotApps are ThoughtSpot’s out-of-the-box solution templates built for specific use cases and data sources. They leverage ThoughtSpot Modeling Language (TML) Blocks, which are pre-built pieces of code that are easy to download and implement directly from the product.

The Snowflake Query Profile SpotApp mimics the Snowflake data model. When deployed, it creates several Worksheets, Answers, and Liveboards based on your Snowflake data in your cloud data warehouse.

This is a sample Liveboard, created after you deploy the Snowflake Query Profile SpotApp:
![Sample Liveboard](https://github.com/thoughtspot/Snowflake-Query-Profile-SpotApp/assets/102629468/04372167-7284-4c80-b127-781db796ea2e)

## Prerequisites

Before you can deploy the Snowflake Query Profile SpotApp, you must complete the following prerequisites:

- **Review Required Data**: Examine the required tables and columns for the SpotApp.
- **Ensure Column Compatibility**: Ensure that your columns match the required column type listed in the schema for your SpotApp.
- **Sync Data**: Synchronize all tables and columns from Snowflake to your cloud data warehouse. ThoughtSpot recommends syncing all tables and columns from Snowflake to your CDW. This includes Snowflake’s out-of-the-box columns or any custom columns you are using.
- **Collaborate on Data Movement**: If using an ETL/ELT tool or working with another team within your organization, sync all columns from the tables listed in the SpotApp.
- **Obtain Credentials**: Acquire credentials and SYSADMIN privileges to connect to your Snowflake environment. Your Snowflake environment must contain the data you would like ThoughtSpot to use to create Answers, Liveboards, and Worksheets.
- **Unique Connection Name**: Each new SpotApp connection name must be unique.
- **ACCOUNTADMIN Access to Snowflake**: Maintain ACCOUNTADMIN access to manage Snowflake.
- **Access to Snowflake Tables**: Ensure access to the following Snowflake tables in your Snowflake environment:
  - `QUERY_OPERATOR_STATS_CACHE`

# Snowflake Query Profile SpotApp Schema

The following table describes the schema for the Snowflake Query Profile SpotApp:

| Table                         | Column Name                     | Column Type | Required |
|-------------------------------|---------------------------------|-------------|----------|
| QUERY_OPERATOR_STATS_CACHE    | APPLICATION_NAME                | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | BYTES_SCANNED                   | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | BYTES_SPILLED_LOCAL_STORAGE     | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | BYTES_SPILLED_REMOTE_STORAGE    | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | CLIENT_APPLICATION_ID           | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | CLUSTERING_KEY                  | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | DATABASE_NAME                   | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | ELAPSED_SECONDS                 | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | END_TIME                        | DATE_TIME   | Y        |
| QUERY_OPERATOR_STATS_CACHE    | EXECUTION_STATUS                | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | EXECUTION_TIME_BREAKDOWN        | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | EXPLODING_JOIN                  | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | INEFFICIENT_PRUNING_FLAG        | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | INPUT_ROWS                      | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | LOCAL_DISK_IO                   | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | NETWORK_BYTES                   | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OPERATOR_ATTRIBUTES             | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OPERATOR_EXECUTION_TIME         | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OPERATOR_ID                     | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OPERATOR_STATISTICS             | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OPERATOR_TYPE                   | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | OUTPUT_ROWS                     | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PARENT_OPERATOR_ID              | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PARTITION_SCAN_RATIO            | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PARTITIONS_SCANNED              | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PARTITIONS_TOTAL                | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PERCENTAGE_SCANNED_FROM_CACHE   | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | PROCESSING                      | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | QUERIES_TOO_LARGE_MEMORY        | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | QUERY_ID                        | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | QUERY_TEXT                      | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | ROW_MULTIPLE                    | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | SCHEMA_NAME                     | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | START_TIME                      | DATE_TIME   | Y        |
| QUERY_OPERATOR_STATS_CACHE    | STEP_ID                         | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | SYNCHRONIZATION                 | DOUBLE      | Y        |
| QUERY_OPERATOR_STATS_CACHE    | TABLENAME                       | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | TABLE_NCOLS                     | INT64       | Y        |
| QUERY_OPERATOR_STATS_CACHE    | UNION_WITHOUT_ALL               | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | USER_NAME                       | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | WAREHOUSE_NAME                  | VARCHAR     | Y        |
| QUERY_OPERATOR_STATS_CACHE    | WAREHOUSE_SIZE                  | VARCHAR     | Y        |

## Run SQL Commands

Create an empty table to insert the query operator stats for each query id. Replace `<database_name>.<schema_name>` with the database and schema where you want to create the query stats metadata table.

```sql
CREATE OR REPLACE TABLE <database_name>.<schema_name>.query_operator_stats_cache (
    QUERY_ID VARCHAR(16777216),
    QUERY_TEXT VARCHAR(16777216),
    DATABASE_NAME VARCHAR(16777216),
    SCHEMA_NAME VARCHAR(16777216),
    WAREHOUSE_NAME VARCHAR(16777216),
    WAREHOUSE_SIZE VARCHAR(16777216),
    EXECUTION_STATUS VARCHAR(16777216),
    CLIENT_APPLICATION_ID VARCHAR(16777216),
    APPLICATION_NAME VARCHAR(16777216),
    START_TIME TIMESTAMP_LTZ(6),
    END_TIME TIMESTAMP_LTZ(6),
    USER_NAME VARCHAR(16777216),
    STEP_ID NUMBER(38,0),
    OPERATOR_ID NUMBER(38,0),
    PARENT_OPERATOR_ID NUMBER(38,0),
    OPERATOR_TYPE VARCHAR(16777216),
    OPERATOR_STATISTICS VARIANT,
    EXECUTION_TIME_BREAKDOWN VARIANT,
    OPERATOR_ATTRIBUTES VARIANT,
    TABLENAME VARCHAR(16777216),
    TABLE_NCOLS NUMBER(9,0),
    OPERATOR_EXECUTION_TIME FLOAT,
    LOCAL_DISK_IO FLOAT,
    PROCESSING FLOAT,
    SYNCHRONIZATION FLOAT,
    BYTES_SCANNED NUMBER(38,0),
    PERCENTAGE_SCANNED_FROM_CACHE FLOAT,
    NETWORK_BYTES NUMBER(38,0),
    OUTPUT_ROWS VARIANT,
    INPUT_ROWS VARIANT,
    ROW_MULTIPLE FLOAT,
    BYTES_SPILLED_LOCAL_STORAGE VARIANT,
    BYTES_SPILLED_REMOTE_STORAGE VARIANT,
    PARTITIONS_SCANNED VARIANT,
    PARTITIONS_TOTAL VARIANT,
    PARTITION_SCAN_RATIO FLOAT,
    EXPLODING_JOIN VARCHAR(16777216),
    UNION_WITHOUT_ALL VARCHAR(16777216),
    QUERIES_TOO_LARGE_MEMORY VARCHAR(16777216),
    INEFFICIENT_PRUNING_FLAG VARCHAR(16777216),
    ELAPSED_SECONDS NUMBER(38,6),
    CLUSTERING_KEY VARCHAR(16777216)
);
```

## Run SQL Commands

Create an empty table to insert the query operator stats for each query id. Replace `<database_name>.<schema_name>` with the database and schema where you want to create the query stats metadata table.

```
CREATE OR REPLACE TABLE <database_name>.<schema_name>.query_operator_stats_cache (
    QUERY_ID VARCHAR(16777216),
    QUERY_TEXT VARCHAR(16777216),
    DATABASE_NAME VARCHAR(16777216),
    SCHEMA_NAME VARCHAR(16777216),
    WAREHOUSE_NAME VARCHAR(16777216),
    WAREHOUSE_SIZE VARCHAR(16777216),
    EXECUTION_STATUS VARCHAR(16777216),
    CLIENT_APPLICATION_ID VARCHAR(16777216),
    APPLICATION_NAME VARCHAR(16777216),
    START_TIME TIMESTAMP_LTZ(6),
    END_TIME TIMESTAMP_LTZ(6),
    USER_NAME VARCHAR(16777216),
    STEP_ID NUMBER(38,0),
    OPERATOR_ID NUMBER(38,0),
    PARENT_OPERATOR_ID NUMBER(38,0),
    OPERATOR_TYPE VARCHAR(16777216),
    OPERATOR_STATISTICS VARIANT,
    EXECUTION_TIME_BREAKDOWN VARIANT,
    OPERATOR_ATTRIBUTES VARIANT,
    TABLENAME VARCHAR(16777216),
    TABLE_NCOLS NUMBER(9,0),
    OPERATOR_EXECUTION_TIME FLOAT,
    LOCAL_DISK_IO FLOAT,
    PROCESSING FLOAT,
    SYNCHRONIZATION FLOAT,
    BYTES_SCANNED NUMBER(38,0),
    PERCENTAGE_SCANNED_FROM_CACHE FLOAT,
    NETWORK_BYTES NUMBER(38,0),
    OUTPUT_ROWS VARIANT,
    INPUT_ROWS VARIANT,
    ROW_MULTIPLE FLOAT,
    BYTES_SPILLED_LOCAL_STORAGE VARIANT,
    BYTES_SPILLED_REMOTE_STORAGE VARIANT,
    PARTITIONS_SCANNED VARIANT,
    PARTITIONS_TOTAL VARIANT,
    PARTITION_SCAN_RATIO FLOAT,
    EXPLODING_JOIN VARCHAR(16777216),
    UNION_WITHOUT_ALL VARCHAR(16777216),
    QUERIES_TOO_LARGE_MEMORY VARCHAR(16777216),
    INEFFICIENT_PRUNING_FLAG VARCHAR(16777216),
    ELAPSED_SECONDS NUMBER(38,6),
    CLUSTERING_KEY VARCHAR(16777216)
);
```

**Task Creation**
Create or replace tasks to capture the id of each query which ran for more than 2 minutes and scanned more than 1 MB of data in the last 14 days. The task is scheduled to run every 60 minutes.

```
CREATE OR REPLACE TASK query_session_task
    WAREHOUSE = <warehouse_names>
    SCHEDULE = '60 minute'
    AS
    CREATE OR REPLACE TABLE <database_name>.<schema_name>.query_sessions AS
    SELECT QUERY_ID
    FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY q
    INNER JOIN SNOWFLAKE.ACCOUNT_USAGE.SESSIONS s
    ON q.SESSION_ID = s.SESSION_ID
    WHERE bytes_scanned > 1024 * 1024
    AND total_elapsed_time / 1000 > 120
    AND execution_status = 'SUCCESS'
    AND end_time >= CURRENT_DATE - INTERVAL '13 DAY'
    AND query_type = 'SELECT'
    AND warehouse_name IS NOT NULL
    ORDER BY start_time desc;
```

Create another task to capture the query operator stats for the query ids collected from the above step. The task is scheduled to run every 65 minutes.
CREATE OR REPLACE TASK query_stats_task
```
    WAREHOUSE = <warehouse_names>
    SCHEDULE = '65 minute'
    AS
    EXECUTE IMMEDIATE $$
    DECLARE
        query_id STRING;
        qid INT;
        session_id INT;
        c1 CURSOR FOR
            SELECT query_id
            FROM <database_name>.<schema_name>.query_sessions
            WHERE query_id NOT IN (SELECT query_id FROM <database_name>.<schema_name>.query_operator_stats_cache);
    BEGIN
        OPEN c1;
        FOR record IN c1 DO
            FETCH c1 INTO query_id;
            INSERT INTO <database_name>.<schema_name>.query_operator_stats_cache
            WITH query_stats AS (
                SELECT
                q_h.QUERY_ID,
                q_h.QUERY_TEXT,
                ...
            FROM ...
            ORDER BY STEP_ID, OPERATOR_ID)
            SELECT ...
            FROM query_stats
            INNER JOIN "SNOWFLAKE"."ACCOUNT_USAGE".SESSIONS s ON query_stats.SESSION_ID = s.SESSION_ID
            LEFT JOIN "SNOWFLAKE"."INFORMATION_SCHEMA"."TABLES" t ON query_stats.tablename = t.TABLE_CATALOG || '.' || t.TABLE_SCHEMA || '.' || t.TABLE_NAME;
        END FOR;
        RETURN query_id;
    END
    $$;
  ```

Execute or resume the tasks created above to start immediately.

To run the task immediately:
```
EXECUTE TASK query_session_task;
EXECUTE TASK query_stats_task;
```
To run the task at the scheduled interval:

```
ALTER TASK query_session_task RESUME;
ALTER TASK query_stats_task RESUME;
The query_operator_stats_cache table data updates on a daily basis. You must set up retention on the table. Use the following command to set up retention for the query_operator_stats_cache table.
```
```
ALTER TABLE <database_name>.<schema_name>.query_operator_stats_cache SET DATA_RETENTION_TIME_IN_DAYS=30;
```

The number of days can be set based on your requirements.
Wait for the tasks created above to run, capturing the query profile stats at the individual query level.


Once the stats are captured, you are ready to deploy the Snowflake Query Profile SpotApp.

## Deploy the SpotApp

After you have downloaded the Zip file and verified its contents, follow these steps:

1. Log into your ThoughtSpot instance and create an Embrace connection to all of the relevant views.
2. Import the TML for the Worksheets and verify that it has all been imported without any errors.
3. Import the TML for the Liveboards and verify that it has all been imported without any errors.
4. You are ready to start searching your data!

For detailed instructions on how to import TML files, refer to the [ThoughtSpot documentation](https://docs.thoughtspot.com/software/latest/tml-import-export-multiple).


