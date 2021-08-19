# Snowflake

Panther can be configured to write processed log data to an AWS-based [Snowflake](https://www.snowflake.com) database cluster. Once the Panther-Snowflake integration is set up, Data Explorer, Indicator Search and other back-end processes will automatically switch to using Snowflake instead of Athena.

Integrating Panther with Snowflake enables Panther data to be used in your Business Intelligence tools to make dashboards tailored to you operations. In addition, you can join Panther data \(e.g., Panther alerts\) to your business data, enabling assessment of your security posture with respect to your organization.

For example, you can tally alerts by organizational division \(e.g., Human Resources\) or by infrastructure \(e.g., Development, Test, Production\).

Panther uses [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro.html) to copy the data into your Snowflake cluster.

## Use additional data sets in Panther

Panther uses a `panther_readonly` Snowflake user to query the data in Snowflake. By default, this user's role `panther_readonly_role` is only endowed with a minimal set of grants to enable it to access the data in the panther databases. However, if you wish to add your own preexisting datasets to your Panther data-explorer queries \(such as HR data, in-house or vendor-provided whitelists/blacklists\) you can easily make that data accessible to the role.

```sql
GRANT USAGE
  ON DATABASE my_database_name
  TO ROLE panther_readonly_role;
GRANT USAGE
  ON SCHEMA my_database_name.my_schema_name
  TO ROLE panther_readonly_role;
GRANT SELECT
  ON TABLE  my_database_name.my_schema_name.my_table_name
  TO ROLE panther_readonly_role;
```

Note that the newly granted database, schema and table will _not_ populate in the Panther sidebar, but you will be able to access it using regular SQL.

## Using the Panther Monitor Database

### Overview

Panther implements a set of basic data load status self-monitoring tables:

* a pipe status monitoring table `panther_monitor.public.pipe_history`
* a file load status monitoring table `panther_monitor.public.load_history`
* a data source table history `panther_monitor.public.table_history`
* a monitoring history, tracking any load errors encountered `panther_monitor.public.monitor_history`

Under the default snowflake configuration settings, a master stack variable named `SnowflakeMonitorRunFrequency` is set to run a monitoring sweep every 180 minutes. This variable can be adjusted as desired down to a minimum of once every 2 minutes, or up to a maximum of once every 10080 minutes \(a week\).

If a data loading error is found by the monitor sweep, this should trigger an alarm in CloudWatch, under the `/aws/lambda/panther-snowflake-admin-api` log group. The data loading errors are also stored in the data monitoring history table.

The monitoring tables are implemented in the `panther_monitor` database, and can be queried via the WebUI or the Panther Data Explorer. Please note that the monitor database is _not_ currently one of the pre-populated data sets in the Panther UI.

### Upgrading considerations

Customers should run the latest version of the snowflake [setup instructions](../../system-configuration/snowflake-setup/) before upgrading to a version including snowflake data load monitoring.

### Using the load monitors

The monitors are designed to capture information from a variety of sources: 1. The pipe status captures information about the state of all the snowpipes used by the Panther system, and will keep a history of it in the `pipe_history` table. Pipes that are not in a `RUNNING` state will generate an alert in CloudWatch and will be recorded as an entry in `monitor_history`

1. Load status of every file for every table is stored in `load_history` -- if a file failed to load \(state not `LOADED` or `LOAD_IN_PROGRESS`\) for any reason, the even will be recorded in CloudWatch and also as an entry in the `monitor_history` table.
2. Table History -- at every run of the monitoring sweep, the table history will update, including such information as row and byte counts and row and byte count deltas. Please note that due to clustering compaction operations by Snowflake, byte counts _may_ decrease occasionally between sweeps. Additional useful information in this table is a last\_altered time which indicates the last time changes were made to the data in the table, and a last altered delta, indicating the time in seconds between when these changes were made and the last monitor sweep. As different data sources can have dramatically different load frequencies \(ranging from seconds to weeks\) no default alerting is currently driven from the table history table.
3. Monitor History -- if there are errors encountered during the load process \(an example would be s3 access errors caused by a change in s3 permissions\) such errors would be summarized in the monitor\_history table.

