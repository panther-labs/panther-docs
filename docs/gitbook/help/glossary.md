---
description: >-
  This Glossary introduces common cloud-native, security, and Panther-specific
  terminology.
---

# Glossary

### A

#### **Alert**

> * A brief and human-readable event that correlated to a programmed alarm rule to provide information about data breaches, exploits, or malicious behaviors.
> * The event triggered by Panther after the criteria on your rule, policy, or query is met. See [Triaging Alerts](../writing-detections/triaging-alerts/) for more information.

#### **Alert Destination**

> * A designated location where a security alert is sent after being created.
> * Your selected services(s) where Panther alerts are sent, such as Jira, Slack, or PagerDuty. See [Destinations](../destinations/) for more information.

#### **API (application programming interface)**

> * A connection between computers or applications, which defines a specific set of rules for how they communicate and interact with one another.
> * [Panther's GraphQL API](../api-beta/) is used for alert, role, and user management, and data lake querying.

#### Auxiliary Functions

> Python functions that control analysis logic, generated alert title, event grouping, routing of alerts, and metadata overrides for [Panther's Detections](../writing-detections/#detection-types). These functions are applicable to both [Rules](../writing-detections/rules.md) and [Policies](../writing-detections/policies.md).

### **C**

#### **CI/CD (continuous integration/continuous deployment)**

> * Continuous integration means work is constantly merged back into a central location, and generally includes automated testing for safety purposes. Continuous deployment or delivery means work is constantly deployed into production.
> * See Panther's [CI/CD Onboarding Guide](../guides/ci-cd-onboarding-guide.md) for information on managing detections with a CI/CD workflow.

#### **CLI (command line interface)**

> * A term for tools that you interact with from a command line, shell, virtual terminal, or similar interface.
> * In Panther’s context, CLI refers to tools like [`panther_analysis_tool`](../writing-detections/panther-analysis-tool.md) and `pantherlog`, which are distributed by Panther and executed by customers locally on their own machines.

#### **Cloud-Native**

> Cloud-native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.&#x20;
>
> **Source:** [CNCF](https://github.com/cncf/toc/blob/main/DEFINITION.md)

#### **Cron**

> * A time-based scheduler that executes one or more commands at specific dates and times.
> * In Panther's context, a Cron Expression is used to set a defined interval while running [Scheduled Rules](glossary.md#scheduled-rule) or [Scheduled Queries](glossary.md#scheduled-query).

#### **Custom Webhook**

> * Also known as web callbacks; a lightweight API that enables one system to forward data to another system when a specific event occurs.
> * Panther’s [custom webhook](../destinations/custom\_webhook.md) alert destination allows you to deliver alerts to selected third-party platforms that accept webhooks.

### **D**

#### **Data Explorer**

> [Data Explorer](../data-analytics/data-explorer.md) is a Panther tool where you can view your normalized data, select rule matches, perform SQL queries, search standard fields across data, load or schedule queries, and download sharable results in a CSV file.

#### **Deduplication**

> In Panther, [deduplication](../writing-detections/rules.md#how-rules-work) refers to the process of grouping suspicious events together into a single alert to prevent receiving duplicate alerts for the same behavior that may have multiple indicators. Any event that triggers a detection is grouped together with other events that triggered the same detection and subsequent deduplication string within the designated deduplication period. This is controlled by two aspects:
>
> * The deduplication string returned by the `dedup` function
> * The deduplication period configured on a detection

#### **Detections-as-Code (DaC)**

> Detections-as-Code is a modern, flexible, and structured approach to writing security detections that applies software engineering best practices like version control systems (VCS) to manage detections, requires testing and manual reviews for detection changes, automatically enforces these testing and standards (CI), and automatically deploys these changes (CD).

#### **Detection Pack**

> Panther’s [Detection Packs](glossary.md#detection-pack) logically group detections as well as enable detection updates via the Panther Console. Panther-provided packs are defined in this open-source repository: [`panther-labs/panther-analysis`](https://github.com/panther-labs/panther-analysis).

### **E**

#### **EDR (endpoint detection and response)**

> A cybersecurity solution that continuously monitors endpoint data and triggers rule-based automated responses.

#### **Enrichment**

> Panther’s Enrichment features add important context to your detections and alerts for faster investigation workflows, enhanced detections, and reduced alert noise. Panther offers the following enrichment features: [Lookup Tables](https://docs.panther.com/enrichment/lookup-tables) and a [GreyNoise integration](https://docs.panther.com/enrichment/greynoise).

### **G**

#### **Global Helper**

> * A “helper” function performs one part of the computation of a larger function or program. This allows you to re-use logic defined in one place multiple times, in addition to logically separating code for better comprehension and testing.
> * In Panther, [Global Helpers](../writing-detections/globals.md) contain python code that can be used in other types of Panther detections (such as policies, rules, and data models). These Global Helpers serve as a library of common programming patterns that you can extend and use in any of the detections you write.

#### **GreyNoise**

> * A company that collects, analyzes, and labels data on IPs that scan the internet and saturate security tools with noise
> * Panther's [GreyNoise](../enrichment/greynoise/) integration allows you to reduce false-positive alerts by identifying opportunistic attacks allowed into your perimeter and emerging threats based on GreyNoise context data and tagging.

### **I**

#### **IOC (indicator of compromise)**

> Data collected that is likely to indicate a security breach or threat.

**Indicator Search**

> Panther’s [Indicator Search](../data-analytics/indicator-search.md) queries all of your normalized log data to find related activity based on common IOCs such as usernames, emails, IPs, and more to tell the full story during an incident.

### **L**

#### **Log Normalization**

> Log normalization parses and normalizes your uploaded logs for IOC (indicator of compromise) fields like domains and IPs to support efficient and effective analysis, searches, and correlations across all log types.

#### **Lookup Tables**

> Panther’s [Lookup Tables](../enrichment/lookup-tables/) allow you to add important context to your detections and alerts by enriching the events they process and contain. They help you save time by enhancing detections, reducing alert noise, and speeding up investigations for improved investigation workflows.

### **M**

#### **MDR (managed detection and response)**

> A cybersecurity service that continuously monitors all security data and thus allows for robust detection, monitoring, and response to limit malicious threats and breaches.

### **P**

#### **Panther-analysis repo**

> A [public Github repository](https://github.com/panther-labs/panther-analysis) of all detections developed by Panther including rules, policies, and scheduled rules.

#### **Panther Analysis Tool (PAT)**

> Also known as [panther\_analysis\_tool](../writing-detections/panther-analysis-tool.md); an open-source utility for testing, packaging, and deploying Panther detections from source code. PAT’s intent is to enable CI/CD workflows for customers.

#### **Panther Console**

> Panther’s web-based application for UI workflows. Customers can log in at \[customer-URL].runpanther.net.

#### **Panther Developer Workflows**

> Non-Panther Console workflows you can use to interact with your Panther account, including CI/CD, API, Panther Log Tool, and the Panther Analysis Tool (PAT).

#### Policy

> Panther's [Policies](../writing-detections/policies.md) are Python functions that scan and evaluate cloud infrastructure configurations to identify misconfigurations and subsequently generate alerts. Policies specifically apply to cloud resources, whereas [Rules](glossary.md#rule) apply to security logs.



#### Pretty print

> In JSON, pretty printing includes **** proper line breaks, indentation, white space, and overall structure.
>
> **Source:** [Datagy](https://datagy.io/python-pretty-print-json/)

### **R**

#### **RBAC (role-based access control)**

> An authorization method that assigns access based on user roles and user permissions.

#### Rule

> Panther's [Rules](../writing-detections/rules.md) are Python functions for detecting suspicious security log activity and generating alerts.
>
> * Real-time rules, simply known as Rules, are used to analyze a point-in-time (single log)
> * [Scheduled rules](glossary.md#scheduled-rule) are used to analyze aggregated or statistical data sets (many logs)

### **S**

#### **Scheduled Rule**

> Panther's [Scheduled Rules](../data-analytics/scheduled-queries.md#create-a-scheduled-rule) are a detection executed against a [Scheduled Query](glossary.md#scheduled-query) data set and are used to investigate smaller subsets of data.

#### **Scheduled Query**

> Panther's time-based SQL queries that allow you to pull data on a recurring basis. A Scheduled Query is always associated with at least one [Scheduled Rule](glossary.md#scheduled-rule).

#### **Schema**

> * Schemas inform Panther of how to normalize data for downstream services like the detection engine and tables in the data lake.

#### **Security Data Lake**

> Also known as a data lake or SDL. A centralized repository aimed at maintaining and managing all log or other data sources relevant to an organization’s security posture. An SDL can ingest data from myriad sources and can integrate with other security analytics tools to provide a single place for security data to be housed, searched**,** and utilized.

#### **SIEM (security information and event manager)**

> A SIEM collects, stores, and analyzes security data across broad networks and data sources, allowing organizations to detect and respond to escalating threats.

#### **Snowflake**

> A cloud-based Data Warehouse [that integrates with Panther](https://www.snowflake.com/news/snowflake-launches-new-cybersecurity-workload-to-detect-and-respond-to-threats-with-the-data-cloud/) to provide unified, secure, and scalable security capabilities so businesses can eliminate blind spots and respond to threats at cloud-scale.

#### **SOAR (security orchestration, automation, and response)**

> A collection of security management solutions that combine threat management with incident response and automated security operations.

#### **SOC (security operations center)**

> Pronounced “sock.” A team of IT professionals tasked to monitor, analyze, and respond to security threats

#### **SSO (single sign-on)**

> An authentication process that allows a user to log in with one ID credential to access multiple separate and independent applications and services.

### **X**

#### **XDR (extended detection and response)**

> A consolidation of data tools to give extended visibility, analysis, and response across multiple applications.
