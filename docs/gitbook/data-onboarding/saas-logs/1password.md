# 1Password

{% hint style="info" %}
This feature supports all 1Password plans and regions in Panther versions 1.31 and newer.

This feature is available only for US-based [**1Password Business**](https://1password.com/business/) accounts in Panther versions 1.25 to 1.30.&#x20;
{% endhint %}

Panther has the ability to fetch 1Password event logs by querying the [1Password Events API](https://support.1password.com/events-api-reference/). Panther is specifically monitoring the following 1Password events:

* Sign-in attempts from a user's 1Password account
* Items in shared vaults that have been modified, accessed or used

In order to set up 1Password as a log source in Panther, you'll need to authorize Panther in 1Password by generating an access token in your 1Password account and then set up 1Password as a log source in Panther.&#x20;

## Generate an Access Token in 1Password

1. [Sign in](https://start.1password.com/signin) to your 1Password account, then click **Integrations** in the sidebar
2. Choose "Other" as your SIEM
3. Enter a name for the integration, then click **Add Integration**.
4. Enter a name for the bearer token and choose when the token will expire. Select the event types the token has access to which will be `ItemUsage` and `SignInAttempts`**.**
   * For  information on issuing or revoking bearer tokens, please see [1Password's documentation](https://support.1password.com/events-reporting/#appendix-issue-or-revoke-bearer-tokens).
5. Click **Issue Token**.
6. Click **Save in 1Password** and choose which vault to save your token to. Then click **View Integration Details**.
7. Copy and paste the token into the Panther Console.&#x20;

## Create a new 1Password log source in Panther

{% hint style="info" %}
Once you complete the step above, you can continue with the step below.
{% endhint %}

1. Login to your Panther Console
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **1Password** from the list of available log sources
5. In the next screen, enter in a friendly name for the source e.g. `My 1Password Event logs` and then click **Next**
6. On this page, you'll authorize Panther to receive logs from 1Password. Copy the **access token key** from your 1Password account and paste it into the Panther UI.
7. Click on **Continue Setup** once you've filled out those fields
8. Panther will verify access to 1Password at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
9. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).



Note: By default, 1Password logs do not contain human readable values for objects such as vaults and login credentials. Please see our guide about using Lookup Tables to translate 1Password's Universally Unique Identifier (UUID) values into human readable names: [Using Lookup Tables: 1Password UUIDs](https://docs.runpanther.io/guides/using-lookup-tables-1password-uuids).
