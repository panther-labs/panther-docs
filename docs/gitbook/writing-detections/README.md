# Writing Detections

## Overview

### Detection Types

In Panther there are three core Detection types:

* **Real-Time Rules** that analyze data as soon as it's sent to Panther
* **Scheduled Rules** that run after a SQL query has been executed
* **Policies** that detect insecure cloud resources

Each detection type is written in Python and sends alerts to detect suspicious behavior or insecure infrastructure. Similar concepts are applied to each detection type and the docs in this section will outline those features.

### Supported Workflows

Panther detections can be created and managed in two different ways:

* In the Panther Console by navigating to **Detections** in the left-side navigation. These can be supplemented with Panther-managed detections from [Packs](https://docs.runpanther.io/writing-detections/detection-packs).
* Using CLI workflows with the [Panther Analysis Tool](https://docs.runpanther.io/writing-detections/panther-analysis-tool).
  * Learn more about managing Panther detections using a Continuous Integration and Continuous Deployment workflow in [our CI/CD Guide](https://docs.runpanther.io/guides/ci-cd-onboarding-guide).

We do not recommend using both the Console and Panther Analysis Tool to manage detections because of complexity and potential issues. We also do not recommend the use of Detection Packs by customers using CLI workflows.

## Alert Severities

There are many standards on what different severity levels should mean, and in Panther, we recommend severities on this table:

| Severity   | Exploitability | Description                        | Examples                                                                                                                           |
| ---------- | -------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `Info`     | `None`         | No risk, simply informational      | Gaining operational awareness.                                                                                                     |
| `Low`      | `Difficult`    | Little to no risk if exploited     | Non-sensitive information leaking such as system time and OS versions.                                                             |
| `Medium`   | `Difficult`    | Moderate risk if exploited         | Expired credentials, missing protection against accidental data loss, encryption settings, best practice settings for audit tools. |
| `High`     | `Moderate`     | Very damaging if exploited         | Large gaps in visibility, directly vulnerable infrastructure, misconfigurations directly related to data exposure.                 |
| `Critical` | `Easy`         | Causes extreme damage if exploited | Public data/systems available, leaked access keys.                                                                                 |

Use this as a reference point to create your own standards.

