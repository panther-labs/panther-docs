# Data Onboarding

Panther offers several options to onboard data, including SaaS Logs, Data Transports, Cloud Accounts, and Custom Log Types. You can also [request support for a specific log source](https://docs.runpanther.io/data-onboarding#request).&#x20;

### SaaS Logs

Panther leverages two mechanisms to pull logs from SaaS vendors:&#x20;

* Direct integrations (by querying APIs)
* AWS EventBridge

For a list of vendors currently supported, see [SaaS Logs](saas-logs/).

### Data Transports

You may leverage AWS Services in tandem with Panther to get data such as S3 buckets, CloudWatch, SQS, or SNS.

For more information, see [Data Transports](data-transports/).

### Cloud Accounts

Onboard your AWS account to allow Panther to scan its resources and check for potential vulnerabilities. For more information, see [Cloud Accounts](https://docs.panther.com/cloud-scanning).

In addition to onboarding AWS accounts for Cloud Scanning, we also advise you to onboard the same AWS account as a log source so you can configure Detections and receive alerts for active incidents and breaches. For more information, see [Built-in Log Types > AWS](supported-logs/aws.md).

### Custom Log Types

Do you have a log type you would like to monitor that Panther does not have [schema built](https://docs.runpanther.io/data-onboarding/supported-logs) for? Panther gives you the ability to generate a custom schema, which informs Panther how to parse events correctly.

For more information, see [Custom Log Types](custom-log-types/).

### Request support for a log source <a href="#request" id="request"></a>

If you do not see the log source you want in the list at **Integrations > Log Sources**, you can request support of a new log source:

1. Log in to your Panther Console.
2. Navigate to **Integrations > Log Sources**.
3. Scroll to the bottom of the page and click **Request it here**.
4. Fill in the form then click **Create Request**.

![](../.gitbook/assets/request-log-source.png)
