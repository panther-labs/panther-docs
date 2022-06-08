---
description: Panther supports pulling logs directly from G Suite
---

# G Suite

## Overview

Panther can fetch G Suite (now named Google Workspace) events by querying the [Google Workspace Admin Reports API](https://developers.google.com/admin-sdk/reports/v1/get-start/getting-started). Panther will query the Reports API for new events every 60 seconds.

In order for Panther to access the API, you need to create a new G Suite App and provide the app credentials to Panther.

### G Suite integrations configured before February 2022

In February 2022, Google deprecated an OAuth flow that was used by the Panther integration. **If you had a G Suite integration configured in Panther prior to February 2022, you will need to re-authenticate your G Suite integration by following the steps below**. Note that you must follow these steps by October 2022 to prevent your integration from becoming disabled. &#x20;

1. Complete the steps under [Configure credentials for the new G Suite app](g-suite.md#configure-credentials-for-the-new-g-suite-app).&#x20;
2. In the Panther Console, re-enter the App Client ID and Client Secret provided in your GCP console, as described under [Create a new G Suite Source in Panther](g-suite.md#create-a-new-g-suite-source-in-panther).

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
     ![](<../../.gitbook/assets/gsuite-oauth (1) (1) (1) (3).png>)
4. Click **Create**
5. A pop up screen will display the Client ID and Client Secret. **Using a secure method, make note of the ClientID and Client Secret**. You will need to provide them in the Panther Console to pull your reports.

### Create a new G Suite source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Google Workspaces** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g., `My GSuite logs` and select the G Suite applications you want to monitor.
6. Click **Continue Setup.**
7. On the **Set Credentials** page, enter your **Client ID** and the **Client Secret** that were provided in your Google Cloud Platform console.
8. Click **Continue Setup**.
9. Click **Grant Access**.
   * This will open a new tab, for you to authorize the G Suite App you created earlier to pull G Suite logs from your account.&#x20;
   * Authorize the app and copy the Authorization Code from the screen.
10. Enter the Authorization Code that you copied into your Panther Console.
11. Click **Continue Setup** and then **Save Source**.
12. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
    * This feature sends an error message if logs aren't received within a specified time interval.
13. Click **Finish Setup**.

### Panther-Built Detections for G Suite

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

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### GSuite.ActivityEvent

Each activity event for a specific account and application such as the Admin console application or the Google Drive application.

Reference: [Google Workspace Documentation on Reports API Activities List.](https://developers.google.com/admin-sdk/reports/v1/reference/activities/list#response)

```yaml
schema: GSuite.ActivityEvent
parser:
    native:
        name: GSuite.ActivityEvent
description: Each activity event for a specific account and application such as the Admin console application or the Google Drive application.
referenceURL: https://developers.google.com/admin-sdk/reports/v1/reference/activities/list#response
version: 0
fields:
    - name: id
      required: true
      description: Unique identifier for each activity record.
      type: object
      fields:
        - name: applicationName
          description: Application name to which the event belongs.
          type: string
        - name: customerId
          description: The unique identifier for a G suite account.
          type: string
        - name: time
          description: Time of occurrence of the activity.
          type: timestamp
          timeFormat: rfc3339
          isEventTime: true
        - name: uniqueQualifier
          description: Unique qualifier if multiple events have the same time.
          type: string
    - name: actor
      description: User doing the action.
      type: object
      fields:
        - name: email
          description: The primary email address of the actor. May be absent if there is no email address associated with the actor.
          type: string
          indicators:
            - email
        - name: profileId
          description: The unique G Suite profile ID of the actor. May be absent if the actor is not a G Suite user.
          type: string
        - name: callerType
          description: The type of actor.
          type: string
        - name: key
          description: Only present when callerType is KEY. Can be the consumer_key of the requestor for OAuth 2LO API requests or an identifier for robot accounts.
          type: string
    - name: kind
      required: true
      description: The type of API resource. For an activity report, the value is reports#activities.
      type: string
    - name: ownerDomain
      description: This is the domain that is affected by the report's event. For example domain of Admin console or the Drive application's document owner.
      type: string
      indicators:
        - domain
    - name: ipAddress
      description: IP address of the user doing the action. This is the Internet Protocol (IP) address of the user when logging into G Suite which may or may not reflect the user's physical location. For example, the IP address can be the user's proxy server's address or a virtual private network (VPN) address. The API supports IPv4 and IPv6.
      type: string
      indicators:
        - ip
    - name: type
      description: Type of event. The G Suite service or feature that an administrator changes is identified in the type property which identifies an event using the eventName property. For a full list of the API's type categories, see the list of event names for various applications above in applicationName.
      type: string
    - name: name
      description: Name of the event. This is the specific name of the activity reported by the API. And each eventName is related to a specific G Suite service or feature which the API organizes into types of events.
      type: string
    - name: parameters
      description: Parameter value pairs for various applications. For more information about eventName parameters, see the list of event names for various applications above in applicationName.
      type: json
```

### GSuite.Reports

Contains the activity events for a specific account and application such as the Admin console application or the Google Drive application.

Reference: [Google Workspace Documentation on Reports API Activities List.](https://developers.google.com/admin-sdk/reports/v1/reference/activities/list#response)

```yaml
schema: GSuite.Reports
parser:
    native:
        name: GSuite.Reports
description: Contains the activity events for a specific account and application such as the Admin console application or the Google Drive application.
referenceURL: https://developers.google.com/admin-sdk/reports/v1/reference/activities/list#response
version: 0
fields:
    - name: id
      required: true
      description: Unique identifier for each activity record.
      type: object
      fields:
        - name: applicationName
          description: Application name to which the event belongs.
          type: string
        - name: customerId
          description: The unique identifier for a G suite account.
          type: string
        - name: time
          description: Time of occurrence of the activity.
          type: timestamp
          timeFormat: rfc3339
          isEventTime: true
        - name: uniqueQualifier
          description: Unique qualifier if multiple events have the same time.
          type: string
    - name: actor
      description: User doing the action.
      type: object
      fields:
        - name: email
          description: The primary email address of the actor. May be absent if there is no email address associated with the actor.
          type: string
          indicators:
            - email
        - name: profileId
          description: The unique G Suite profile ID of the actor. May be absent if the actor is not a G Suite user.
          type: string
        - name: callerType
          description: The type of actor.
          type: string
        - name: key
          description: Only present when callerType is KEY. Can be the consumer_key of the requestor for OAuth 2LO API requests or an identifier for robot accounts.
          type: string
    - name: kind
      required: true
      description: The type of API resource. For an activity report, the value is reports#activities.
      type: string
    - name: ownerDomain
      description: This is the domain that is affected by the report's event. For example domain of Admin console or the Drive application's document owner.
      type: string
      indicators:
        - domain
    - name: ipAddress
      description: IP address of the user doing the action. This is the Internet Protocol (IP) address of the user when logging into G Suite which may or may not reflect the user's physical location. For example, the IP address can be the user's proxy server's address or a virtual private network (VPN) address. The API supports IPv4 and IPv6.
      type: string
      indicators:
        - ip
    - name: events
      description: Activity events in the report.
      type: array
      element:
        type: object
        fields:
            - name: type
              description: Type of event. The G Suite service or feature that an administrator changes is identified in the type property which identifies an event using the eventName property. For a full list of the API's type categories, see the list of event names for various applications above in applicationName.
              type: string
            - name: name
              description: Name of the event. This is the specific name of the activity reported by the API. And each eventName is related to a specific G Suite service or feature which the API organizes into types of events.
              type: string
            - name: parameters
              description: Parameter value pairs for various applications. For more information about eventName parameters, see the list of event names for various applications above in applicationName.
              type: array
              element:
                type: object
                fields:
                    - name: name
                      description: The name of the parameter.
                      type: string
                    - name: value
                      description: String value of the parameter.
                      type: string
                    - name: intValue
                      description: Integer value of the parameter.
                      type: bigint
                    - name: boolValue
                      description: Boolean value of the parameter.
                      type: boolean
                    - name: multiValue
                      description: String values of the parameter.
                      type: array
                      element:
                        type: string
                    - name: multiIntValue
                      description: Integer values of the parameter.
                      type: array
                      element:
                        type: bigint
                    - name: messageValue
                      description: 'Nested parameter value pairs associated with this parameter. Complex value type for a parameter are returned as a list of parameter values. For example, the address parameter may have a value as [{parameter: [{name: city, value: abc}]}]'
                      type: json
                    - name: multiMessageValue
                      description: List of messageValue objects.
                      type: array
                      element:
                        type: json
```
