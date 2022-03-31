# Cloud Accounts

## Overview

Panther's Cloud Security works by scanning AWS accounts, modeling the **Resources** within them, and using **Policies** to detect misconfigurations.

This feature can be used to power your compliance and improve your cloud security posture. Common security misconfigurations detectable by Panther include:

* S3 Buckets without encryption
* Security Groups allowing inbound SSH traffic from `0.0.0.0/0`
* Access Keys being older than 90 days
* IAM policies that are too permissive

When adding a new AWS account, Panther runs a baseline scan and models all of the resources in your account. Account scans are performed _daily_ to ensure the most consistent state possible. This works by using an assumable IAM Role with ReadOnly permissions.

{% hint style="info" %}
Resource updates can be configured to track changes in real-time using CloudTrail or CloudWatch Events.
{% endhint %}

****
