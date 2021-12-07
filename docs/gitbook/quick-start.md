---
description: Get started with Panther in 15 minutes
---

# Quick Start

{% hint style="info" %}
Get started with Panther by scheduling a [demo](https://runpanther.io/request-a-demo/)!
{% endhint %}

Welcome to Panther!

First things first, let's get you get logged in. See the guidelines below to get credentials and set up your Panther account.&#x20;

Then, depending on the desired use case, follow the onboarding paths below:

### First Login

Once your account has been provisioned, you will get an invitation email from `no-reply@verificationemail.com` with your temporary login credentials. If you don't see it, be sure to check your spam folder.

You can use these temporary credentials to login and setup MFA:

![Login Screen](../../.gitbook/assets/quick-start-login.png)

### Alert Destinations

The first recommended step after initial login is to configure [destinations](https://docs.runpanther.io/destinations) to receive alerts in notification systems such as Slack, PagerDuty, or automation platforms like Tines. You can quickly set up a destination by following the steps below:

1. Go to  `Integrations` > `Alert Destinations` > `Add New Destination`
2. Select a destination you would like to push alert to
3. Follow the onboarding experience, use Panther Docs to correctly configure the destination

### Data Onboarding

For data normalization, real-time analytics, and storage into the data lake, start by onboarding data through one (or multiple) of the transport methods below:

* [Amazon S3](https://docs.runpanther.io/data-onboarding/data-transports/s3)
* [AWS SQS](https://docs.runpanther.io/data-onboarding/data-transports/sqs)
* Directly pulled from various [SaaS Services](https://docs.runpanther.io/data-onboarding/saas-logs)

After onboarding, your data will be searchable with SQL via the [Data Explorer](https://docs.runpanther.io/data-analytics/data-explorer) and can be correlated with the [Indicator Search](https://docs.runpanther.io/data-analytics/indicator-search). These tools can help provide samples for [custom rule](https://docs.runpanther.io/writing-detections/rules) writing.

### Set Up Detections & Cloud Compliance

Panther comes with build-in detections that alert against common security events and monitoring of cloud infrastructure. Building on these built-on detections is easy; use Panther to create custom detections that address your organizational needs. Use the documentation below to guide you through setting up detections and cloud compliance:

1. Onboard AWS accounts for [cloud security scans](https://docs.runpanther.io/data-onboarding/setup-cloud-accounts)
2. Write [rules](https://docs.runpanther.io/writing-detections/rules) based on internal business logic or monitoring needs
3. Write [policies](https://docs.runpanther.io/writing-detections/policies) for supported [AWS resource types](https://docs.runpanther.io/resources)

### Supported AWS Regions

Panther can be deployed to any of the following regions:

* `ap-northeast-1` (Tokyo)
* `ap-south-1` (Mumbai)
* `ap-southeast-1` (Singapore)
* `ap-southeast-2` (Sydney)
* `ca-central-1` (Canada)
* `eu-central-1` (Frankfurt)
* `eu-west-1` (Ireland)
* `eu-west-2` (London)
* `us-east-1` (N. Virginia)
* `us-east-2` (Ohio)
* `us-west-2` (Oregon)
