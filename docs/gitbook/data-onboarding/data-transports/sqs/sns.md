# SNS source

## SNS Source Setup

Following the steps below, you will be able to onboard into Panther data that are sent to your SNS topics. You will be able to configure your SNS topics to send data to an SQS queue managed for you by Panther.

## Step 1: Create SQS source in Panther

Follow the guide on [setting up an SQS source](./). Make sure to add to the list of `Allowed Source ARNs` the ARN of your SNS topic.

Keep a note of the SQS Queue URL that Panther created for you!

## Step 2: Create SNS subscription to SQS

In this step, you will configure your SNS topic to forward messages to the SQS queue. To do that:&#x20;

1\. Log in to the AWS account that contains your SNS topic&#x20;

2\. Go to SNS service&#x20;

3\. Select your SNS topic. Click on **Create Subscription**&#x20;

4\. Leave the **Topic ARN** field as is.&#x20;

5\. In the next screen, set **Protocol** to **Amazon SQS**&#x20;

6\. In **Endpoint** add the ARN of the SQS queue that Panther created for you in Step 1. You can infer the SQS queue ARN by the URL knowing the following:

* An SQS queue ARN is in the format `arn:aws:sqs:<region>:<account id>:<queue name>`&#x20;
*   An SQS queue URL is in the format `https://sqs.<region>.amazonaws.com/<account id>/<queue name>`

    So for example a queue with URL `https://sqs.eu-central-1.amazonaws.com/123456789012/panther-source-test` has ARN `arn:aws:sqs:eu-central-1:123456789012:panther-source-test`
* Select **Enable raw message delivery**
*   Click on **Create subscription**

    ![](<../../../../../.gitbook/assets/sns-page1 (5) (5) (7) (8) (1) (1) (3) (6).png>)

You are done! Panther will start processing any message sent to the SNS topic
