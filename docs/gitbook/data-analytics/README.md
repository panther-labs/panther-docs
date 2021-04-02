# Data Analytics

Panther's Data Analytics allow for freely searching collected and normalized log data using SQL via [AWS Athena](https://aws.amazon.com/athena/) and [Snowflake](https://www.snowflake.com/).

This is necessary for investigations, baselining behaviors, writing rules, and generating analytics on logs in the context of days, weeks, or months of data.

Panther performs data normalization and processing to store the log data in a standard and efficient way in S3.

Additionally, other applications that can read data from S3 can access this data for search, business intelligence, redundancy, or anything else.

## Available Databases

The following databases are available:

| Database | Description |
| :---: | :--- |
| `panther_logs` | All data sent via Log Analysis, organized by log type |
| `panther_rule_matches` | Events for all triggered alerts, organized by log type |
| `panther_rule_errors` | Events for all errors from rules \(e.g., Python tracebacks\) |
| `panther_views` | Standardized fields across all logs and rule matches |
| `panther_cloudsecurity` | Panther cloud security scanning data |
| `panther_monitor` | Panther data loader self-monitoring \(Snowflake Only\) |

### Panther Logs

This is the main Panther database, holding parsed records of all the onboarded log types. The number and size of the tables here will vary depending on the sources you onboard. See a few sample queries [here](example-queries.md)

### Panther Rule Matches

For every onboarded source that appears in a rule match, Panther creates a row in the corresponding table in the rule matches database. This allows for an easy historical view over what rules are firing and why.

### Panther Rule Errors

Sometimes, due to either incorrect code or permissions issues, a rule returns an error, and does not complete its run successfully. The rule errors tables keep track of any such events, for easy debugging.

### Panther Views

The Panther team has worked hard to bring together common data fields that enable users to do searches across multiple data sources at once. These are exposed here as virtual tables or views.

The following views are available:

| View | Description |
| :---: | :--- |
| `panther_views.all_databases` | Search all data \(logs, rule matches and errors\) |
| `panther_views.all_logs` | Search all log data |
| `panther_views.all_cloudsecurity` | Search all cloud security data |
| `panther_views.all_rule_matches` | Search all events matching rules |
| `panther_views.all_rule_errors` | Search all events causing rule errors |

### Panther Cloud Security

The Panther Cloud Security Database stores AWS configuration information and changes detected from the scans on the monitored environments.

### Panther Monitor \(Snowflake Only\)

Panther Monitor contains information about the data load process into Panther's Snowflake database itself. See the [Snowflake Backend](backend/snowflake.md) section for more details on this.

### Additional Features Coming Soon

We are working hard to add to the power of the Panther Query Engine. We will be delivering ways to create:

* Indicator Lists where users will be able to name and save their own indicator lists
* Automated intel enrichment such as Tor Exit nodes, GeoIP, Cloud provider CIDR ranges.
* Summary Tables that will enable uses to create baselines over large sets of data
* Derivative Tables that will automatically process, filter and/or enrich your log data 
* Search optimization will deliver much faster search speeds
* Pre-canned searches will provide you templates to speed up your work

