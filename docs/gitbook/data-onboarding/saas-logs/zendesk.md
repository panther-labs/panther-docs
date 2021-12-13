# Zendesk

{% hint style="info" %}
This feature will be available in version 1.22
{% endhint %}

Panther has the ability to fetch [Zendesk audit logs ](https://developer.zendesk.com/api-reference/ticketing/account-configuration/audit\_logs/)by querying the [Zendesk Support API](https://developer.zendesk.com/api-reference/ticketing/introduction/).

{% hint style="warning" %}
Audit Logs may not be available depending on your Zendesk Suite Plan. You can learn more about Audit Logs availability per plan and instructions on how to view Audit Logs in Zendesk by following [this guide](https://support.zendesk.com/hc/en-us/articles/4408828001434-Viewing-the-audit-log-for-changes).
{% endhint %}

In order to set up Zendesk as a log source in Panther, you'll need to authorize Panther in Zendesk and then set up Zendesk as a log source in Panther. There are three ways to authorize Panther to receive Zendesk audit logs:

* **Create a new OAuth2 App** in Zendesk and provide the app credentials to Panther
* **Provide your Zendesk email address and password** in Panther for basic authentication
* **Generate a Personal Access Token** in Zendesk and provide credentials to Panther

{% hint style="warning" %}
Note that Zendesk API rate limits depend on your Zendesk Suite Plan, see [here](https://developer.zendesk.com/api-reference/ticketing/account-configuration/usage\_limits/) for more details
{% endhint %}

## Create a new OAuth2 App (option 1)

{% hint style="info" %}
You must be signed in as a Zendesk Support administrator to register an OAuth2 app. Find Zendesk docs [here](https://developer.zendesk.com/documentation/ticketing/working-with-oauth/using-oauth-to-authenticate-zendesk-api-requests-in-a-web-app/).
{% endhint %}

1. In Zendesk Support, click **Admin** and then select **API** in the Channels category.
2. Click the **OAuth Clients** tab on the Channels/API page, and then click **Add a Client** on the right side of the client list. A page for registering your application appears. The **Secret** field is pre-populated.
3. Complete the following required fields:
   * **Client Name** - This is the name that you will see on a list of apps that have access to your Zendesk Support instance.
   * **Unique Identifier** - Click the field to auto-populate it with the name you entered for your app. You can change it if you want.
   * **Redirect URLs** - You will find this in the Zendesk log source onboarding flow in the Panther UI (see screenshot below). This is the URL that Zendesk Support will use to send the user's decision to grant access to your application.
4.  Click **Save**.

    You'll be prompted to save the secret on the next page.
5.  Copy the **Secret** value and save it somewhere safe.

    The characters may extend past the width of the text box, so make sure to select everything before copying.
6. Click **Save**.
7. Once the application is registered, you will be able to view the **Client ID** and then generate a new **client secret** to paste into Panther. You are done with this part!

![Set up an OAuth Client here in the Zendesk UI](<../../../../.gitbook/assets/image (26).png>)

![Find the URL redirect here in the Panther UI](<../../../../.gitbook/assets/image (23).png>)

## Provide Zendesk Email and Password (option 2)

You can also set up Zendesk as a log source by providing your Zendesk Suppor Admin email and password in the Panther. If you choose to go with this approach, proceed to the last section of this article and have your Admin email and password handy as you onboard Zendesk as a log source in the Panther UI.

## Generate a Personal Access Token (option 3)

{% hint style="info" %}
The steps below can only be performed if you have admin permission in your Zendesk Support account. You can read more on generating a Personal Access Token in Zendesk [here](https://support.zendesk.com/hc/en-us/articles/226022787-Generating-a-new-API-token-).
{% endhint %}

1. Log into your Zendesk Support Admin account
2. Click the **Admin** icon in the sidebar, then select **Channels > API**.
3. Click the **Settings** tab, and make sure Token Access is **enabled**.
4. Click the **+** button to the right of **Active API Tokens**.
5. Enter a name for the token, and click **Create**. The token is generated, and displayed for you in a pop-up window:
6. Copy the token (in red), and paste it somewhere secure. Once you close this window, the full token will never be displayed again.
7. Copy and paste the token into the Panther UI as well as your Zendesk Support Admin Email. You are done with this part!

![Create a API token here in the Zendesk UI](<../../../../.gitbook/assets/image (14).png>)

![Find the API token here in the Zendesk UI](https://zen-marketing-documentation.s3.amazonaws.com/docs/en/token\_created.png)

## Create a new Zendesk source in Panther

{% hint style="info" %}
Once you complete one of the three options above, you can continue with the step below.
{% endhint %}

1. Login to your Panther account
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **Zendesk** from the list of available log sources
5. In the next screen, enter in a friendly name for the source e.g. `My Zendesk Audit logs` and your organization's Zendesk subdomain then click **Next**
6. On this page, you'll authorize Panther to receive logs from Zendesk. Depending on the option you chose above, follow the steps below:
   1. **Created an OAuth App**: Enter the **App Client ID** and the **Client Secret** that you acquired from Zendesk. You should be able to find this information on the details page of the OAuth app in your Zendesk account once you **register the application.**
   2. **Zendesk Support Admin Email and Password**: Enter in your Zendesk Support Admin Email and Password for basic authentication.
   3. **Generate a Personal Access Token:** Copy the **personal access token key,** paste it into Panther UI and then enter in the Zendesk Support Admin email.
7. Click on **Continue Setup** once you've filled out those fields
8. Panther will verify access to Zendesk at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
9. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
10. Congrats, you're done!



