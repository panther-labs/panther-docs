---
description: >-
  Use Panther Detections to analyze data, run queries, and trigger alerts on
  suspicious behavior
---

# Writing Detections

## Overview

Panther has three core detection types that are all written in Python - **Rules, Scheduled Rules,** and **Policies:**

* **Rules**
  * Panther's Rules are Python functions for detecting suspicious log activity and generating alerts. Rules specifically apply to security logs. For more information, see [Rules](rules.md).
* **Scheduled Rules**&#x20;
  * Panther's Scheduled Rules are a specific type of detection that runs on query results against your data lake. For more information, see [Scheduled Rules](../data-analytics/scheduled-queries.md#create-a-scheduled-rule).
* **Policies**&#x20;
  * Panther's Policies are Python functions that scan and evaluate cloud infrastructure configurations to identify misconfigured infrastructure and subsequently generate alerts. Policies specifically apply to cloud resources. For more information, see [Policies](policies.md).

## Supported Workflows

Panther detections are created and managed using one of the following methods:

* **The Panther Console,** under Detections > All Detections.
  * These can be supplemented with Panther-managed detections from [Detection Packs](detection-packs.md).
* [Panther Developer Workflows](panther-analysis-tool.md) such as the [Panther Analysis Tool](panther-analysis-tool.md#using-the-panther-analysis-tool) (PAT).
  * Learn more about managing Panther detections using a Continuous Integration and Continuous Deployment workflow in our [CI/CD Guide](../guides/ci-cd-onboarding-guide.md).

{% hint style="warning" %}
**Note**: Managing Detections via the Panther Console is not recommended if you are already using a Git-Based workflow to manage and upload detections with Panther Analysis Tool. Managing detections via both methods simultaneously may result in unexpected behavior.
{% endhint %}

## How to write detections

### Writing detections in the Panther Console

1. Click **Detections > All Detections**.
2. Click **Create New**.
3. Select your detection type between a **Rule, Policy,** or **Scheduled Rule.**
4. Fill out the required Basic Info as follows:
   * **Enabled:** Toggle the button in the right-hand corner to "On."
   * **Name**: **** Enter a memorable name for your new detection.
   * **Severity**: **** Select a [detection severity](triaging-alerts.md#alert-severities) from the drop-down options.
   * The lower right-hand drop-down menu differs depending on the detection type you chose:
     * **Rule:** Select the applicable Log Types.
     * **Policy:** Select the applicable Resource Types.
     * **Scheduled Rule:** Select the [Scheduled Query](../data-analytics/scheduled-queries.md) or queries this rule should apply to.
   * **Unique ID** (optional)**:** Click the **pen icon** and enter a unique ID for your detection**.**&#x20;
5. Click the **Functions & Tests tab** and **write** a Python function to define your Detection in the Rule Function text editor.
   * For detection templates and examples, see the [panther\_analysis repo on Github](https://github.com/panther-labs/panther-analysis/tree/master/templates).
6. Click **{} Create Test** to run a test against the Detection you defined in the previous step.
7. Optional: Adjust your **Rule Settings** or add a report on **Report Mapping**.
8. Click **Save** in the upper right corner.
9. You will land on your detection's information page - click **Edit** in the upper right-hand corner to manage or update your detection.

### Writing detections with Panther Developer Workflows

The below instructions are high-level - for more information see [Panther Developer Workflows: Detections](panther-analysis-tool.md).

1. Using [Panther Analysis](https://github.com/panther-labs/panther-analysis), [**clone** the `panther-analysis` repository](https://github.com/panther-labs/panther-analysis#clone-the-repository).
2. ****[**Configure** your Python environment](https://github.com/panther-labs/panther-analysis#configure-your-python-environment).
3. ****[**Write** detections-as-code](https://github.com/panther-labs/panther-analysis#writing-detections) using Panther's [templates](https://github.com/panther-labs/panther-analysis/tree/master/templates).
4. For further testing, packaging, and deploying capabilities, **install our Python CLI** called **** the [Panther Analysis Tool](https://github.com/panther-labs/panther\_analysis\_tool#panther-analysis-tool).

## Panther Detection Features & Functions

### Detection Packs

Panther packs logically group and update detections via the Panther Console. Detection packs can group any number of Panther features including but not limited to detections, queries, global helpers, data models, or Lookup Tables. packs are defined in this open source repository: [`panther-labs/panther-analysis`](https://github.com/panther-labs/panther-analysis). For more information, see [Detection Packs](detection-packs.md).

### Panther Developer Workflows

Panther Developer Workflows are workflows that can be used outside of the Panther Console to interact with your Panther account, such as the Panther API, Panther Analysis Tool (PAT), Panther Log Tool, and CI/CD workflows. Specifically, the PAT can test and upload locally managed Detections and optionally integrate with a CI/CD setup. For more information, see [Panther Developer Workflows](panther-analysis-tool.md).

### Data Models

Panther's data models provide a way to configure a set of unified fields across all log types. Data models allow you to monitor particular fields across many log types at once, avoiding cumbersome and complex individual log monitoring. For more information, see [Data Models](data-models.md).

### Global Helper Functions

Panther supports the common programming pattern to extract repeated code into helper functions, via the `global` analysis type. Import global helper functions in your detections by inserting certain commands at the top of your analysis function body then calling the global function as if it were any other Python library. For more information, see [Global Helper Functions](globals.md).

### Caching

Panther examines events one-by-one and provides a way to cache results across invocations. To accommodate stateful checks, Panther rules can cache values by using built-in helper functions. For more information, see [Caching.](caching.md)

### Data Replay

Data Replay (Beta) allows rules to be tested against historical log data to preview the outcome of a rule before enabling it. Data Replay can simulate what types of alerts you are likely to receive before deploying the detection. For more information, see [Data Replay](rules/data-replay.md).

### Testing

Panther's detection testing ensures that detections behave as expected and generate alerts once deployed correctly. Test inputs are utilized to determine whether or not an alert will generate in order to promote reliability as code evolves and protect against regressions. For more information, see [Testing.](testing.md)

### Report Mapping

Panther supports the ability to map rules, policies, and scheduled rules to compliance frameworks for the purposes of tracking coverage against that framework. Reports can be mapped to your detection within the Detections > All Detections navigation section of the Panther Console. For more information, see [Report Mapping](report-mapping/).

* Panther supports the ability to map frameworks that track coverage against [MITRE ATT\&CK](https://attack.mitre.org/)®. This MITRE integration helps you visualize coverage, identify gaps, and report progress internally for your detections using Panther. For more information, see [MITRE ATT\&CK® Matrix](report-mapping/mitre-attack.md).

### Detection Auxiliary Functions

Panther's detection auxiliary functions are Python functions that control analysis logic, generated alert title, event grouping, routing of alerts, and metadata overrides. Applicable to both Rules and Policies, each function listed takes a single argument of `event` (Rules) or `resource` (Policies). Advanced users may define functions, variables, or classes outside of Panther's auxiliary functions. For more information, see [Detection Auxiliary Functions](detection-auxiliary-functions.md).&#x20;

### Alert Summaries

Panther's alert summaries instantly indicate `Who,` `What`, `Where` information when triaging matching events in a rule alert. Alert summaries parse large numbers of matching events and provide overview and threat level information so users avoid manually reviewing individual events. For more information, see [Alert Summaries.](alert-summaries.md)                                                                                                                                                        &#x20;

### Triaging Alerts

The Panther Console has a navigation section called **Alerts & Errors** where you can interpret and triage alerts. Triaging is grouped into three sub-sections: alerts, detection errors, or system errors. Triage label options include: open, invalid, resolved, or triaged. For more information, see [Triaging Alerts](triaging-alerts.md).

