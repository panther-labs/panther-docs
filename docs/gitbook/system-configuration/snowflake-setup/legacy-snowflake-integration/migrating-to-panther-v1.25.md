# Migrating to Panther v1.25

### Disclaimer

The following instructions are only intended for users who currently opt for a [Customer-managed Snowflake Integration](../customer-managed-snowflake/#legacy-snowflake-configuration-deprecated) and are looking to upgrade to Panther v1.25

### Support for Panther Lookup Tables

In v1.25, Panther supports **Lookup Tables,** a feature that allows users to import their own data for quickly enriching alerts. This requires a new database table and permissions grants to Panther Snowflake roles.

To update the Panther data lake, run the following script as a user with at least `SECURITYADMIN` and `SYSADMIN` privileges:

```sql
USE ROLE SYSADMIN;

-- Create the new PANTHER_LOOKUPS database
CREATE DATABASE IF NOT EXISTS PANTHER_LOOKUPS;

USE ROLE SECURITYADMIN;

-- Grant read privileges to existing Panther roles on the new DB
GRANT USAGE 
	ON DATABASE PANTHER_LOOKUPS 
	TO ROLE PANTHER_READONLY_ROLE;
GRANT USAGE 
	ON SCHEMA PANTHER_LOOKUPS.PUBLIC 
	TO ROLE PANTHER_READONLY_ROLE;
GRANT SELECT 
	ON ALL TABLES IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
	TO ROLE PANTHER_READONLY_ROLE;
GRANT SELECT 
	ON ALL VIEWS IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
	TO ROLE PANTHER_READONLY_ROLE;
GRANT SELECT 
	ON FUTURE TABLES IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
	TO ROLE PANTHER_READONLY_ROLE;
GRANT SELECT 
	ON FUTURE VIEWS IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
	TO ROLE PANTHER_READONLY_ROLE;

-- Grant create privileges to existing Pnther roles on the new DB
GRANT CREATE TABLE, CREATE VIEW, CREATE STAGE, CREATE PIPE, MODIFY 
  ON ALL SCHEMAS IN DATABASE PANTHER_LOOKUPS 
  TO ROLE PANTHER_ADMIN_ROLE;
GRANT SELECT, INSERT, UPDATE 
  ON ALL TABLES IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
  TO ROLE PANTHER_ADMIN_ROLE;
GRANT SELECT, INSERT, UPDATE 
  ON FUTURE TABLES IN SCHEMA PANTHER_LOOKUPS.PUBLIC 
  TO ROLE PANTHER_ADMIN_ROLE;
```

Once run, your data lake will contain a new `PANTHER_LOOKUPS` database, which is used by the new Lookup Tables feature.
