---
description: Panther supports pulling logs directly from Box
---

# Box

## Overview

Panther can pull audit events from the [Box Events API](https://developer.box.com/reference/get-events/) every 60 seconds for real-time detection.

For Panther to access the Box API, you will need to create a new Box App and provide its credentials to Panther.

## How to onboard Box logs to Panther

### Prerequisites

* To read events from the entire enterprise account, the Box user performing the following steps _must_ have [full admin priviledges on the account](https://developer.box.com/guides/authentication/user-types/managed-users/#admin--co-admin-roles) (_not_ co-admin).
* For security and availability reasons, we recommend creating a new Box App solely for Panther. Make sure to copy the **redirect URL** from this page.

### Step 1: Create a new Box source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Box** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g., `My Box logs`.
6. Click **Continue Setup**.&#x20;
7. On the **Set Credentials** page, click **Copy** under Step 1 to copy your redirect URL.\
   ![](<../../.gitbook/assets/Screen Shot 2022-03-16 at 2.19.06 PM.png>)
8. **Note:** Before you continue the setup process in your Panther Console, you must create a new app in your Box Developer Console and retrieve the Client ID and Client Secret.

### Step 2: Create a new Box app in your Box Developer Console

1. In a separate browser tab or window, log in to the [Box Developer Console](https://app.box.com/developers/console).
2. Click **Create New App.**\
   ****![](../../.gitbook/assets/box-new-app.png)
3. Select **Custom App** for the app type then click **Next.**
4. Select **User Authentication (OAuth 2.0)**, enter a memorable name for your app (e.g. `Panther`), then click **Create App.**\
   ****![](../../../../.gitbook/assets/box-new-app-page3.png)****
5. In your new app's Configuration tab, scroll down to the **OAuth 2.0 Redirect URI** section and paste the redirect URL you copied from your Panther console.\
   ![](../../.gitbook/assets/box-oauth-redirect.png)
6. On the **Application Scopes** section make sure **Manage enterprise properties** is selected (it is **not** selected by default).\
   ![](../../.gitbook/assets/box-app-scopes.png)
7. Click **Save Changes**.

### Step 3: Finalize Box onboarding in Panther

1. In the Box Developer console, navigate to the new app you created for Panther. In the Configuration tab, scroll down to the **OAuth 2.0 Credentials** section.\
   ![](../../.gitbook/assets/box-credentials.png)
2. Copy the **Client ID** and **Client Secret** credentials and paste them into the **Set Credentials** page in your Panther Console.
3. Click **Continue Setup**.&#x20;
4. Click **Grant Access**.
   * You will be redirected to Box.
5. Click **Grant Access to Box.**&#x20;
   * You will be redirected back to Panther.
6. Click **Continue Setup.**
7. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
8. Click **Finish Setup**.

### Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Access Granted
* Anomalous Download
* Brute Force Login
* Event Triggered Externally
* Item Shared Externally
* Malicious Content
* New Login
* Policy Violation
* Suspicious Login or Session
* Untrusted Device
* User Downloads
* User Permission Updates

Please look at the code and corresponding metadata in the [box\_rules](https://github.com/panther-labs/panther-analysis/tree/master/box\_rules) repository to see how these are built.&#x20;

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Box.Event

Contains events for the entire enterprise.

Reference: [Box Documentation on List User and Enterprise Events.](https://developer.box.com/reference/get-events/)

```yaml
schema: Box.Event
parser:
    native:
        name: Box.Event
description: Contains events for the entire enterprise
referenceURL: https://developer.box.com/reference/get-events
version: 0
fields:
    - name: additional_details
      description: This object provides additional information about the event if available.
      type: json
    - name: created_at
      description: The timestamp of the event
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: created_by
      description: The user that performed the action represented by the event.
      type: object
      fields:
        - name: id
          description: The unique identifier for this object
          type: string
        - name: type
          description: The object type
          type: string
        - name: login
          description: The primary email address of this user
          type: string
          indicators:
            - email
        - name: name
          description: The display name of this user
          type: string
    - name: event_id
      required: true
      description: The ID of the event object. You can use this to detect duplicate events
      type: string
    - name: event_type
      required: true
      description: The event type that triggered this event
      type: string
    - name: type
      required: true
      description: The object type (always 'event')
      type: string
    - name: source
      required: true
      description: The item that triggered this event
      type: object
      fields:
        - name: id
          description: The unique identifier for this object
          type: string
        - name: type
          description: The object type
          type: string
        - name: login
          description: The primary email address of this user
          type: string
          indicators:
            - email
        - name: name
          description: The display name of this user
          type: string
        - name: item_id
          description: The unique identifier that represents the item.
          type: string
        - name: item_name
          description: The name of the item.
          type: string
        - name: item_type
          description: The type of the item that the event represents. Can be file or folder.
          type: string
        - name: owned_by
          description: The user who owns this item.
          type: object
          fields:
            - name: id
              description: The unique identifier for this object
              type: string
            - name: type
              description: The object type
              type: string
            - name: login
              description: The primary email address of this user
              type: string
              indicators:
                - email
            - name: name
              description: The display name of this user
              type: string
        - name: parent
          description: The optional folder that this folder is located within.
          type: object
          fields:
            - name: etag
              description: The HTTP etag of this folder.
              type: string
            - name: id
              description: The unique identifier that represent a folder.
              type: string
            - name: type
              required: true
              description: The type of the object (always 'folder')
              type: string
            - name: name
              description: The name of the folder
              type: string
            - name: sequence_id
              description: A numeric identifier that represents the most recent user event that has been applied to this item.
              type: string
        - name: api_key
          description: The API key used for this action
          type: string
    - name: session_id
      description: The event type that triggered this event
      type: string
    - name: ip_address
      description: The IP address the request was made from.
      type: string
      indicators:
        - ip
```
