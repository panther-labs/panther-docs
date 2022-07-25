---
description: Panther supports pulling logs directly from Snyk
---

# Snyk Logs

## Overview

Panther has the ability to fetch Snyk audit logs by querying the [Snyk Audit API](https://snyk.docs.apiary.io/#reference/audit-logs). Panther is specifically monitoring the following Snyk events:

* User logged in and out of Snyk
* User's role was changed in Snyk
* License policy was modified and by whom
* Service account was created or deleted.

In order to set up Snyk as a log source in Panther, you'll need to authorize Panther in Snyk by generating an access token in your Snyk account and then set up Snyk as a log source in Panther.&#x20;

## How to onboard Snyk logs to Panther

### Step 1: Generate an API token in Snyk

1. Go to the **General Account Settings** in [your Snyk account](https://app.snyk.io/account).
2. In the KEY field, **click to show** select and copy the value in that field. Store this in a secure location, as you will need it in the next steps.\
   ![](<../../.gitbook/assets/image (12) (1) (1) (2) (1) (1).png>)



### Step 2: Create a new Snyk log source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Snyk** from the list of available log sources. Click **Start Source Setup.**
5. On the next screen, enter in a memorable name for the source e.g. `My Snyk logs`.
6. Click **Continue Setup.**
7. On the **Set Credentials** page, fill in the form:
   * Enter in your Snyk's organization ID.
   * Paste the **API token** from your Snyk account into the API token field.
   * Click **Continue Setup**.
8. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
9. Click **Finish Setup**.

{% hint style="warning" %}
**Note:** By default, Snyk logs do not contain human-readable values for objects such as vaults and login credentials. Please [see our guide about using Lookup Tables](https://docs.panther.com/guides/using-lookup-tables-1password-uuids) to translate Universally Unique Identifier (UUID) values into human-readable names.
{% endhint %}

## Supported log types

{% hint style="info" %}
Required fields in the schemas are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Snyk.GroupAudit

Snyk.GroupAudit item usage.

Reference: [https://snyk.docs.apiary.io/#reference/audit-logs](https://snyk.docs.apiary.io/#reference/audit-logs)

```yaml
schema: Snyk.GroupAudit
parser:
  native:
    name: Snyk.GroupAudit
description: Audit logs of your group.
referenceURL: https://snyk.docs.apiary.io/#reference/audit-logs/get-group-level-audit-logs
version: 0
fields:
  - name: groupId
    required: true
    description: The group id
    type: string
  - name: orgId
    required: true
    description: The organization id
    type: string
  - name: userId
    required: true
    description: The user id
    type: string
    indicators:
      - username
  - name: projectId
    description: The project id
    type: string
  - name: event
    required: true
    description: The event type
    type: string
  - name: created
    required: true
    description: The date and time of the event in rfc3339 standard format
    type: timestamp
    timeFormat: rfc3339
    isEventTime: true
  - name: content
    description: The content relating to the event
    type: json
```

### Snyk.OrgAudit

Snyk.OrgAudit item usage.

Reference: [https://snyk.docs.apiary.io/#reference/audit-logs](https://snyk.docs.apiary.io/#reference/audit-logs)

```yaml
schema: Snyk.OrgAudit
parser:
  native:
    name: Snyk.OrgAudit
description: Audit logs of your organization.
referenceURL: https://snyk.docs.apiary.io/#reference/audit-logs/organization-level-audit-logs/get-organization-level-audit-logs
version: 0
fields:
  - name: groupId
    required: true
    description: The group id
    type: string
  - name: orgId
    required: true
    description: The organization id
    type: string
  - name: userId
    required: true
    description: The user id
    type: string
    indicators:
      - username
  - name: projectId
    description: The project id
    type: string
  - name: event
    required: true
    description: The event type
    type: string
  - name: created
    required: true
    description: The date and time of the event in rfc3339 standard format
    type: timestamp
    timeFormat: rfc3339
    isEventTime: true
  - name: content
    description: The content relating to the event
    type: json
```

