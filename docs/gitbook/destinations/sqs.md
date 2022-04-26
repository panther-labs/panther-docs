# Amazon SQS

This page will walk you through configuring Amazon Simple Queue Service (SQS) as a destination for your Panther alerts.&#x20;

### SQS Queue Setup

First, you need to create an SQS queue and grant Panther permission to send to it.&#x20;

1. Navigate to the [AWS SQS Console](https://console.aws.amazon.com/sqs/home) and click **Create New Queue** to create a new queue. In the **Name** field, enter a name of the new queue.
2. In the "Access Policy" section of your new queue's Basic setup options, under "Define who can send messages to the queue," select **Only the specified AWS Accounts, IAM users and roles**.\
   ![](<../../../.gitbook/assets/sqs2 (9) (3) (1) (1) (11) (1) (1) (17).png>)
3. In the field below "Only the specified AWS Accounts, IAM users and roles," enter the AWS Account ID from your Panther Console.&#x20;
   * You can find your account ID at the bottom of the **Settings > General** page in the Console.\
     ![](<../../../.gitbook/assets/sqs3 (9) (4) (1) (1) (11) (1) (1) (20).png>)
4. Click the **Save** button at the bottom of the SQS Create Queue page to save your new queue.

### Setting Up the Destination in Panther

Next, add your SQS Queue destination to Panther.

1. Log in to your Panther Console and navigate to **Integrations > Alert Destinations**.&#x20;
2. Click **+Add your first Destination**.&#x20;
   * If you have already created Destinations, click **+** in the upper right side of the page to add a new Destination.
3. Click **Amazon SQS** from the list of options.
4. Fill in the form to configure the SQS destination.
   * **Display Name**: Enter a descriptive name.
   * **Queue UR**L: Enter your SQS Queue URL. This can be found in the "Details" tab in your AWS SQS console.
5. Click **Add Destination**.
6. Click **Finish Setup** to complete your setup, or click **Send Test Alert** to test your setup.

