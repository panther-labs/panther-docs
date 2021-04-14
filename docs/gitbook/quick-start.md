---
description: Get started with Panther in 15 minutes
---

# Quick Start



{% hint style="info" %}
Get started with Panther by scheduleding a [demo](https://runpanther.io/request-a-demo/)!
{% endhint %}

## First Login

Once your account has been provisioned, you will get an invitation email from `no-reply@verificationemail.com` with your temporary login credentials. If you don't see it, be sure to check your spam folder.

You can use these temporary credentials to login and setup MFA:

![Login Screen](.gitbook/assets/quick-start-login.png)

### Certificate Warning

{% hint style="warning" %}
By default, Panther generates a self-signed certificate for the web UI, which will cause most browsers to present a warning page:

![Self-Signed Certificate Warning](.gitbook/assets/quick-start-cert-warning%20%289%29%20%284%29%20%285%29.png)

Your connection _is_ encrypted, and it's generally safe to continue. However, the warning exists because self-signed certificates do not protect you from man-in-the-middle attacks; for this reason production deployments should provide their own `CertificateArn` parameter value.
{% endhint %}

## Data Onboarding

Congratulations! You are now ready to use Panther. Follow the steps below to start analyzing data:

1. Invite your team in `Settings` &gt; `Users` &gt; `Invite User`
2. Configure [destinations]() to receive generated alerts
3. Onboard data for real-time log analysis from [S3]() or [SQS]()
4. Write custom [rules]() based on internal business logic
5. Onboard AWS accounts for [cloud security scans]()
6. Write custom [policies]() for supported [AWS resources]()
7. Enterprise Only: Query collected logs with [data explorer]()

## Supported AWS Regions

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

At this time, only the backend processing can be deployed to China \(no web app\). See our [China docs]() for more details.

