# G Suite

Panther has the ability to fetch events by querying the [G Suite Reports API](https://developers.google.com/admin-sdk/reports/v1/get-start/getting-started). Panther will query the G Suite Reports API for new events every 1' minute.

In order for Panther to access the API you need to create a new 'G Suite App' and provide the app credentials to Panther.

## Create a new G Suite App

{% hint style="info" %}
The steps below can only be performed if your G Suite user has permissions to see your organization's Reports. If your user doesn't have such permissions, you can follow the steps [here](https://support.google.com/a/answer/2406043) in order to create a new role with Reports access and assign the role to your user.
{% endhint %}

1. Go to [Google API Console](https://console.developers.google.com/project)
2. Click **Create Project**

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.22.33 PM.png>)

1. Enter a project name e.g. `Panther Integration`. Make sure that the organization you want to monitor is selected under **Organization**. Click on **Create**

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.27.19 PM.png>)

1. It will take a few seconds to create the project. Once created, you will get an on-screen notification.
2. Go again to [Google API Console](https://console.developers.google.com). Select the project you just created

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.32.13 PM.png>)

1. In the top search bar, search for **OAuth consent screen**, **** then select it.

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.37.45 PM.png>)

1. Select **Internal** as **User Type** and click on **Create**
2. In the next page fill the following information
   1. Populate the **App Name** field with a value, e.g. `Panther Integration`
   2. Populate **User support email** with your email
   3. Populate **Developer contact information** near the bottom of the page with your desired email addresses
   4. Click on **Save And Continue**
3. Click on **Add Or Remove Scopes**
4. In the **Manually add scopes** section, paste `https://www.googleapis.com/auth/admin.reports.audit.readonly`. Click on **Add to Table** and **Update**.
5. Click on **Save and Continue**
6. Click **Back to Dashboard**
7. You will be navigated back to the dashboard of your new application. Click **Dashboard** in the top left.
8. Click on **Enable APIs and Services**

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.41.45 PM.png>)

1. In the search bar in the top, type `Admin SDK API`
2. Click on **Admin SDK API**, then click **Enable**
3. You will be navigated to another screen. Once this happens, just go to [Google API Console](https://console.developers.google.com) again and select your project  like you did in Step #5
4. Click on **Create Credentials** in the menu on the left.
5. Click on **OAuth client ID**

![](<../../.gitbook/assets/Screen Shot 2021-10-26 at 2.49.24 PM.png>)

1. In the new screen select as **Application Type** **Desktop App** and type in a friendly name e.g. `Panther`
2. Click on **Create**
3. A pop up screen will display the **Client ID** and **Client Secret**. Keep note of the ClientID and Client Secret! You will need to provide them in the Panther UI to pull your reports.

## Create a new G Suite source in Panther

1. Login to your Panther account
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **G Suite** from the list of available types
5.  In the next screen enter the following: 1. Friendly name for the source e.g. `My GSuite logs` 2. Select the GSuite applications you want to monitor

    Then click **Next**
6. The next page asks you to enter the **App Client ID** and the **Client Secret** that you acquired from GSuite
7. Click on **Next**
8.  Click on the **Click here to authorize Panther to collect GSuite logs** link.

    This will open a new tab, where you to authorize the GSuite App you create earlier to pull GSuite logs from your account. Authorize the app and copy the authorization code from the screen
9. Enter the Authorization code that you copied earlier in the Panther UI
10. Click on **Next** and then **Save source**.
