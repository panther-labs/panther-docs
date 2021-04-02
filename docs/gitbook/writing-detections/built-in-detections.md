# Built-in Detections

By default, detections are pre-installed from the open source [panther-analysis](https://github.com/panther-labs/panther-analysis) repository to establish a strong baseline.

Detections are grouped by log or resource type and will automatically take effect once data is onboarded.

Rules:

* AWS CloudTrail
* AWS GuardDuty
* AWS S3 Server
* AWS VPC Flow
* Cisco Umbrella
* GCP Audit
* Gravitational Teleport
* G Suite
* Okta
* OneLogin
* Osquery

Policies:

* AWS Accounts
* AWS ACM
* AWS CloudFormation Stacks
* AWS CloudTrail
* AWS CloudWatch
* AWS Config
* AWS DynamoDB
* AWS EC2
* AWS ELB
* AWS GuardDuty
* AWS IAM
* AWS KMS
* AWS Load Balancer
* AWS RDS
* AWS Redshift
* AWS S3
* AWS VPC

{% hint style="info" %}
Some detections require specific settings to best work in your environment and are disabled by default.

Search for the "Configuration Required" tag to set the proper context before enabling the detection.
{% endhint %}

## Severities

There are many standards on what different severity levels should mean, and at Panther, we base our severities on this table:

| Severity | Exploitability | Description | Examples |
| :--- | :--- | :--- | :--- |
| `Info` | `None` | No risk, simply informational | Name formatting, missing tags. General best practices for ops. |
| `Low` | `Difficult` | Little to no risk if exploited | Non sensitive information leaking such as system time and OS versions. |
| `Medium` | `Difficult` | Moderate risk if exploited | Expired credentials, missing protection against accidental data loss, encryption settings, best practice settings for audit tools. |
| `High` | `Moderate` | Very damaging if exploited | Large gaps in visibility, directly vulnerable infrastructure, misconfigurations directly related to data exposure. |
| `Critical` | `Easy` | Causes extreme damage if exploited | Public data or systems, leaked access keys. |

Use this as a reference point to create your own standards.

## Rules and Policies

Follow the links below to learn more about the anatomy of built-in detections:

* [Writing Rules for log events](rules.md)
* [Writing Policies for cloud security resources](policies.md)

