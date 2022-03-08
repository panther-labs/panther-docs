---
description: Get started with Panther in 15 minutes
---

# Quick Start

{% hint style="info" %}
Get started with Panther by scheduling a [demo](https://runpanther.io/request-a-demo/)!
{% endhint %}

Welcome to Panther!

First things first, let's get you get logged in to the Panther Console. See the guidelines below to get credentials and set up your Panther account.  Then you can get started with adding integrations and more.

### Initial Login

Once your account has been provisioned, you will receive an invitation email from `no-reply@verificationemail.com` with your temporary Panther Console login credentials. If you don't see it, be sure to check your spam folder or reach out to your customer support team.

After the initial console login with the provided credentials, you will need to update your password and set up MFA.

![Login Screen](<.gitbook/assets/image (40).png>)

### Inviting Users

After you have successfully logged in, you can invite more users to the platform by navigating to **Settings** > **Users**. You may also set up [SAML integration](system-configuration/saml/).

![](<.gitbook/assets/image (43).png>)

### Alert Destinations

The first recommended step after initial login to the Panther Console is to configure [destinations](https://docs.runpanther.io/destinations) to receive alerts in notification systems such as [Slack](https://docs.runpanther.io/destinations/slack), [PagerDuty](https://docs.runpanther.io/destinations/pagerduty), or automation platforms like Tines with a [custom webhook](https://docs.runpanther.io/destinations/custom\_webhook). You can quickly set up a destination by following the steps below:

1. In the Panther Console, go to **Integrations > Alert Destinations.**
2. Click **+Add your first Destination**.
3. Click a destination you would like to configure:\
   ![](.gitbook/assets/destination-options.png)

See [Panther's Destinations documentation](https://docs.runpanther.io/destinations) for configuration steps specific to each service.

### Data Onboarding

For data normalization, real-time analytics, and storage into the data lake, start by onboarding data through one (or multiple) of the transport methods below:

* [Amazon S3](https://docs.runpanther.io/data-onboarding/data-transports/s3)
* [AWS SQS](https://docs.runpanther.io/data-onboarding/data-transports/sqs)
* [CloudWatch Logs](https://docs.runpanther.io/data-onboarding/data-transports/cwl-source)
* [Google Cloud Storage](https://docs.runpanther.io/data-onboarding/data-transports/gcs)
* Directly pulled from various [SaaS Services](https://docs.runpanther.io/data-onboarding/saas-logs)

After onboarding, your data will be searchable with SQL via the [Data Explorer](https://docs.runpanther.io/data-analytics/data-explorer) and can be correlated with the [Indicator Search](https://docs.runpanther.io/data-analytics/indicator-search). These tools can help provide samples for [custom rule](https://docs.runpanther.io/writing-detections/rules) writing.

### Set Up Detections and Cloud Compliance

Panther comes with build-in detections that alert against common security events and monitoring of cloud infrastructure. Building on these built-on detections is easy; use Panther to create custom detections that address your organizational needs. Use the documentation below to guide you through setting up detections and cloud compliance:

* Onboard AWS accounts for [cloud security scans](https://docs.runpanther.io/data-onboarding/setup-cloud-accounts).
* Write [rules](https://docs.runpanther.io/writing-detections/rules) based on internal business logic or monitoring needs.
* Write [policies](https://docs.runpanther.io/writing-detections/policies) for supported [AWS resource types](https://docs.runpanther.io/resources).
