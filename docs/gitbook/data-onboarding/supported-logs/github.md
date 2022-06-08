---
description: Panther supports pulling logs directly from GitHub
---

# Github

## Overview

Panther has the ability to fetch Github audit logs by querying the [Github API](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization). Panther will query the Github API for new events every 1' minute.&#x20;

In order to set up Github as a log source in Panther, you'll need to authorize Panther in Github and then set up Github as a log source in Panther. There are two ways to authorize Panther to receive Github audit logs:

* **Create a new OAuth App** in Github and provide the app credentials to Panther
* **Generate a Personal Access Token** in Github and provide credentials to Panther

## How to onboard Github logs to Panther

### Option 1: Create a new OAuth App

{% hint style="warning" %}
The steps below can only be performed if you have organization owner permission in your Github organization and a Github Enterprise subscription. If you need to configure multiple integrations for different Github Organizations using the same credentials, you can either use a Personal Access Token or an [OAuth2 App](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app) that is created on the user account, instead of the Organization account. If any Organizations [have enabled OAuth2 App Access Restrictions](https://docs.github.com/en/organizations/restricting-access-to-your-organizations-data/enabling-oauth-app-access-restrictions-for-your-organization), the app must be first approved by an Organization admin.
{% endhint %}

1. Log in to your Github Enterprise account.
2. On the homepage of your organization's account, click on the **Settings** tab.
3. Scroll to the bottom of the page and click on **Developer Settings** and then **OAuth Apps.**
4. Click on **Register an application.** Fill in the form:
   * Enter a memorable application name into the **Name** field e.g. `Panther Integration`.&#x20;
   * Enter your Panther instance's primary URL into the **Homepage URL** field e.g. [`https://test.runpanther.xyz`](https://snowflake.staging.runpanther.xyz/)
   * Copy the **Redirect URL** from Panther and paste into the **Authorization Callback URL** field.&#x20;
     * To do this, you will need to log into Panther and set up Github as a log source [by following the directions below. ](https://app.gitbook.com/o/-LgddDaIOc7MA4mxoaPa/s/CBgRvjQpW660YkSmuOxK/)Once you've made it to the step where you see a **Redirect URL**, you can copy it and continue setting up your Github app.
5. Once all necessary fields are filled in, click **Register Application.**
6. Once the application is registered, you can view the **Client ID** and generate a new **Client Secret.** Store them in a secure location – you will need them in the next steps.

### Option 2: Generate a personal access token

{% hint style="warning" %}
The steps below can only be performed if you have organization owner permission in your Github organization and a Github Enterprise subscription. You can read more on generating a Personal Access Token in Github [here](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).
{% endhint %}

1. Log into your Github Enterprise account.
2. Click on your profile then click on the **Settings** option.
3. Scroll to the bottom of the page and click on **Developer Settings** and then **Personal Access Token.**
4. Click **Generate new token** and **** enter a memorable token name e.g. `Panther Integration`.&#x20;
5. Select the scopes, or permissions, you'd like to grant this token.
   * Check the boxes next to **admin:org > read:org**.&#x20;
   * You do not need to enable the **write:org** permission.
6. Click **Generate token.**&#x20;
7. Copy the token and store it in a secure location – you will need it in the next steps.

### Create a new Github source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Github** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g, `My Github Audit logs` and the name of the Github organization you want to monitor.&#x20;
6. Click **Continue Setup**.
7. Authorize Panther to receive logs from Github - depending on the option you chose above, follow the steps below:
   * **Use OAuth2 Authorization Flow**: Enter the **App Client ID** and the **Client Secret** that you acquired from Github. You can find this information on the details page of the OAuth app in your Github account once you **register the application.**
   * **Use a Personal Access Token:** Copy the personal access token key and paste it into Personal Access token field.
8. Click **Continue Setup.**
9. You will be presented with the option to **Grant Access.**
10. Click **Authorize \[name of organization].**
11. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
    * This feature sends an error message if logs aren't received within a specified time interval.
12. Click **Finish Setup**.

### Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Branch Policy Override
* Branch Protection Disabled
* Org Auth Modified
* Org IP Allowlist
* Org Modified
* Repo Collaborator Change
* Repo Created
* Repo Hook Modified
* Repo Initial Access
* Repo Visibility Change
* Team Modified
* User Access Key Created
* User Role Updated

Review the files in the [github\_rules](https://github.com/panther-labs/panther-analysis/tree/master/github\_rules) repository to see how these are built.&#x20;

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"** just below the "name" field.
{% endhint %}

### Github.Audit

The audit log allows organization admins to quickly review the actions performed by members of your organization.

Reference: [Github Documentation on Accessing Audit Logs.](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization#using-the-rest-api)

```yaml
schema: GitHub.Audit
parser:
    native:
        name: GitHub.Audit
description: The audit log allows organization admins to quickly review the actions performed by members of your organization.
referenceURL: https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization#using-the-rest-api
version: 0
fields:
    - name: _document_id
      description: Document id for the audit log events
      type: string
    - name: workflow_id
      description: Workflow id if the event is CI workflow
      type: bigint
    - name: workflow_run_id
      description: Workflow run id if the event is CI workflow
      type: bigint
    - name: '@timestamp'
      required: true
      description: Timestamp for the event
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: action
      required: true
      description: The action performed
      type: string
    - name: actor
      description: Actor that performed the action
      type: string
      indicators:
        - username
    - name: created_at
      description: Creation timestamp for audit event
      type: timestamp
      timeFormat: unix_ms
    - name: completed_at
      description: Completion timestamp for audit event
      type: string
    - name: actor_location
      description: Actor location
      type: object
      fields:
        - name: country_code
          required: true
          description: Country code for the actor's location'
          type: string
    - name: org
      description: The Organization where the action was performed
      type: string
    - name: config
      description: Webhook configuration
      type: object
      fields:
        - name: content_type
          description: content type for the webhook
          type: string
        - name: insecure_ssl
          description: Boolean value if ssl connection is secure
          type: string
        - name: url
          description: payload URL for webhook
          type: string
    - name: config_was
      description: Previous webhook configuration
      type: object
      fields:
        - name: content_type
          description: content type for the webhook
          type: string
        - name: insecure_ssl
          description: Boolean value if ssl connection is secure
          type: string
        - name: url
          description: payload URL for webhook
          type: string
    - name: hook_id
      description: Webhook ID
      type: bigint
    - name: name
      description: name of the event action category
      type: string
    - name: active
      description: Webhook is active
      type: boolean
    - name: repo
      description: Name of the repository
      type: string
    - name: visibility
      description: Visibility of the repository
      type: string
    - name: events
      description: List of events which will send webhook payload
      type: array
      element:
        type: string
    - name: user
      description: User added/removed for certain permission
      type: string
      indicators:
        - username
    - name: team
      description: Team name for team category action
      type: string
    - name: event
      description: Workflow event
      type: string
    - name: transport_protocol_name
      description: Transport protocol name for git audit events
      type: string
    - name: transport_protocol
      description: Transport protocol for git audit events
      type: int
    - name: repository
      description: Repository name for git event
      type: string
    - name: repository_public
      description: If the repository for git audit event is public
      type: boolean
```
