---
description: Panther supports pulling logs directly from Asana
---

# Asana

## Overview

Panther has the ability to fetch Asana audit logs by querying the [Asana Audit Log API](https://asana.com/guide/help/api/audit-log-api). The below steps outline how to connect your Asana logs to the Panther Console.

## How to onboard Asana logs to Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Asana** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g., `My Asana logs`.&#x20;
6. Click **Continue Setup**.
7. Enter the credentials required for the integration.&#x20;
   * Open a new browser tab and [Sign in](https://app.asana.com/-/login) to your Asana account. **Note:** Log in as administrator.
   * Click your profile picture at the top right. Click **Admin Console** and then click **Settings** on the left.
   * At the bottom of the page you'll find the **Domain ID**. Copy and paste it into the **Organization Id** field in Panther.
   * In your Asana account, click **Apps** on the left sidebar.
   * At the bottom of the page, click **Add Service Account and s**pecify a name.&#x20;
   * Copy the token and then click **Save changes**.
8. Navigate back to the Panther Console and paste the Asana token into the **Service Account Token** field in Panther.
9. Click **Continue Setup**.
10. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
    * This feature sends an error message if logs aren't received within a specified time interval.
11. Click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as "required: true" just below the "name" field.
{% endhint %}

### Asana.Audit

The Audit Logs allow you to monitor and act upon critical events in your organization's Asana instance.

Reference: [Asana Documentation on Audit Log Events.](https://developers.asana.com/docs/audit-log-events)

```yaml
schema: Asana.Audit
parser:
    native:
        name: Asana.Audit
description: The Audit Logs allows you to monitor and act upon critical events in your organization's Asana instance.
referenceURL: https://developers.asana.com/docs/audit-log-events
version: 0
fields:
    - name: gid
      required: true
      description: Global unique identifier of the AuditLogEvent.
      type: string
    - name: actor
      required: true
      description: User that triggered the event.
      type: object
      fields:
        - name: actor_type
          description: Type of actor.
          type: string
        - name: email
          description: Email of the actor, if it is a user.
          type: string
          indicators:
            - email
        - name: gid
          description: Global unique identifier of the actor, if it is a user.
          type: string
        - name: name
          description: Name of the actor, if it is a user.
          type: string
          indicators:
            - username
    - name: context
      description: Context from which this event originated.
      type: object
      fields:
        - name: api_authentication_method
          description: Authentication method used in the context of an API request.
          type: string
        - name: client_ip_address
          description: IP address of the client that initiated the event.
          type: string
          indicators:
            - ip
        - name: context_type
          description: Type of context.
          type: string
        - name: oauth_app_name
          description: Name of the OAuth App that initiated the event.
          type: string
        - name: user_agent
          description: User agent of the client that initiated the event.
          type: string
    - name: created_at
      required: true
      description: The time the event was created.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: details
      description: Event specific details. The schema depends on event type.
      type: json
    - name: event_category
      description: Category that this event type belongs to.
      type: string
    - name: event_type
      required: true
      description: Type of the event.
      type: string
    - name: resource
      description: The primary object that was affected by this event.
      type: object
      fields:
        - name: email
          description: The email of the resource, if applicable.
          type: string
          indicators:
            - email
        - name: gid
          description: Global unique identifier of the resource.
          type: string
        - name: name
          description: The name of the resource.
          type: string
        - name: resource_subtype
          description: The subtype of resource.
          type: string
        - name: resource_type
          description: The type of resource.
          type: string
```
