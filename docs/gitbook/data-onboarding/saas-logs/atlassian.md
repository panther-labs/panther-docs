---
description: Panther supports pulling logs directly from Atlassian.
---

# Atlassian

## Overview

Panther has the ability to fetch Atlassian event logs by querying the [Atlassian Organizations REST API](https://developer.atlassian.com/cloud/admin/organization/rest/intro/). Panther is specifically monitoring the following Atlassian events:

* Administrative actions, related to settings or other organization pages&#x20;
* Actions that organization admins take related to the organization’s security policies

In order to set up Atlassian as a log source in Panther, you'll need to authorize Panther in Atlassian by generating an API key in your Atlassian account and then set up Atlassian as a log source in Panther.&#x20;

## How to onboard Atlassian logs to Panther

### Step 1: Generate an API key in Atlassian

1. From your organization at [admin.atlassian.com](http://admin.atlassian.com/), select **Settings** > **API keys**.
2. Click **Create API key** in the top right.
3. Enter a descriptive API key name.
   * By default, the key expires one week after creation.&#x20;
   * To change the expiration date, pick a new date under **Expires on**.&#x20;
   * **Note:** The maximum you can extend your expiration date is up to one year from creation date.
4. Click **Create** to save the API key.
5. Copy the values for your **Organization ID** and **API key**.&#x20;
   * You'll need these values to access your organization in Step 2.
   * Make sure you store these values in a safe place, as Atlassian will not display them again.
6. Click **Done**. The new key will appear in your list of API keys.

**Note:** If you have trouble creating the API key, reference [Atlassian's docs](https://developer.atlassian.com/cloud/admin/organization/rest/intro/).



### Step 2: Create a new Atlassian log source in Panther

1. Log in to your Panther Console.
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu.
3. Click **Create New.**
4. Select **Atlassian** from the list of available log sources, then click **Start Source Setup**.
5. Enter a descriptive name for the source e.g. `My Atlassian Event logs` and then click **Continue Setup.**
6. On the Set Credentials page, fill in the form:&#x20;
   * **Organization**: Enter your Atlassian organization ID that you generated in the previous steps of this documentation.
   * **API Key**: Enter your Atlassian API Key that you generated in the previous steps of this documentation.
7. Click **Continue Setup**.
8. On the confirmation screen, you can optionally set up a log drop-off alarm, which sends an error message if logs aren't received within a specified time interval. When you are finished, click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Atlassian.Audit

The audit log of events from an organization.

Reference: [Atlassian Documentation on Audit Logs & Events.](https://developer.atlassian.com/cloud/admin/organization/rest/api-group-orgs/#api-orgs-orgid-events-get)

```yaml
schema: Atlassian.Audit
parser:
    native:
        name: Atlassian.Audit
description: The audit log of events from an organization.
referenceURL: https://developer.atlassian.com/cloud/admin/organization/rest/api-group-orgs/#api-orgs-orgid-events-get
version: 0
fields:
    - name: type
      required: true
      description: Type name of the event object
      type: string
    - name: id
      required: true
      description: Unique identifier of the event object
      type: string
    - name: attributes
      required: true
      description: Attributes of the event object
      type: object
      fields:
        - name: time
          description: The date and time of the event
          type: string
          timeFormat: rfc3339
          isEventTime: true
        - name: action
          description: Kind of action associated with the event. The complete list can be accessed with event-actions API
          type: string
        - name: actor
          description: Actor associated with the event
          type: object
          fields:
            - name: id
              description: Unique identifier of the event actor
              type: string
            - name: name
              description: Name of the actor who performed the event
              type: string
              indicators:
                - username
            - name: email
              description: Email of the actor who performed the event
              type: string
              indicators:
                - email
            - name: links
              description: Profile of the actor whc performed the event
              type: object
              fields:
                - name: self
                  description: The event self link
                  type: string
                - name: alt
                  description: The event alt link
                  type: string
        - name: context
          description: One or more entities that the action was performed against
          type: array
          element:
            type: object
            fields:
                - name: id
                  description: Unique identifier of the event context
                  type: string
                - name: type
                  description: Event context type
                  type: string
                - name: attributes
                  description: Event context attributes
                  type: json
                - name: links
                  description: Event context self or alt link
                  type: object
                  fields:
                    - name: self
                      description: The event self link
                      type: string
                    - name: alt
                      description: The event alt link
                      type: string
                  indicators:
                    - url
        - name: container
          description: List of containers associated with the events
          type: array
          element:
            type: object
            fields:
                - name: id
                  description: Unique identifier of the event container
                  type: string
                - name: type
                  description: Type name of the event container object
                  type: string
                - name: attributes
                  description: Attributes of the event container object
                  type: json
                - name: links
                  description: Links for the event container object
                  type: object
                  fields:
                    - name: self
                      description: The event self link
                      type: string
                    - name: alt
                      description: The event alt link
                      type: string
        - name: location
          description: Location where the action was performed
          type: object
          fields:
            - name: ip
              description: IP address of the actor location
              type: string
              indicators:
                - ip
            - name: geo
              description: Geo location of the IP address
              type: string
            - name: countryName
              description: Country location according to the IP address
              type: string
            - name: regionName
              description: Region location according to the IP address
              type: string
            - name: city
              description: City location according to the IP address
              type: string
    - name: message
      description: Message associated with the event object
      type: object
      fields:
        - name: content
          description: Message content associated with the event
          type: string
        - name: format
          description: Message format with the event
          type: string
    - name: relations
      description: Relations associated with the event object
      type: json
    - name: links
      required: true
      description: URL to fetch this resource
      type: object
      fields:
        - name: self
          description: The event self link
          type: string
        - name: alt
          description: The event alt link
          type: string

```
