---
description: Get started with Panther in 15 minutes
---

# Quick Start

{% hint style="info" %}
Get started with Panther by scheduling a [demo](https://runpanther.io/request-a-demo/)!
{% endhint %}

Welcome to Panther!

First things first, let's make sure you get logged in. See the guidelines below to get credentials and set up your Panther account. The first recommended step after initial login is to configure [destinations](destinations/README.md) to receive alerts in notification systems such as Slack, PagerDuty, or automation platforms like Tines.

Then, depending on the desired use case, follow the onboarding paths below:

### First Login

Once your account has been provisioned, you will get an invitation email from `no-reply@verificationemail.com` with your temporary login credentials. If you don't see it, be sure to check your spam folder.

You can use these temporary credentials to login and setup MFA:

![Login Screen](.gitbook/assets/quick-start-login.png)

{% hint style="warning" %}
By default, Panther generates a self-signed certificate for the web UI, which will cause most browsers to present a warning page:

![Self-Signed Certificate Warning](.gitbook/assets/quick-start-cert-warning%20%289%29%20%284%29%20%285%29.png)

Your connection _is_ encrypted, and it's generally safe to continue. However, the warning exists because self-signed certificates do not protect you from man-in-the-middle attacks; for this reason production deployments should provide their own `CertificateArn` parameter value.
{% endhint %}

### Alert Destinations

The first recommended step after initial login is to configure [destinations](destinations/README.md) to receive alerts in notification systems such as Slack, PagerDuty, or automation platforms like Tines. You can quickly set up a destination by following the steps below:

1. Go to  `Integrations` &gt; `Alert Destinations` &gt; `Add New Destination`
2. Select a destination you would like to push alert to
3. Follow the onboarding experience, use Panther Docs to correctly configure the destination

### Data Onboarding

For data normalization, real-time analytics, and storage into the data lake, start by onboarding data through one \(or multiple\) of the transport methods below:

* [Amazon S3](log-analysis/setup/s3.md)
* [AWS SQS](log-analysis/setup/sqs.md)
* Directly pulled from various [SaaS Services](data-onboarding/saas-logs)

After onboarding, your data will be searchable with SQL via the [Data Explorer](data-analytics/indicator-search) and can be correlated with the [Indicator Search](data-analytics/data-explorer). These tools can help provide samples for [custom rule](logs-analysis/rules/README.md) writing.

### Set Up Detections & Cloud Compliance

Panther comes with build-in detections that alert against common security events and monitoring of cloud infrastructure. Building on these built-on detections is easy; use Panther to create custom detections that address your organizational needs. Use the documentation below to guide you through setting up detections and cloud compliance:

1. Onboard AWS accounts for [cloud security scans](cloud-security/setup-cloud-accounts.md)
2. Write [rules](log-analysis/rules/README.md) based on internal business logic or monitoring needs
3. Write [policies](cloud-security/policies/README.md) for supported [AWS resource types](cloud-security/resources/README.md)

### Supported AWS Regions

Panther can be deployed to any of the following regions:

* `ap-northeast-1` \(Tokyo\)
* `ap-northeast-2` \(Seoul\)
* `ap-south-1` \(Mumbai\)
* `ap-southeast-1` \(Singapore\)
* `ap-southeast-2` \(Sydney\)
* `ca-central-1` \(Canada\)
* `eu-central-1` \(Frankfurt\)
* `eu-north-1` \(Stockholm\)
* `eu-west-1` \(Ireland\)
* `eu-west-2` \(London\)
* `eu-west-3` \(Paris\)
* `sa-east-1` \(São Paulo\)
* `us-east-1` \(N. Virginia\)
* `us-east-2` \(Ohio\)
* `us-west-1` \(N. California\)
* `us-west-2` \(Oregon\)

Panther also has limited support for AWS China:

* `cn-north-1` \(Beijing\)
* `cn-northwest-1` \(Ningxia\)

At this time, only the backend processing can be deployed to China \(no web app\). See our [China docs](https://docs.runpanther.io/help/china) for more details.

