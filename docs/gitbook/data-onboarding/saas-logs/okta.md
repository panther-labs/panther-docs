# Okta

Panther has the ability to fetch Okta events by querying the [Okta System Log API](https://developer.okta.com/docs/reference/api/system-log/). Panther will query the System Log API every 1' minute.

In order for Panther to access the API you need to create a new API token or use an existing one.

## Create a new Okta API token

{% hint style="info" %}
To create an API token with permissions to query Okta System Logs, you will need to be logged in as an administrator that has the rights to perform your API call's actions. Please refer to [Okta documentation](https://help.okta.com/en/prod/Content/Topics/Security/Administrators.htm?Highlight=administrators) for information on managing Admin roles and their rights.
{% endhint %}

1. Log in as Okta administrator
2. In the Okta Admin Console, navigate to **Security** &gt; **API**
3. Click **Create Token**
4. Enter a name for your token, e.g. `Panther API token`
5. Keep a note of **Token value** in the pop-up screen.

   **Important**: Be sure to document and store the API token value carefully, as it cannot be retrieved later and can present a security risk if used in an unauthorized fashion.

## Create a new Okta source in Panther

1. Login to your Panther account.
2. Go to **Integrations &gt; Log Sources** from the sidebar menu.
3. Click **Add Source.**
4. Select **Okta** from the list of available types.
5. In the following form, fill in the following fields:
   1. **Name**: A friendly name for the source e.g. `My Okta logs`
   2. **Okta**: The name of your Okta domain. Should be in the form `https://my-org.okta.com`
   3. **API Token**: The token value you noted before
6. Click on **Next** and then **Save Source**. The **API Token** will be stored, encrypted, in Panther backend.
7. You are done! You can now start writing detections and exploring your Okta data.

