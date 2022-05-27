---
description: 'Panther''s Data Analytics: Run queries and search your normalized log data'
---

# Data Analytics

Panther's Data Analytics allow for freely searching collected and normalized log data using SQL via [Snowflake](https://docs.panther.com/data-analytics/backend/snowflake) and [AWS Athena](https://docs.panther.com/data-analytics/backend/athena).

This is necessary for investigations, baselining behaviors, writing rules, and generating analytics on logs in the context of days, weeks, or months of data.

Panther performs data normalization and processing to store the log data in a standard and efficient way in S3.

Additionally, other applications that can read data from S3 can access this data for search, business intelligence, redundancy, or anything else.

## Available Databases

The following databases are available:

|              Database              | Description                                                                                                                                                                                                                                                                                                          |
| :--------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|           `panther_logs`           | <p>All data sent via Log Analysis, organized by log type.<br><br>This is the main Panther database, holding parsed records of all the onboarded log types. The number and size of the tables here will vary depending on the sources you onboard. See a few sample queries <a href="example-queries.md">here</a></p> |
|       `panther_rule_matches`       | <p>Events for all triggered alerts, organized by log type.<br><br>For every onboarded source that appears in a rule match, Panther creates a row in the corresponding table in the rule matches database. This allows for an easy historical view over what rules are firing and why.</p>                            |
|        `panther_rule_errors`       | <p>Events for all errors from rules (e.g., Python tracebacks)<br><br>rect code or permissions issues, a rule returns an error, and does not complete its run successfully. The rule errors tables keep track of any such events, for easy debugging.</p>                                                             |
|           `panther_views`          | Standardized fields across all logs and rule matches.                                                                                                                                                                                                                                                                |
|       `panther_cloudsecurity`      | Panther cloud security scanning data.                                                                                                                                                                                                                                                                                |
| `panther_monitor` (Snowflake only) | <p>Panther data loader self-monitoring.<br><br>Panther Monitor contains information about the data load process into Panther's Snowflake database itself. See the <a href="backend/snowflake.md">Snowflake Backend</a> section for more details on this.</p>                                                         |

### Panther Views

Panther Views bring together common data fields that enable you to search across multiple data sources at once.&#x20;

The following views are available:

|                View               | Description                                                                                                                                                                               |
| :-------------------------------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   `panther_views.all_databases`   | Search all data (logs, rule matches and errors)                                                                                                                                           |
|      `panther_views.all_logs`     | Search all log data                                                                                                                                                                       |
| `panther_views.all_cloudsecurity` | <p>Search all cloud security data.<br><br>The Panther Cloud Security Database stores AWS configuration information and changes detected from the scans on the monitored environments.</p> |
|  `panther_views.all_rule_matches` | Search all events matching rules                                                                                                                                                          |
|  `panther_views.all_rule_errors`  | Search all events causing rule errors                                                                                                                                                     |

