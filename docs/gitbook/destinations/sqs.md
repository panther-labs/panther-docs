# Amazon SQS

## Step 1: SQS Queue Setup

An SQS Queue can be connected to Panther by creating a queue and granting Panther permission to send to it:

Navigate to the AWS [SQS Console](https://console.aws.amazon.com/sqs/home) and select `Create New Queue` to create a new queue, then set the name of the new queue.

![](<../../../.gitbook/assets/sqs1 (9) (2) (6).png>)

In the `Access Policy` section of the new queue Basic setup, under the `Define who can send messages to the queue` heading, select `Only the specified AWS Accounts, IAM users and role` radio button.

![](<../../../.gitbook/assets/sqs2 (9) (3) (9).png>)

You will need to enter the AWS account of your Panther Console. You can find this in the `Settings -> General` page of your Panther Console:

![](<../../../.gitbook/assets/sqs3 (9) (4) (21).png>)

Save the new queue by clicking on the Save button at the bottom of the AWS `Create queue` page.

## Step 2: Add Destination to Panther

The SQS Queue will have a `URL` field under the `Details` tab. Paste the copied URL into the Panther Destination configuration settings:

![](<../../../.gitbook/assets/sqs-panther (7) (7) (4) (9).png>)
