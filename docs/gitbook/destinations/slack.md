---
description: A walkthrough on how to configure Slack as an alert destination
---

# Slack

## Step 1: Slack App Setup

Slack can be connected to Panther by creating a custom Slack app with a webhook:

Navigate to [Your Slack Apps](https://api.slack.com/apps), and select `Create New App` to create a custom app

![](../.gitbook/assets/slack1%20%289%29%20%287%29%20%2810%29.png)

After creating the app, an administrator will need to authorize its access and enable it to the appropriate channel.

![](../.gitbook/assets/slack2%20%2813%29%20%287%29.png)

Click `Incoming Webhooks`, then enable `Activate Incoming Webhooks`

![](../.gitbook/assets/slack3%20%2813%29%20%286%29%20%285%29.png)

Scroll down and click `Add New Webhook to Workspace`

![](../.gitbook/assets/slack4%20%2812%29%20%286%29.png)

Copy the generated `Webhook URL`to use in the next step. You should also see a message in the connected Slack channel indicating the integration was added.

Your Slack destination is now ready to receive alerts.

## Step 2: Add Destination to Panther

Paste the copied `Slack Webhook URL` into the Panther Destination configuration settings:

![](../.gitbook/assets/slack-panther%20%287%29%20%285%29%20%285%29.png)

