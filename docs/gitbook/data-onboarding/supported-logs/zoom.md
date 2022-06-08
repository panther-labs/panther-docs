---
description: Panther supports pulling logs directly from Zoom
---

# Zoom

## Overview

Panther can fetch Zoom operational and activity logs by querying various Zoom API endpoints. Panther can specifically monitor the following Zoom events:

* Changes to Account and Group settings
* Changes in role and license assignments for users
* Changes to subscriptions under Billing
* Changes made to SSO configuration, including changes made by your SSO and SAML mapping configuration

To set up this integration, you will create an OAuth2 app in your Zoom account and then configure Zoom as a log source in your Panther Console.&#x20;

## How to onboard Zoom logs to Panther

### Prerequisites

* To create an OAuth app in Zoom, you need to register the app in the Zoom Marketplace. You can find more details on this process in Zoom's documentation here: [Create an OAuth App](https://marketplace.zoom.us/docs/guides/build/oauth-app/).&#x20;
* The Zoom user authorizing the integration must have the Admin role and Reports permissions to read [Operations Logs and Sign-in/Sign-out Activity](https://support.zoom.us/hc/en-us/articles/360032748331).

### Create a new Zoom log source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Zoom** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g. `My Zoom logs`.&#x20;
6. Click **Continue Setup**.
7. Copy the Redirect URL from Panther and save it in a safe location. You will need this in the next steps when you create an OAuth2 App in Zoom.&#x20;
8. Keep this browser window open, as you will need to complete additional configuration in the next steps.

### Create a new OAuth2 App&#x20;

{% hint style="info" %}
You must be an Admin user in your organization's Zoom account to create the OAuth2 App.
{% endhint %}

1. Log in to your Zoom account.
2. Register your app.
   1. Go to the [Zoom App Marketplace](https://marketplace.zoom.us/) .
   2. Click **Develop** in the dropdown menu in the top-right corner of the page.&#x20;
   3. Select **Build App**.&#x20;
      * A new page will appear displaying the available app types.&#x20;
   4. Click **Create** in the **OAuth** option to continue.
3. Create an OAuth app with the following parameters:
   * **App Name:** The app’s name.
   * **App Type:** Choose **Account-level app** as the app type.
   * **Distribution:** You can choose **Enabled** to make the app publicly available in the Zoom App Marketplace, or toggle to **Disabled** to make the app private. The app does not need to be publicly available for this integration.
4. When finished, click **Create**.&#x20;
   * A new window displaying your new OAuth app will appear.
5. When you create your app, be sure to copy the **Client ID** and **Client Secret** for your app as you will need this to finish your setup in the Panther Console.
6. In the **Redirect URL** and **OAuth allow list** fields, paste in the Redirect URL that you copied from the Panther Console in the earlier steps of this documentation.&#x20;
7. Navigate to **Scopes > Add Scopes** and Select **Report** with "View report data" ticked.&#x20;
8. Click **Done**.
9. Navigate back to the Panther Console to complete the final setup.

### Finish Setup in Panther

1. In the Panther Console, on the **Set Credentials** page, enter the **Client ID** and the **Client Secret** that you obtained from Zoom.&#x20;
2. Click **Continue Setup**.
3. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
4. Click **Finish Setup**.

### Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Operation Passcode Disabled
* Operation User Granted Admin

Refer to the files in the [zoom\_operation\_rules](https://github.com/panther-labs/panther-analysis/tree/master/zoom\_operation\_rules) repository to see how these are built.

## Supported log types

{% hint style="info" %}
Required fields in the schemas are listed as "required: true" just below the "name" field.
{% endhint %}

### Zoom.Activity

Sign in/sign out activity logs of users under a Zoom account.

Reference: [Zoom Documentation on Sign In Sign Out Reports.](https://marketplace.zoom.us/docs/api-reference/zoom-api/methods/#operation/reportSignInSignOutActivities)

```yaml
schema: Zoom.Activity
parser:
    native:
        name: Zoom.Activity
description: Sign in / sign out activity logs of users under a Zoom account
referenceURL: https://marketplace.zoom.us/docs/api-reference/zoom-api/reports/reportsigninsignoutactivities
version: 0
fields:
    - name: email
      required: true
      description: The email address of the user used for activity.
      type: string
      indicators:
        - email
    - name: time
      required: true
      description: The timestamp of user activity
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: type
      description: 'Type of user activity: sign in/sign out'
      type: string
    - name: ip_address
      description: The IP address of the device used to access Zoom.
      type: string
      indicators:
        - ip
    - name: client_type
      description: The client interface type using which the activity was performed.
      type: string
    - name: version
      description: Zoom client version of the user.
      type: string
```

### Zoom.Operation

The report allows you to audit admin and user activity, such as adding a new user, changing account settings, and deleting recordings.

Reference: [Zoom Documentation on Operation Log Reports.](https://marketplace.zoom.us/docs/api-reference/zoom-api/methods/#operation/reportMeetingPolls)

```yaml
schema: Zoom.Operation
parser:
    native:
        name: Zoom.Operation
description: The report allows you to audit admin and user activity, such as adding a new user, changing account settings, and deleting recordings
referenceURL: https://marketplace.zoom.us/docs/api-reference/zoom-api/reports/reportoperationlogs
version: 0
fields:
    - name: time
      required: true
      description: The time at which the operation was performed.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: operator
      required: true
      description: The user who performed the operation.
      type: string
      indicators:
        - email
    - name: category_type
      required: true
      description: Operation category type
      type: string
    - name: action
      description: Action descriptions
      type: string
    - name: operation_detail
      description: Operation detail
      type: string
```
