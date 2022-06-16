# Snowflake Integration

Panther is configured to write processed log data to an AWS-based [Snowflake](https://www.snowflake.com) database cluster.&#x20;

Integrating Panther with Snowflake enables Panther data to be used with your Business Intelligence tools to make dashboards tailored to you operations. In addition, you can join Panther data (e.g., Panther alerts) to your business data, enabling assessment of your security posture with respect to your organization.

For example, you can tally alerts by organizational division (e.g., Human Resources) or by infrastructure (e.g., Development, Test, Production).

Panther uses [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro.html) to copy the data into your Snowflake cluster.

## Configuration Overview

### SaaS

For Panther SaaS customers, a Snowflake instance is automatically provisioned and we take care of setup and maintenance for you. You're all set!

For SaaS customers who wish to share data between the Panther SaaS Snowflake account and their own internal Snowflake account, this can be accomplished by contacting your Panther support team.

For SaaS customers who wish to access their SaaS configuration to use the data in BI reporting, but do not have their own corporate Snowflake account, our support team can provision special access roles for your use.&#x20;

### Bring Your Own Snowflake (BYOSF)

If you are an existing Snowflake customer, Panther can be configured to use one of your own Snowflake accounts. We call this Bring Your Own Snowflake (BYOSF).&#x20;

#### Step 1: Create a new Snowflake Account&#x20;

Create a new dedicated Snowflake account for Panther using the example template below. To minimize latency, your Panther deployment and Snowflake instance should reside in the same AWS region.

```sql
USE ROLE ORGADMIN;
CREATE ACCOUNT <YOUR_PANTHER_ACCOUNT_NAME>
  ADMIN_NAME = <YOUR_ADMIN_NAME>
  ADMIN_PASSWORD = '<YOUR ADMIN PASSWORD>' # we recommend at least 32 characters
  EMAIL = '<your sfnowflake DBA email>'
  MUST_CHANGE_PASSWORD = FALSE
  EDITION = ENTERPRISE # BUSINESS_CRITICAL edition required for certain features
  REGION = <your region, i.e. aws_us_west_2>
  COMMENT =  'Panther Snowflake BYOSF Production Environment'; 
```

#### Step 2: Create a Panther Account Admin

Next, in the new account, create a Panther Account Administrator user and grant it administrative privileges using the following commands:&#x20;

```sql
USE ROLE SECURITYADMIN;

CREATE USER IF NOT EXISTS pantheraccountadmin password='<panther_credential>';

GRANT ROLE SYSADMIN
   TO USER pantheraccountadmin;
   
GRANT ROLE SECURITYADMIN
   TO USER pantheraccountadmin;

GRANT ROLE ACCOUNTADMIN
   TO USER pantheraccountadmin;
   
ALTER USER pantheraccountadmin SET DEFAULT_ROLE = SYSADMIN;
```

The Panther customer support team will _**provide**_ you with a unique one-time credential over a secure channel to use for this account. Panther will regularly rotate this credential in the future, so you are advised to maintain a separate administrative user for your own administrative needs.

And you're done! Panther will automatically configure and maintain the account for you using our automated tooling. We'll manage integrations, databases, warehouses, and users and roles in the new account.&#x20;

#### **Disabling and Reenabling the Panther Account Admin User (Optional)**

This new role is only needed for the original deploy and occasional on-upgrade migrations. If desired, the administrative role can be disabled after the initial deployment is complete:

```sql
USE ROLE SECURITYADMIN;

ALTER USER PANTHERACCOUNTADMIN SET DISABLED = TRUE
```

If a Panther upgrade requires us to do a data migration, the user can be temporarily reenabled:

```sql
USE ROLE SECURITYADMIN;

ALTER USER PANTHERACCOUNTADMIN SET DISABLED = FALSE
```

### Customer-managed Snowflake Integration

For customer-managed Snowflake connection options see [this page](customer-managed-snowflake.md).
