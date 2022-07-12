---
description: Get started with your new Panther account
---

# Quick Start

{% hint style="info" %}
Get started with Panther by scheduling a [demo](https://runpanther.io/request-a-demo/)!
{% endhint %}

Welcome to Panther!

This guide will walk you through:

* Your initial login and how to invite other users to your Panther Console
* Setting up a destination where you will receive alerts&#x20;
* Onboarding logs you want to monitor
* Setting up detections to alert you against common security threats

## Overview

### Overview Video

{% embed url="https://panther.wistia.com/medias/vxn8h7c6fq" %}
Tour of getting started in the Panther Console
{% endembed %}

### Using Panther

You can manage your account and workflows in the Panther Console or using Panther Developer Workflows.

#### Panther Console

The Panther Console is Panther's web-based UI where Panther admins manage their account.&#x20;

#### Panther Developer Workflows

Panther Developer Workflows are non-console workflows you can use to interact with your Panther account, including CI/CD, API, and the Panther Analysis Tool (PAT).&#x20;

### Glossary

Panther's [Glossary](help/glossary.md) introduces common cloud-native, security, and Panther-specific terminology. Refer to the Glossary for extra context and clarity on terms found throughout the Documentation site.

## Getting Started in Panther

### Initial Login

To access your Panther Console, you need an instance. An instance is created when the Panther team provisions your account.

Once your account has been provisioned, you will receive an invitation email from `no-reply@verificationemail.com` with your temporary Panther Console login credentials. If you don't see it, be sure to check your spam folder or reach out to your customer support team.

After the initial console login with the provided credentials, you will need to update your password and set up MFA.

![Login Screen](<.gitbook/assets/image (40).png>)

### Inviting Users

After you have successfully logged in, you can invite more users to the platform by navigating to **Settings** > **Users**. You may also set up [SAML integration](system-configuration/saml/).

![](<.gitbook/assets/image (43) (1).png>)

**We strongly recommend having at least two users with** [**Admin role**](https://docs.runpanther.io/system-configuration/rbac) **set up.** This will help your organization regain access to the Panther Console if needed. ****&#x20;

It is also recommended to routinely audit the users who have access to your Panther Console.

### Alert Destinations

The first recommended step after initial login to the Panther Console is to configure [destinations](https://docs.runpanther.io/destinations) to receive alerts in notification systems such as [Slack](https://docs.runpanther.io/destinations/slack), [PagerDuty](https://docs.runpanther.io/destinations/pagerduty), or automation platforms like Tines with a [custom webhook](https://docs.runpanther.io/destinations/custom\_webhook). You can quickly set up a destination by following the steps below:

1. In the Panther Console, go to **Integrations > Alert Destinations.**
2. Click **+Add your first Destination**.
3. Click a destination you would like to configure:\
   ![](.gitbook/assets/destination-options.png)

See [Panther's Destinations documentation](https://docs.runpanther.io/destinations) for configuration steps specific to each service.

### Data Onboarding

Next up is to onboard data sources for data normalization, which will also allow you to query the logs in the data lake and perform real-time analysis with Python.

This Quick Start guide provides the general steps required to onboard data. To view instructions for specific integrations, please see the [Data Onboarding](https://docs.runpanther.io/data-onboarding) documentation.

#### Create a new log source

To start onboarding data, navigate to **Integrations** > **Log Sources** and click **Onboard your logs**.&#x20;

Here you will be able to choose from a list of services we currently support (or select **Custom Onboarding** on the left to view the available transport methods.)

![](<.gitbook/assets/image (1).png>)

The most common data source methods are ingesting data from an Amazon S3 bucket or directly pulling the logs from a supported SaaS service. For more information on each transport method visit the links below:&#x20;

* [Amazon S3](https://docs.runpanther.io/data-onboarding/data-transports/s3)
* [AWS SQS](https://docs.runpanther.io/data-onboarding/data-transports/sqs)
* [CloudWatch Logs](https://docs.runpanther.io/data-onboarding/data-transports/cwl-source)
* [Google Cloud Storage](https://docs.runpanther.io/data-onboarding/data-transports/gcs)
* Directly pulled from various [SaaS Services](https://docs.runpanther.io/data-onboarding/saas-logs)

{% hint style="info" %}
If the log source you want to ingest is not natively supported yet, visit the [Custom Log Types](data-onboarding/custom-log-types/) documentation to upload logs and infer a schema.
{% endhint %}

After following the onboarding steps, your data will begin to be ingested into Panther. Your logs will be checked against the built-in Python detections and will be searchable within the [Data Explorer](https://docs.runpanther.io/data-analytics/data-explorer). You can now query [Indicator Search](https://docs.runpanther.io/data-analytics/indicator-search) for investigations on common indicators for your various data sources.

![](<.gitbook/assets/image (6) (1) (1).png>)

### Set Up Detections and Cloud Compliance

Panther comes with built-in detections that alert against common security events and monitoring of cloud infrastructure. Building on these built-in detections is easy; use Panther to create custom detections that address your organizational needs. Use the documentation below to guide you through setting up detections and cloud compliance:

* Onboard AWS accounts for [cloud security scans](https://docs.panther.com/cloud-scanning).
* Write [rules](https://docs.runpanther.io/writing-detections/rules) based on internal business logic or monitoring needs.
* Write [policies](https://docs.runpanther.io/writing-detections/policies) for supported [AWS resource types](https://docs.panther.com/cloud-scanning/cloud-resource-attributes).
