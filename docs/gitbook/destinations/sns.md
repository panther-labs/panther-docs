# Amazon SNS

This page will walk you through configuring SNS as a Destination for your Panther alerts. This is a simple way to send notifications to email addresses, multiple queues, and more.

The SNS Destination requires a `Topic ARN`. When an alert is forwarded to an SNS Destination, it publishes a JSON string to that topic:

![](<../../../.gitbook/assets/sns-panther (7) (7) (7) (1) (8).png>)

From the AWS [SNS console](https://us-west-2.console.aws.amazon.com/sns/v3/home#/topics), create a new Topic or navigate to the topic you wish to add as a destination. Copy the ARN out and into the Panther Destinations configuration, then select the topic. We will be editing its permissions so Panther can publish messages to it:

![](<../../../.gitbook/assets/sns1 (7) (1) (1) (14).png>)

After selecting the SNS topic, select the `Edit` button then scroll down and expand the `Access policy` section:

![](<../../../.gitbook/assets/sns2 (8) (1) (1) (13).png>)

After expanding the `Access policy` section, add the following statement to the `Statement` block:

```javascript
    {
      "Sid": "AllowPantherAlarming",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<PANTHER-ACCOUNT-ID>:root"
      },
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:us-west-2:123456789012:example-topic"
    }
```

{% hint style="warning" %}
Be sure to replace the `Resource` field with the ARN of your own SNS Topic, and the Principal with the AWS account ID where Panther is deployed.
{% endhint %}

When specifying the `Principal`, you can find the account ID where Panther is running in the `Settings -> General` page of your Panther deployment:

![](<../../../.gitbook/assets/sqs3 (9) (4) (1) (16).png>)

Enter this account ID as part of the `principal`:

![](<../../../.gitbook/assets/sns3 (9) (1) (14).png>)

1. Select the `Save changes` button to confirm your changes, and your SNS Topic will now be able to receive Panther alerts

If your goal is to set up email notifications with this topic, continue below:

1. Click `Create Subscription` on the topic you just created
2. The topic ARN should match the topic you created
3. Select `Email` in the protocol dropdown menu and enter the email address you would like to receive alerts to
4. Click `Create subscription`
5. You must confirm the subscription sent to your email before receiving alerts from this topic

![](<../../../.gitbook/assets/image (12).png>)
