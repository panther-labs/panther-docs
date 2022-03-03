# Microsoft Teams

This page will walk you through configuring MS Teams as a Destination for your Panther alerts.

The MS Teams Destination requires a `Microsoft Teams Webhook URL`. When an alert is forwarded to an MS Teams Destination, it sends a message to the specified Webhook URL:

![](<../../../.gitbook/assets/msteams-panther (7) (5) (4).png>)

The Microsoft Teams Destination is configured via a custom connector with a Webhook URL. First, ensure that your team has the option to add Incoming Webhooks as a connector. Go the `Apps` settings at the bottom left of you Teams client, then select `Connectors` and then `Incoming Webhook`:

![](<../../../.gitbook/assets/msteams1 (9) (7) (13).png>)

Select the `Add to a team` button and you will be prompted to select a team to add the Incoming Webhook connector to, select the appropriate team and select `Setup a connector`:

![](<../../../.gitbook/assets/msteams2 (13) (7) (1) (11).png>)

Select the `Configure` button next to Incoming Webhook, and configure the name, description, and other settings as appropriate:

![](<../../../.gitbook/assets/msteams3 (13) (6) (15).png>)

You will be prompted to name the integration, and optionally upload an image to display. After filling out these settings, select the `Create` button:

![](<../../../.gitbook/assets/msteams4 (13) (5) (14).png>)

You will then be presented with the webhook URL. Copy this out into the Panther Destinations page and select the `Done` button:

![](<../../../.gitbook/assets/msteams5 (12) (5) (12).png>)

Your MS Teams destination is now ready to receive notifications when Policies and Rules send alerts:

![](<../../../.gitbook/assets/msteams6 (12) (4) (1) (3).png>)
