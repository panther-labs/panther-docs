---
description: Panther supports pulling logs directly from Zendesk
---

# Zendesk

## Overview

Panther supports pulling logs directly from Zendesk. Panther can fetch [Zendesk audit logs ](https://developer.zendesk.com/api-reference/ticketing/account-configuration/audit\_logs/)by querying the [Zendesk Support API](https://developer.zendesk.com/api-reference/ticketing/introduction/).

{% hint style="warning" %}
Audit Logs may not be available depending on your Zendesk Suite Plan. You can learn more about Audit Logs availability per plan and instructions on how to view Audit Logs in Zendesk by following [this guide](https://support.zendesk.com/hc/en-us/articles/4408828001434-Viewing-the-audit-log-for-changes).
{% endhint %}

In order to set up Zendesk as a log source in Panther, you'll need to authorize Panther in Zendesk and then set up Zendesk as a log source in Panther. There are three ways to authorize Panther to receive Zendesk audit logs:

* **Create a new OAuth2 App** in Zendesk and provide the app credentials to Panther
* **Provide your Zendesk email address and password** in Panther for basic authentication
* **Generate a Personal Access Token** in Zendesk and provide credentials to Panther

{% hint style="warning" %}
**Note:** Zendesk API rate limits depend on your Zendesk Suite Plan, see [here](https://developer.zendesk.com/api-reference/ticketing/account-configuration/usage\_limits/) for more details
{% endhint %}

## How to onboard Zendesk logs to Panther

### Option 1: Create a new OAuth2 App

{% hint style="info" %}
You must be signed in as a Zendesk Support administrator to register an OAuth2 app. For more information, see [Zendesk's OAuth documentation](https://developer.zendesk.com/documentation/ticketing/working-with-oauth/using-oauth-to-authenticate-zendesk-api-requests-in-a-web-app/).&#x20;
{% endhint %}

1. Log in to your Zendesk Admin Center.&#x20;
2. Click the **Admin** icon in the sidebar, then navigate to **Channels > Apps and Integrations> APIs> Zendesk API**.
3. Click the **OAuth Clients** tab on the Channels/API page, and then click **Add Oauth Client** on the right side of the client list.&#x20;
   * A page for registering your application appears. The **Secret** field is pre-populated.
4. Complete the following required fields, then click **Save.**
   * **Client Name** - This is the name that you will see on a list of apps that have access to your Zendesk Support instance.
   * **Unique Identifier** - Click the field to auto-populate it with the name you entered for your app. You can change it if you want.
   * **Redirect URLs** - You will find this in the Zendesk log source onboarding flow in the Panther UI (see screenshot below). This is the URL that Zendesk Support will use to send the user's decision to grant access to your application.
5. When prompted, copy the **Secret** value and save it somewhere safe. Click **Save**.
   * The characters may extend past the width of the text box, so make sure to select everything before copying.
6.  Once the application is registered, you will be able to view the **Unique Identifier**. Use the Unique Identifier generated by Zendesk as the Client ID and then generate a new **client secret** to paste into Panther.



![Set up an OAuth Client here in the Zendesk UI](<../../.gitbook/assets/image (23).png>)

![Find the URL redirect here in the Panther UI](<../../.gitbook/assets/image (14).png>)

### Option 2: Provide Zendesk Email and Password

You can also set up Zendesk as a log source by providing your Zendesk Suppor Admin email and password in the Panther. If you choose to go with this approach, proceed to the last section of this article and have your Admin email and password handy as you onboard Zendesk as a log source in the Panther UI.

### Option 3: Generate a Personal Access Token

{% hint style="info" %}
The steps below can only be performed if you are a Zendesk Support administrator. You can read more on generating a Personal Access Token in Zendesk [here](https://support.zendesk.com/hc/en-us/articles/226022787-Generating-a-new-API-token-).
{% endhint %}

1. Log in to your Zendesk Support account.
2. Click the **Admin** icon in the sidebar, then select **Channels > Apps and Integrations> APIs > Zendesk API**.
3. Click the **Settings** tab, and make sure Token Access is **enabled**.
4. Click the **+** button to the right of **Active API Tokens**.
5. Enter a name for the token, and click **Create**. The token is generated, and displayed in a pop-up window.
6. Copy the token (in red), and store it in a secure location. You will need it in the next steps.&#x20;
   * **Note**: Once this window is closed, the full token will never be displayed again.

![Create a API token here in the Zendesk UI](<../../.gitbook/assets/image (26).png>)

![Find the API token here in the Zendesk UI](https://zen-marketing-documentation.s3.amazonaws.com/docs/en/token\_created.png)

### Create a new Zendesk source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Zendesk** from the list of available log sources. Click **Start Source Setup.**
5. On the next screen, enter a memorable name for the source e.g., `My Zendesk Audit logs` and your organization's Zendesk subdomain.
6. Click **Continue Setup.**
7. Authorize Panther to receive logs from Zendesk. Depending on the option you chose above, follow the accompanying steps below:
   * **Created an OAuth App**: Enter the **Client ID** and the **Client Secret** that you acquired from Zendesk. You should be able to find this information on the details page of the OAuth app in your Zendesk account once you **register the application.**
   * **Zendesk Support Admin Email and Password**: Enter your Zendesk Support Admin Email and Password for basic authentication.
   * **Generate a Personal Access Token:** Enter the Zendesk Support Admin email. Copy the **personal access token** and paste it into the API token field.
8. Click **Continue Setup**.
9. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
10. Click **Finish Setup**.

## Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Mobile App Access
* New API Token
* New Owner
* Sensitive Data Redaction
* User Assumption
* User Role
* User Suspension

Refer to the files in the [zendesk\_rules](https://github.com/panther-labs/panther-analysis/tree/master/zendesk\_rules) repository to see how these are built.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as "required: true" just below the "name" field.
{% endhint %}

### Zendesk.Audit

The audit log shows various changes in your Zendesk since the account was created. It saves a record of these changes indefinitely, and you can view the entire change history.

Reference: [Zendesk Documentation on Audit Logs.](https://developer.zendesk.com/api-reference/ticketing/account-configuration/audit\_logs/)

```yaml
schema: Zendesk.Audit
parser:
    native:
        name: Zendesk.Audit
description: The audit log shows various changes in your Zendesk since the account was created. It saves a record of these changes indefinitely, and you can view the entire change history.
referenceURL: https://developer.zendesk.com/rest_api/docs/support/audit_logs
version: 0
fields:
    - name: action
      description: Values can be 'login', 'create', 'update', or 'destroy'
      type: string
    - name: action_label
      description: Localized string of action field
      type: string
    - name: actor_id
      description: The id of the user creating the ticket
      type: bigint
    - name: change_description
      description: The description of the change that occurred
      type: string
    - name: created_at
      required: true
      description: The time the audit got created
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: id
      required: true
      description: The id automatically assigned upon creation
      type: bigint
    - name: ip_address
      description: The IP address of the user doing the audit
      type: string
      indicators:
        - ip
    - name: source_id
      description: The id of the item being audited
      type: bigint
    - name: source_label
      description: The name of the item being audited
      type: string
    - name: source_type
      description: The item type being audited
      type: string
    - name: url
      description: The URL to access the audit log
      type: string
```
