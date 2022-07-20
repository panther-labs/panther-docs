---
description: Onboarding CloudWatch as a Data Transport log source in the Panther Console
---

# CloudWatch Logs Source

## Overview

Follow the steps below to enable secure access for Panther to pull security logs from CloudWatch.

## How to connect CloudWatch as a Data Transport log source

### Configure CloudWatch in the Panther Console

Follow the steps below to enable secure access for Panther to pull security logs from CloudWatch.

1. Log in to your Panther Console and click **Integrations** on the left sidebar menu.&#x20;
2. Click **Log Sources** > **Create New Source**.&#x20;
3. On the left side of the page, click **Custom Onboarding**. Click **Select** in the tile for AWS CloudWatch Logs.\
   ![](<../../.gitbook/assets/onboard-cloudwatch (2).png>)
4. On the "Configure your source" page, fill in the fields:
   * **Name**: Enter a Friendly name of the CloudWatch logs source
   * **Log Group Name**: Enter the unique name of the CloudWatch logs group
   * **AWS Account ID**: Enter the AWS Account ID number that the S3 bucket lives in
   * **Pattern Filter (optional)**: Use this field to filter data log data received from CloudWatch. Read more in [Amazon's documentation on filter and pattern syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html).&#x20;
   * **Log Types**: Select the Log Types Panther should use to parse CloudWatch logs. At least one Log Type must be selected from the dropdown menu.
5. Click **Continue**.

### Setup an IAM role

Panther needs an AWS IAM role with permissions to read objects from your CloudWatch log source.&#x20;

1. Choose a method set up the IAM role:
   * **Launch Console.**
     * You will be redirected to the AWS console with the template URL pre-filled
   * **Get Template**.&#x20;
     * Download the template and apply it through your own pipeline.
   * **Configure the role manually**.&#x20;
     *   Create the role manually or through your own automation, then fill in the role ARN in the Panther Console. Note, the IAM role policy must include at least the statements defined in the below policy:

         ```javascript
         {
             "Version": "2012-10-17",
             "Statement": [
                 {
                     "Action": ["s3:GetBucketLocation", "s3:ListBucket"],
                     "Resource": "arn:aws:s3:::<bucket-name>",
                     "Effect": "Allow"
                 },
                 {
                     "Action": "s3:GetObject",
                     "Resource": "arn:aws:s3:::<bucket-name>/*",
                     "Effect": "Allow"
                 }
             ]
         }
         ```
2. Optionally, Check the box next to _I want Panther to configure bucket notifications for me_ to allow Panther to configure bucket notifications automatically.&#x20;
   *   Panther uses [S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html) for notifications about new files added to your bucket. If you check the box, the provided CloudFormation template will add extra permissions to the IAM role, and Panther will configure bucket notifications automatically. Existing configurations will not be removed or overwritten. Otherwise, you will be prompted to configure bucket notifications manually, at a later step.

       We strongly suggest you allow Panther to configure bucket notifications, as it will help you monitor the health of the CloudWatch logs and surface issues through Panther's system health notifications.
3. When the IAM role is ready, fill in the Bucket Name and Role ARN.
   * After the CloudFormation stack creation is complete, you can find the role ARN in the "Outputs" section of the stack in AWS.
4. Click **Continue Setup**.&#x20;

{% hint style="info" %}
In order to enable [real-time processing of log data](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html), Panther will create a [Firehose Delivery Stream](https://aws.amazon.com/kinesis/data-firehose) and an S3 Bucket that will be used as the Delivery Stream's destination. A [subscription filter](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CreateSubscriptionFilterFirehose.html) is then configured for the Cloudwatch Logs log group using the Firehose Delivery Stream as its destination. The required read permissions for processing files added by Firehose to the newly created S3 bucket are granted to the IAM role.

More details on this process can be found in the [AWS Cloudwatch Logs documentation for subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CrossAccountSubscriptions-Firehose.html).
{% endhint %}



### Configure bucket notifications and finish source setup

If you have opted in for Panther-managed notifications in step 2, click **Finish Setup**. Your S3 source is ready to ingest data and a success page is shown. Optionally, you can configure a log drop-off alarm to be alerted if this source stops processing events within a specified time interval.

![](<../../.gitbook/assets/image (30).png>)

## Viewing Collected Logs

After log sources are configured, your data can be searched with [Data Explorer](../../data-analytics/data-explorer.md).
