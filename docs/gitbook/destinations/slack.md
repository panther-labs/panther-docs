---
description: A walkthrough on how to configure Slack as an alert destination
---

# Slack

## Step 1: Slack App Setup

Slack can be connected to Panther by creating a custom Slack app with a webhook:

Navigate to [Your Slack Apps](https://api.slack.com/apps), and select `Create New App` to create a custom app

![](<../../../.gitbook/assets/slack1 (9) (7) (1) (1) (1) (11) (13).png>)

After creating the app, an administrator will need to authorize its access and enable it to the appropriate channel.

![](<../../../.gitbook/assets/slack2 (13) (7) (1) (1) (1) (11) (14).png>)

Click `Incoming Webhooks`, then enable `Activate Incoming Webhooks`

![](<../../../.gitbook/assets/slack3 (13) (6) (1) (1) (1) (11) (14).png>)

Scroll down and click `Add New Webhook to Workspace`

![](<../../../.gitbook/assets/slack4 (12) (6) (1) (1) (11) (13).png>)

Copy the generated `Webhook URL`to use in the next step. You should also see a message in the connected Slack channel indicating the integration was added.

Your Slack destination is now ready to receive alerts.

## Step 2: Add Destination to Panther

Paste the copied `Slack Webhook URL` into the Panther Destination configuration settings:

![](<../../../.gitbook/assets/slack-panther (7) (5) (1) (1) (3) (8).png>)
