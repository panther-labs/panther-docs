# 1Password

{% hint style="info" %}
This feature will be available in version 1.25
{% endhint %}

Panther has the ability to fetch 1Password event logs by querying the [1Password Events API](https://support.1password.com/events-api-reference/). Panther is specifically monitoring the following 1Password events:

* Sign-in attempts from a user's 1Password account
* Items in shared vaults that have been modified, accessed or used

In order to set up 1Password as a log source in Panther, you'll need to authorize Panther in 1Password by generating an access token in your 1Password account and then set up 1Password as a log source in Panther.&#x20;

## Generate an Access Token in 1Password

{% hint style="info" %}
The steps below can only be performed if you are an owner or administrator. You can read more on generating a Personal Access Token in 1Password [here](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).
{% endhint %}

1. [Sign in](https://start.1password.com/signin) to your 1Password account, click Integrations in the sidebar
2. Choose "**Other**" as your SIEM
3. Enter a name for the integration, then click Add Integration.
4. Enter a name for the bearer token and choose when the token will expire. Select the event types the token has access to which will be **ItemUsage** and **SignInAttempts.**
5. Click Issue Token.
6. Click Save in 1Password and choose which vault to save your token to. Then click View Integration Details.
7. Copy and paste the token into the Panther UI. You are done with this part!

If you have trouble creating the token, be sure to reference [1Password's docs](https://support.1password.com/events-reporting/) for reference.

## Create a new 1Password log source in Panther

{% hint style="info" %}
Once you complete the step above, you can continue with the step below.
{% endhint %}

1. Login to your Panther account
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **1Password** from the list of available log sources
5. In the next screen, enter in a friendly name for the source e.g. `My 1Password Event logs` and then click **Next**
6. On this page, you'll authorize Panther to receive logs from 1Password. Copy the **access token key** from your 1Password account and  paste it into Panther U..
7. Click on **Continue Setup** once you've filled out those fields
8. Panther will verify access to 1Password at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
9. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
10. Congrats, you're done!

