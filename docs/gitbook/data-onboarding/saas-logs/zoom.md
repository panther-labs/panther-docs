# Zoom

{% hint style="info" %}
This feature will be available in version 1.25
{% endhint %}

Panther has the ability to fetch Zoom operational and activity logs by querying various Zoom API endpoints. Panther is specifically monitoring the following Zoom events:

* Changes to Account and Group settings
* Changes in role and license assignments for users
* Changes to subscriptions under Billing
* Changes made to SSO configuration, including changes made by your SSO and SAML mapping configuration

In order to set up Zoom as a log source in Panther, you'll need to authorize Panther in Zoom by creating an OAuth2 app in your Zoom account and then set up Zoom as a log source in Panther.&#x20;

## Create a new OAuth App&#x20;

{% hint style="info" %}
The steps below can only be performed if you are an account admin in your organization's Zoom account.
{% endhint %}

In order to create an OAuth app in Zoom, you'll need to register the app in the Zoom marketplace where you'll create the actually OAuth app. You can find more details on this process [here](https://marketplace.zoom.us/docs/guides/build/oauth-app). Follow the below instructions to get started.

1. Log into your Zoom account
2. To register your app, visit the [Zoom App Marketplace](https://marketplace.zoom.us) and click **Develop** in the dropdown menu in the top-right corner of the page. Select **Build App**. A new page will appear displaying the available app types. Click **Create** in the **OAuth** option to continue.
3. Create an OAuth app with the following parameters:
   * **App Name** — The app’s name.
   * **App Type** — choose **Account-level app** as the app type.
   * **Distribution** — Set this toggle to enabled to make the app publicly available in the Zoom App Marketplace. Set this at your discretion but the app does not need to be publicly available.
4. When finished, click **Create**. A new window displaying your new OAuth app will appear.
5. When you create your app, be sure to copy the **Client ID** and **Client Secret** for your app as you will need this to authenticate Zoom as a log source in the Panther UI.
6. At this point, you will need the **Redirect URL** from Panther to paste into the **Redirect URL** field and **OAuth allow list** in the app. In order to do this, you will need to log into Panther and set up Zoom as a log source by following the directions below in the next section. Once you've made it to Step 6, you should be able to find the **Redirect URL **and continue setting up your Zoom app.
7. Once the **Redirect URL **is entered into both fields in the OAuth App in your Zoom account, you should be set!

If you have trouble setting up the OAuth app in Zoom, be sure to check out[ Zoom's documentation](https://marketplace.zoom.us/docs/guides/build/oauth-app).

## Create a new Zoom log source in Panther

{% hint style="info" %}
Once you complete the steps above, you can continue with the section below.
{% endhint %}

1. Login to your Panther account
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **Zoom** from the list of available log sources
5. In the next screen, enter in a friendly name for the source e.g. `My Zoom logs` and then click **Next**
6. On this page, you'll authorize Panther to receive logs from Zoom. Enter the **App Client ID** and the **Client Secret** that you acquired from Zoom. You should be able to find this information on the details page of the OAuth app in your Zoom account after you've **registered the application. **Be sure to copy the Redirect URL from Panther and paste it into the OAuth app in Zoom as mentioned in the previous section.
7. Click on **Continue Setup** once you've filled out those fields
8. Panther will verify access to Zoom at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
9. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
10. Congrats, you're done!

