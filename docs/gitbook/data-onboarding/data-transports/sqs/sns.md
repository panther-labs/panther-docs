---
description: Onboarding SNS Logs as a Data Transport log source in the Panther Console
---

# SNS Source

## Overview

The steps below enable you to onboard an SNS source into Panther. You will be able to configure your SNS topics to send data to an [SQS queue](./) managed by Panther.

## How to create an SNS source in Panther

### Step 1: Set up your SQS source

Follow the guidance on [setting up an SQS source](./), making sure to add the ARN of your SNS topic to the list of `Allowed Source ARNs`.

{% hint style="info" %}
Keep a note of the SQS Queue URL that Panther creates for your SQS Source.
{% endhint %}

### Step 2: Create SNS subscription to SQS queue&#x20;

1. Log in to the AWS account that contains your SNS topic.
2. Go to SNS Service.
3. Select your SNS topic.&#x20;
   * A user who does not own the queue must create a subscription.
4. Click on **Create Subscription.**
5. Fill in the Details section.
   * Make sure to select **Enable Raw Delivery**.
   * For the ARN in Endpoint, use this format: `arn:aws:sqs:<region>:<account id>:<queue name>`.
6. When the subscription has been created, navigate back to your Panther Console to see your SNS topic messages begin to populate.
