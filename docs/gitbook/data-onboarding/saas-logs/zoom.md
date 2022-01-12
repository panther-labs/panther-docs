# Zoom

{% hint style="info" %}
This feature is available in version 1.25.
{% endhint %}

Panther has the ability to fetch Zoom operational and activity logs by querying various Zoom API endpoints. Panther is specifically monitoring the following Zoom events:

* Changes to Account and Group settings
* Changes in role and license assignments for users
* Changes to subscriptions under Billing
* Changes made to SSO configuration, including changes made by your SSO and SAML mapping configuration

In order to set up Zoom as a log source in Panther, you'll need to authorize Panther in Zoom by creating an OAuth2 app in your Zoom account and then set up Zoom as a log source in Panther.&#x20;

## Create a new Zoom log source in Panther

1. Login to your Panther account
2. Navigate to **Integrations** > **Log** **Sources** on the sidebar menu
3. Click **Add Source**
4. Select **Zoom** from the list of available log sources
5. In the next screen, enter a friendly name for the source e.g. `My Zoom logs`. Click **Next.**
6. On this page, you'll authorize Panther to receive logs from Zoom. Copy the Redirect URL from Panther. Then, jump to [Create a new OAuth App section](zoom.md#create-a-new-oauth-app).&#x20;
7. Enter the **App Client ID** and the **Client Secret** that you acquire from Zoom.&#x20;
8. Click on **Continue Setup** once you've filled out those fields
9. Panther will verify access to Zoom at this point. If you run into an error, be sure to review the credentials you provided to Panther. If issues persist, reach out to Panther Support for help.
10. You will be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
11. Congrats, you're done!

## Create a new OAuth2 App&#x20;

{% hint style="info" %}
The steps below can only be performed if you are an account admin in your organization's Zoom account.
{% endhint %}

In order to create an OAuth app in Zoom, you'll need to register the app in the Zoom marketplace where you'll create the actually OAuth app. You can find more details on this process [here](https://marketplace.zoom.us/docs/guides/build/oauth-app). Follow the below instructions to get started.

OAuth2 apps can be set up by a user with the Admin role. The user authorizing the integration must have the Reports permissions to read Operations Logs and Sign-in/Sign-out Activity. The Owner can grant these permissions to the Admin role (Role Management -> Admin -> Reports), or create a new role with these permissions enabled. For details on Zoom Reporting and the provided permissions, you can check [this guide](https://support.zoom.us/hc/en-us/articles/201363213-Getting-started-with-Zoom-reporting#h\_01FM5FHPRMBC5RFJDAZ4FBTPKC). You can verify if your user has access to the Operations Logs by following [this guide](https://support.zoom.us/hc/en-us/articles/360032748331). Reference the screenshots below. before getting started with the steps to create the OAuth2 app.

![](https://lh4.googleusercontent.com/Xj9PNQ209u-5dJ9G35BOVsQzGxIf0qw6152Ysl46-aTsWeaWnjDENR1Pc\_L-veS205rIk-J\_Ga8IHmfOWdtS\_cgvhPs8tErTgEl4DkhdcVv-fraYgs7Gi9G1xZP\_3oNBwHU51use)

![](https://lh3.googleusercontent.com/guz0KTMEiYF6Rpu42ERh7NAeaBPRxBwcnQ9fS3kg5euUpsagXJ0YjIMsmkefGFxBVcOr8PnoUEtgQOdjh69ZorJ-jvt5kXUmc4fvPe8bHGMlWtDANEQg9y5z\_oTxLyAD7icy-K5K)

1. Log into your Zoom account
2. To register your app, visit the [Zoom App Marketplace](https://marketplace.zoom.us) and click **Develop** in the dropdown menu in the top-right corner of the page. Select **Build App**. A new page will appear displaying the available app types. Click **Create** in the **OAuth** option to continue.
3. Create an OAuth app with the following parameters:
   * **App Name** — The app’s name.
   * **App Type** — choose **Account-level app** as the app type.
   * **Distribution** — Set this toggle to enabled to make the app publicly available in the Zoom App Marketplace. Set this at your discretion but the app does not need to be publicly available.
4. When finished, click **Create**. A new window displaying your new OAuth app will appear.
5. When you create your app, be sure to copy the **Client ID** and **Client Secret** for your app as you will need this to authenticate Zoom as a log source in the Panther UI.
6. At this point, you will need the **Redirect URL** from Panther to paste into the **Redirect URL** field and **OAuth allow list** in the app. When creating your Zoom app within Panther, you may copy the Redirect URL within the Set Credentials step.

![](../../.gitbook/assets/papaya-oarfish.runpanther.net\_integrations\_log-sources\_zoom\_a4a9f1e6-45bc-40b3-ba7b-ebd5e441e664\_edit\_.png)

7\. After entering the **Redirect URL** into both fields, navigate to Scopes -> Add Scopes and Select Report with View report data ticked. Hit Done.

8\. Navigate back to step 7 of [Create a new Zoom log source in Panther](zoom.md#create-a-new-zoom-log-source-in-panther).



If you have trouble setting up the OAuth app in Zoom, be sure to check out[ Zoom's documentation](https://marketplace.zoom.us/docs/guides/build/oauth-app).
