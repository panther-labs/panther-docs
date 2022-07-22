---
description: Connecting Dropbox logs to your Panther Console
---

# Dropbox Logs

## Overview

Panther has the ability to fetch Dropbox events by querying the [Dropbox Business API](https://www.dropbox.com/developers/documentation/http/teams#team\_log-get\_events). Panther will specifically monitor the following Dropbox team events:

* User logging in or out of Dropbox (including device information)
* Changing a user's role in Dropbox
* Adding, editing, viewing, and sharing files and folders and by whom
* Creating and sharing links within your team

### Prerequisites

The Dropbox user authorizing this integration must have the "Team Admin" role credentials.

## How to onboard Dropbox logs to Panther

### Step 1: Create a new app in Dropbox

1. Log in to your business Dropbox account and navigate to the [Dropbox app console](https://www.dropbox.com/developers/apps).
2. Click **Create App**.
3. On the "Create a new app on the DBX Platform" page, fill out the fields:
   1. Choose an API: **** Select **Scoped Access**.
   2. Choose the type of access you need: Select **Full Dropbox**.
   3. Name your app: Fill in a **memorable** **name** for your application.
   4. Click **Create app**.\
      ![](<../../.gitbook/assets/image (42).png>)\

4. When you are redirected to the **app Settings panel**, fill out the page as follows:
   * Copy the values for your **App Key** and **App Secret.**&#x20;
     * You will need these values to access your application in Step 2.
   * Fill in the **Redirect URIs** and press the **Add** button next to it.
     * &#x20;You will find this in the Dropbox log source onboarding flow in Step 2.
5. Navigate to the **Permissions tab.**
6. Under the Team Scopes section, check the **team\_data.member** and **events.read** boxes.\
   ![](<../../.gitbook/assets/image (40).png>)
7. Click **Submit** in the bar located at the bottom of your screen.

### Step 2: Create a new Dropbox log source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Dropbox** from the list of available log sources.
5. Click **Start Source Setup.**
6. Enter a **name** for the source e.g., `My Dropbox logs`.
7. Click **Continue Setup.**
8. On the "Set Credentials" page, paste your **App Key** from Dropbox into the **Client ID** field.
9. Paste your **App Secret** from Dropbox into the **Client Secret** field.&#x20;
10. Click **Continue Setup**.
11. On the "Verify Setup" page, click **Grant Access**.
    * You will be redirected to a Dropbox page to install your app.
12. Click **Allow.**
13. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
    * This feature sends an error message if logs aren't received within a specified time interval.
14. Click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Dropbox.TeamEvent

Contains events for an entire team's activity and provides information about how your team is using Dropbox.

Reference: [Dropbox Documentation on Team Log Events.](https://www.dropbox.com/developers/documentation/http/teams#team\_log-get\_events)

```yaml
schema: Dropbox.TeamEvent
parser:
  native:
    name: Dropbox.TeamEvent
description: Dropbox events help you monitor what is going on with you files and Dropbox environment as a whole.
referenceURL: https://www.dropbox.com/developers/documentation/http/teams#team_log-get_events
version: 0
fields:
  - name: timestamp
    required: true
    description: Timestamp for the event
    type: timestamp
    timeFormat: rfc3339
    isEventTime: true
  - name: event_category
    required: true
    description: The category that this type of action belongs to
    type: object
    fields:
      - name: .tag
        required: true
        description: Tag of the category
        type: string
  - name: event_type
    required: true
    description: The particular type of action taken
    type: object
    fields:
      - name: .tag
        required: true
        description: Tag of the action
        type: string
      - name: description
        description: Description of the action
        type: string
  - name: details
    required: true
    description: The variable event schema applicable to this type of action, instantiated with respect to this particular action
    type: json
  - name: actor
    description: The entity who actually performed the action
    type: object
    fields:
      - name: .tag
        description: Tag of the actor
        type: string
      - name: admin
        description: The admin who did the action
        type: object
        fields:
          - name: .tag
            description: Tag of the member type
            type: string
          - name: account_id
            description: User unique ID
            type: string
          - name: display_name
            description: User display name
            type: string
            indicators:
              - username
          - name: email
            description: User email address
            type: string
            indicators:
              - email
          - name: team_member_id
            description: Team member ID
            type: string
          - name: member_external_id
            description: Team member external ID
            type: string
          - name: team
            description: Details about this user's team for enterprise event
            type: object
            fields:
              - name: display_name
                description: Team display name
                type: string
          - name: trusted_non_team_member_type
            description: Users that are not part of the Dropbox team but are trusted i.e. enterprise admins
            type: object
            fields:
              - name: .tag
                description: Tag of the type
                type: string
      - name: app
        description: The application who did the action
        type: object
        fields:
          - name: app_id
            description: App unique ID
            type: string
          - name: display_name
            description: App display name
            type: string
      - name: reseller
        description: Action done by reseller
        type: object
        fields:
          - name: reseller_name
            description: Reseller name
            type: string
            indicators:
              - username
          - name: reseller_email
            description: Reseller email
            type: string
            indicators:
              - email
      - name: user
        description: The user who did the action
        type: object
        fields:
          - name: .tag
            description: Tag of the member type
            type: string
          - name: account_id
            description: User unique ID
            type: string
          - name: display_name
            description: User display name
            type: string
            indicators:
              - username
          - name: email
            description: User email address
            type: string
            indicators:
              - email
          - name: team_member_id
            description: Team member ID
            type: string
          - name: member_external_id
            description: Team member external ID
            type: string
          - name: team
            description: Details about this user's team for enterprise event
            type: object
            fields:
              - name: display_name
                description: Team display name
                type: string
          - name: trusted_non_team_member_type
            description: Users that are not part of the Dropbox team but are trusted i.e. enterprise admins
            type: object
            fields:
              - name: .tag
                description: Tag of the type
                type: string
  - name: origin
    description: The origin from which the actor performed the action
    type: object
    fields:
      - name: access_method
        description: Indicates the method in which the action was performed
        type: json
      - name: geo_location
        description: Geographic location details
        type: object
        fields:
          - name: ip_address
            description: IP address
            type: string
            indicators:
              - ip
          - name: city
            description: City nme
            type: string
          - name: region
            description: Region name
            type: string
          - name: country
            description: Country code
            type: string
  - name: involve_non_team_member
    description: True if the action involved a non team member either as the actor or as one of the affected users
    type: boolean
  - name: context
    description: The user or team on whose behalf the actor performed the action
    type: object
    fields:
      - name: .tag
        description: Tag of the member type
        type: string
      - name: account_id
        description: User unique ID
        type: string
      - name: display_name
        description: User display name
        type: string
        indicators:
          - username
      - name: email
        description: User email address
        type: string
        indicators:
          - email
      - name: team_member_id
        description: Team member ID
        type: string
      - name: member_external_id
        description: Team member external ID
        type: string
      - name: team
        description: Details about this user's team for enterprise event
        type: object
        fields:
          - name: display_name
            description: Team display name
            type: string
      - name: trusted_non_team_member_type
        description: Users that are not part of the Dropbox team but are trusted i.e. enterprise admins
        type: object
        fields:
          - name: .tag
            description: Tag of the type
            type: string
  - name: participants
    description: Zero or more users and/or groups that are affected by the action. Note that this list doesn't include any actors or users in context
    type: array
    element:
      type: object
      fields:
        - name: group
          description: Group details
          type: object
          fields:
            - name: display_name
              description: The name of this group
              type: string
            - name: group_id
              description: The unique ID of this group
              type: string
            - name: external_id
              description: External group ID
              type: string
        - name: user
          description: A user with a Dropbox account
          type: object
          fields:
            - name: .tag
              description: Tag of the member type
              type: string
            - name: account_id
              description: User unique ID
              type: string
            - name: display_name
              description: User display name
              type: string
              indicators:
                - username
            - name: email
              description: User email address
              type: string
              indicators:
                - email
            - name: team_member_id
              description: Team member ID
              type: string
            - name: member_external_id
              description: Team member external ID
              type: string
            - name: team
              description: Details about this user's team for enterprise event
              type: object
              fields:
                - name: display_name
                  description: Team display name
                  type: string
            - name: trusted_non_team_member_type
              description: Users that are not part of the Dropbox team but are trusted i.e. enterprise admins
              type: object
              fields:
                - name: .tag
                  description: Tag of the type
                  type: string
  - name: assets
    description: Zero or more content assets involved in the action
    type: array
    element:
      type: json
```

