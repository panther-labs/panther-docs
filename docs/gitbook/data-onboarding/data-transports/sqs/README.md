---
description: Onboarding SQS Logs as a Data Transport log source in the Panther Console
---

# SQS Source

{% hint style="info" %}
SQS has a max message size is 256KB a scalability limit of 10K SQS messages per minute. If you're expecting to send messages bigger than this, consider using S3 instead.
{% endhint %}

The steps below will setup an SQS queue and will give you permissions to send data to that queue. Panther will pull events from that queue and will allow you to write rules and run queries on the processed data.

## Step 1: Choose SQS source

Log in to your Panther Console and click Integrations on the left sidebar menu. Click **Log Sources** > **Add Source** > **Data Transport** > **SQS Queue**

![](<../../../../../.gitbook/assets/image (4) (1) (1).png>)

### Step 2: Enter the source details

|           Field          | Required? | Description                                                                                                                                                                             |
| :----------------------: | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|          `Name`          | `Yes`     | Friendly name of the source                                                                                                                                                             |
|        `Log Types`       | `Yes`     | The list of Log Types that you are going to send to this SQS queue                                                                                                                      |
| `Allowed Principal ARNs` | `No`      | The ARNs of the AWS principals that will be allowed to publish messages to the SQS queue e.g. `arn:aws:iam::012345678912:root` or `arn:aws:iam::012345678912:role/Test-*`               |
|   `Allowed Source ARNs`  | `No`      | The ARNs of the AWS resources (S3 buckets, SNS topics, etc) that can publish messages to that SQS queue e.g. `arn:aws:sns:us-east-1:012345678912:my-topic` or `arn:aws:s3:::my-bucket*` |

![](<../../../../../.gitbook/assets/sqs-page2 (5) (5) (7) (7) (1) (1) (3) (1) (1) (1) (2) (6).png>)

Note that if none of **Allowed Principal ARNs** and **Allowed Source ARNs** properties are set, only Principals of the AWS account where Panther is deployed will be able to publish messages to the queue. Click **Continue Setup**.

### Step 2: Create the SQS queue

Click **Save Source**. Panther will create an SQS queue and will allow the ARNs specified above to publish messages to it. The SQS queue URL will be display in the next page

![](<../../../../../.gitbook/assets/sqs-page3 (5) (5) (7) (8) (1) (1) (3) (1) (1) (1) (2) (6).png>)

You are all done! You can now start sending messages to the SQS queue.
