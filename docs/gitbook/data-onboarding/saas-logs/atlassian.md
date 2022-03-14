# Atlassian

{% hint style="info" %}
This feature is available in Panther versions 1.27 and newer and it requires a subscription to [Atlassian Access](https://support.atlassian.com/security-and-access-policies/docs/understand-atlassian-access/).
{% endhint %}

Panther has the ability to fetch Atlassian event logs by querying the [Atlassian Organizations REST API](https://developer.atlassian.com/cloud/admin/organization/rest/intro/). Panther is specifically monitoring the following Atlassian events:

* Administrative actions, related to settings or other organization pages&#x20;
* Actions that organization admins take related to the organization’s security policies

In order to set up Atlassian as a log source in Panther, you'll need to authorize Panther in Atlassian by generating an API key in your Atlassian account and then set up Atlassian as a log source in Panther.&#x20;

## Generate an API key in Atlassian

{% hint style="info" %}
The steps below can only be performed if you are an owner or administrator. You can read more on generating an API Key in Atlassian [here](https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/).
{% endhint %}

1. From your organization at [admin.atlassian.com](http://admin.atlassian.com), select **Settings** > **API keys**.
2. Click **Create API key** in the top right.
3. Enter a name that you’ll remember to identify the API key.
4. By default, the key expires one week from today. If you’d like to change the expiration date, pick a new date under **Expires on**. You’re unable to select a date longer than a year from the date of creation.
5. Click **Create** to save the API key.
6. Copy the values for your **Organization ID** and **API key**. You'll need those to access your organization later. Make sure you store these values in a safe place, as we won't show them to you again.
7. Click **Done** and you’ll see the key in your list of API keys.

If you have trouble creating the API key, be sure to reference [Atlassian's docs](https://developer.atlassian.com/cloud/admin/organization/rest/intro/) for reference.

## Create a new Atlassian log source in Panther

{% hint style="info" %}
Once you complete the step above, you can continue with the step below.
{% endhint %}

1. Login to your Panther Console
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **Atlassian** from the list of available log sources
5. In the next screen, enter in a friendly name for the source e.g. `My Atlassian Event logs` and then click **Next**
6. On this page, you'll authorize Panther to receive logs from Atlassian. Copy the **API Key** from your Atlassian account and paste it into the Panther UI.
7. Click on **Continue Setup** once you've filled out those fields
8. Panther will verify access to Atlassian at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
9. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
10. Congrats, you're done!
