---
description: >-
  Using Panther's Data Analytics to run queries and search your normalized log
  data
---

# Data Analytics

## Overview

Panther's Data Analytics allow for freely searching collected and normalized log data using SQL in a [Snowflake](https://docs.panther.com/data-analytics/backend/snowflake) security data lake. As Panther ingests data, we parse, normalize and store that data in Snowflake, which is necessary for investigations, baselining behaviors, writing rules, and generating analytics on logs in the context of days, weeks, or months of data.

## Panther's Data Analytics

### Standard Fields

Panther's log analysis applies normalization fields (IPs, domains, etc) to all log records. These fields standardize names for attributes across all data sources enabling fast and easy data correlation. For more information, see [Standard Fields](panther-fields.md).

### Indicator Search

Panther's Indicator Search runs quick investigations on [common indicators](panther-fields.md#indicator-fields) across various data sources. Indicator Search removes the need to write SQL to answer common questions about suspicious activity and presents results in a simple visualization. For more information, see [Indicator Search](indicator-search.md).

### Data Explorer

Panther's Data Explorer allows you to view normalized data and perform SQL queries with autocompletion. With Data Explorer, you can browse log data and rule matches, search standard fields across data, manage queries, create scheduled queries, select JSON rows to use as unit tests, share results with your team through a link, and download results in a CSV. For more information, see [Data Explorer.](data-explorer.md)

### Saved Queries

Panther's Saved Queries allows you to save, view, manage, load, update, and delete queries. Saved Queries simplify the process of building context around alerts, shortening the time and effort it takes to get a fully actionable detection story. Saving custom, reusable queries can make investigations more efficient, allow you to utilize [Scheduled Queries](./#scheduled-queries), and in some cases obviate the need for SQL familiarity in order for fast triage and response. For more information, see [Saved Queries](saved-queries.md).

### Scheduled Queries

Panther's Scheduled Queries allow you to use SQL queries as opposed to streaming data as a feed into Panther's rules engine using Scheduled Rules. As a Scheduled Query runs, if a corresponding [Scheduled Rule](scheduled-queries.md#create-a-scheduled-rule) returns any hits, one or more `Alerts` will be generated from the data and dispatched accordingly. For more information, see [Scheduled Queries](scheduled-queries.md).

### Query History

The Query History page displays the last 30 days of SQL queries run through the Panther UI. Clicking on the query name will send you to the data explorer where you can see the results and rerun the query. For more information, see [Query History](query-history.md).

### Example Queries

From VPC Activities, to IPs by row count, to [Okta login queries](../guides/okta-detections-and-queries.md), and more – browse a list of [Example Queries](example-queries.md) run by real Panther customers.&#x20;

## Available Databases

The following databases are available for analyzing in Panther:

|              Database              | Description                                                                                                                                                                                                                                                                                                           |
| :--------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|           `panther_logs`           | <p>All data sent via Log Analysis, organized by log type.<br><br>This is the main Panther database, holding parsed records of all the onboarded log types. The number and size of the tables here will vary depending on the sources you onboard. See a few sample queries <a href="example-queries.md">here</a>.</p> |
|       `panther_rule_matches`       | <p>Events for all triggered alerts, organized by log type.<br><br>For every onboarded source that appears in a rule match, Panther creates a row in the corresponding table in the rule matches database. This allows for an easy historical view over what rules are firing and why.</p>                             |
|        `panther_rule_errors`       | <p>Events for all errors from rules (e.g., Python tracebacks)<br><br>Errors in code or permissions issues, a rule returns an error, and does not complete its run successfully. The rule errors tables keep track of any such events, for easy debugging.</p>                                                         |
|           `panther_views`          | Standardized fields across all logs and rule matches.                                                                                                                                                                                                                                                                 |
|       `panther_cloudsecurity`      | Panther cloud security scanning data.                                                                                                                                                                                                                                                                                 |
| `panther_monitor` (Snowflake only) | <p>Panther data loader self-monitoring.<br><br>Panther Monitor contains information about the data load process into Panther's Snowflake database itself. See the <a href="backend/snowflake.md">Snowflake Backend</a> section for more details on this.</p>                                                          |

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

