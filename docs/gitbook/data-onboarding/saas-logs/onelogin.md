# OneLogin

Panther is able to process OneLogin events through [OneLogin's integration](https://www.onelogin.com/blog/aws-eventbridge-integration) with Amazon EventBridge. This allows Panther to process OneLogin logs in a scalable and reliable, low latency manner.

In order for Panther to process your OneLogin logs, you need to configure your OneLogin account to send data to Amazon EventBridge in Panther AWS account.

## Configure OneLogin to send data to Panther

First of all, you need to keep note of the AWS Account and AWS region where Panther is deployed. You can find this information from your Panther Console, going to **Settings** > **General,** in the footer of the page.

1. Log in to OneLogin Administration console
2. Go to **Developers** > **Webhooks**
3. Go to **New Webhook** > **Event Webhook for Amazon EventBridge**
4. Add a friendly name e.g. `Panther Integration`
5. Fill the AWS Account Id and Region that you noted earlier. Click **Save**
6.  Click on the new integration that got just created. Keep a note of the **Event Source** field as we are going to use it

    in the next step (it should be in the form `aws.partner/onelogin.com/US-123456/ffffffffff`)

## Create a new OneLogin source in Panther

1. Log in to your Panther Console.
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu.
3. Click **Add Source**.
4. Select **OneLogin**.
5. Select **Amazon EventBridge** from the list of available Data Transports if you would like to pull logs directly from OneLogin. You can also select S3 or SNS if you would like to retrieve logs from those sources.
6. In the following form, fill in the following fields:
   1. **Name**: A friendly name for the source e.g. `My OneLogin events`
   2. **Log Type**: Select `OneLogin`
   3. **Bus Name**: The field you noted in the previous text (in the form `aws.partner/onelogin.com/US-123456/ffffffffff`)
7. Click on **Next** and then **Save Source**
8. You are done! You can now start writing detections and exploring your OneLogin data.

## Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Active Login Activity
* Admin Role Assigned
* Brute Force By IP
* Brute Force By Username
* High Risk Failed Login
* High Risk Login
* Password Accessed
* Password Changed
* Remove Authentication Factor
* Threshold Accounts Deleted
* Threshold Accounts Modified
* Unauthorized Access
* Unusual Login
* User Account Locked
* User Assumed

Please take a look at the files in the [onelogin\_rules](https://github.com/panther-labs/panther-analysis/tree/master/onelogin\_rules) repository to see how these are built.&#x20;
