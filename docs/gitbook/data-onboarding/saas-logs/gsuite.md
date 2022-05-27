---
description: Onboarding Google G Suite logs to your Panther Console
---

# G Suite

## Overview

Panther can fetch G Suite (now named Google Workspace) events by querying the [Google Workspace Admin Reports API](https://developers.google.com/admin-sdk/reports/v1/get-start/getting-started). Panther will query the Reports API for new events every 60 seconds.

In order for Panther to access the API, you need to create a new G Suite App and provide the app credentials to Panther.

### G Suite integrations configured before February 2022

In February 2022, Google deprecated an OAuth flow that was used by the Panther integration. **If you had a G Suite integration configured in Panther prior to February 2022, you will need to re-authenticate your G Suite integration by following the steps below**. Note that you must follow these steps by October 2022 to prevent your integration from becoming disabled. &#x20;

1. Complete the steps under [Configure credentials for the new G Suite app](gsuite.md#configure-credentials-for-the-new-g-suite-app).&#x20;
2. In the Panther Console, re-enter the App Client ID and Client Secret provided in your GCP console, as described under [Create a new G Suite Source in Panther](gsuite.md#create-a-new-g-suite-source-in-panther).

## How to onboard G Suite logs to Panther

### Create a new G Suite App

{% hint style="info" %}
The steps below can only be performed if your G Suite user has permission to see your organization's Reports. If your user does not have permissions, follow the steps [here](https://support.google.com/a/answer/2406043) to create a new role with Reports access and assign the role to your user.
{% endhint %}

1. Log in to your [Google Cloud Platform console](https://console.developers.google.com/project).&#x20;
2. Click **Create Project.**\
   ****![](../../.gitbook/assets/gcp-create-project.png)****
3. Enter a descriptive project name (e.g. `Panther Integration`) and a location for the parent organization or folder.
4. Click **Create**
   * It will take a few seconds to create the project. Once created, you will see a notification on the page.
5. On the left sidebar menu, click **Home > Dashboard** to navigate back to your Google Cloud Platform Dashboard.&#x20;
6. Click **Select a Project** at the top of the page, then select the project you just created.\
   ![](../../.gitbook/assets/select-panther-project-google.png)
7. In the top search bar, search for **OAuth consent screen**, **** then select it.\
   ![](../../.gitbook/assets/oauth-consent.png)
8. Select **Internal** as **User Type**, then click **Create.**
9. In the next page fill in the following information:
   * **App Name**: Enter your project name or project ID.
   * **User support email**: **** Enter your email address.
   * **Developer contact information**: Enter your email address.
10. Click **Save And Continue.**

### **Add a scope to your new G Suite app and enable API**

1. Click **Add Or Remove Scopes.**
2. In the **Manually add scopes** section, paste `https://www.googleapis.com/auth/admin.reports.audit.readonly`.&#x20;
3. Click **Add to Table** and **Update**.
4. Click **Save and Continue.**
5. Click **Back to Dashboard.**
6. You will be redirected back to the dashboard of your new application. Click **Dashboard** in the top left.
7. Click **Enable APIs and Services.**\
   ****![](../../.gitbook/assets/gcp-apis-services.png)****
8. In the search bar in the top of the page, search `Admin SDK API.`
9. Click **Admin SDK API**, then click **Enable**
   * You will be redirected to another screen.&#x20;

### Configure credentials for the new G Suite app

1. Click **Credentials** in the left sidebar menu, then click **+Create Credentials** at to the top of the page.
2. Click **OAuth client ID.**
   * You will be redirected to a different page.\
     ![](../../.gitbook/assets/gcp-credentials.png)
3. On the new page, for Application Type, select **Web application** and type in a friendly name e.g. `Panther.`
   * Scroll down to the the section labeled "Authorized redirect URIs." In the **URIs 1** field, paste the redirect URL provided in the source's **Set Credentials** page.\
     ![](<../../.gitbook/assets/gsuite-oauth (1) (3).png>)
4. Click **Create**
5. A pop up screen will display the Client ID and Client Secret. **Using a secure method, make note of the ClientID and Client Secret**. You will need to provide them in the Panther Console to pull your reports.

### Create a new G Suite source in Panther

1. Log in to your Panther Console.
2. In the left sidebar, click **Integrations** > **Log** **Sources.**
3. Click **Add Source** then select G Suite from the list.
4. In the next screen enter the following:
   * &#x20;Friendly name for the source e.g. `My GSuite logs`&#x20;
   * Select the G Suite applications you want to monitor
5. Click **Next**.
6. Enter the **App Client ID** and the **Client Secret** that were provided in your Google Cloud Platform console.
7. Click **Next.**
8. Click the **Click here to authorize Panther to collect G Suite logs** link.
   * This will open a new tab, where you to authorize the G Suite App you create earlier to pull G Suite logs from your account.&#x20;
   * Authorize the app and copy the authorization code from the screen.
9. Enter the Authorization code that you copied into your Panther Console.
10. Click **Next** and then **Save source**.

## Panther-Built Detections for G Suite

The following detections are available for use immediately:&#x20;

* Advanced Protection
* Brute Force Login
* Doc Ownership Transfer
* Drive External Share
* Drive Overly Visible
* Drive Visibility Change
* Drive Visibility Change Deprecated
* External Forwarding
* Google Access
* Gov Attack
* Group Banned User
* High Severity Rule
* Leaked Password
* Login Type
* Low Severity Rule
* Medium Severity Rule
* Mobile Device Compromise
* Move Device Screen Unlock Fail
* Mobile Device Suspicious Activity
* Permissions Delegated
* Suspicious Logins
* Two Step Verification
* User Suspended

Please take a look at the files in the [gsuite\_activityevent\_rules](https://github.com/panther-labs/panther-analysis/tree/master/gsuite\_activityevent\_rules) __ and __ [gsuite\_reports\_rules](https://github.com/panther-labs/panther-analysis/tree/master/gsuite\_reports\_rules) repositories to see how these are built.&#x20;
