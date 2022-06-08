---
description: Panther supports pulling logs directly from Okta
---

# Okta

## Overview

Panther has the ability to fetch Okta events by querying the [Okta System Log API](https://developer.okta.com/docs/reference/api/system-log/). Panther will query the System Log API every 1 minute.

In order for Panther to access the API you need to create a new API token or use an existing one.

## How to Onboard Okta logs to Panther

### Create a new Okta API token

{% hint style="info" %}
To create an API token with permissions to query Okta System Logs, you need to be logged in as an administrator that has the rights to perform your API call's actions. Please refer to [Okta documentation](https://help.okta.com/en/prod/Content/Topics/Security/Administrators.htm?Highlight=administrators) for information on managing Admin roles and their rights.
{% endhint %}

1. Log in as Okta administrator.
2. In the Okta Admin Console, navigate to **Security** > **API.**
3. Click **Create Token.**
4. Enter a memorable name for your token, e.g. `Panther API token`
5. Copy the **Token value** and store it in a secure location. You will need it in the next steps.
   * **Note**: Okta will not display this value again.

### Create a new Okta source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Okta** from the list of available log sources. Click **Start Source Setup**.
5. Select **okta.com** from the Okta domain drop-down.
6. Fill in the following fields:
   * **Name**: A memorable name for the source e.g. `My Okta logs.`
   * **Okta subdomain**: The name of your Okta domain. Should be in the form `https://my-org.okta.com.`
   * **API Token**: The token value from the previous section of our documentation.
   * **Log Types:** Select the log types you would like to monitor.
7. Click **Continue Setup.**
8. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
9. Click **Finish Setup**.

### Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Admin Role Assigned
* API Key Created
* API Key Revoked
* Brute Force Logins
* Geo Improbable Access

Review the files in the [okta\_rules ](https://github.com/panther-labs/panther-analysis/tree/master/okta\_rules)repository to see how these are built.&#x20;

## Supported log types

### Okta.SystemLog

The Okta System Log records system events related to your organization in order to provide an audit trail that can be used to understand platform activity and to diagnose problems.

Reference: [Okta Documentation on System Log APIs.](https://developer.okta.com/docs/reference/api/system-log/)

```yaml
schema: Okta.SystemLog
description: |
    The Okta System Log records system events related to your organization in order to provide an audit trail that can be used to understand platform activity and to diagnose problems.
referenceURL: https://developer.okta.com/docs/reference/api/system-log/
version: 0
fields:
    - name: uuid
      required: true
      description: Unique identifier for an individual event
      type: string
    - name: published
      required: true
      description: Timestamp when event was published
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: eventType
      required: true
      description: Type of event that was published
      type: string
    - name: version
      required: true
      description: Versioning indicator
      type: string
    - name: severity
      required: true
      description: 'Indicates how severe the event is: DEBUG, INFO, WARN, ERROR'
      type: string
    - name: legacyEventType
      description: Associated Events API Action objectType attribute value
      type: string
    - name: displayMessage
      description: The display message for an event
      type: string
    - name: actor
      description: Describes the entity that performed an action
      type: object
      fields:
        - name: id
          required: true
          description: ID of actor
          type: string
        - name: type
          required: true
          description: Type of actor
          type: string
        - name: alternateId
          description: Alternative id of the actor
          type: string
          indicators:
            - email
        - name: displayName
          description: Display name of the actor
          type: string
        - name: details
          description: Details about the actor
          type: json
        - name: detailEntry
          description: Detail entry
          type: json
    - name: client
      description: The client that requested an action
      type: object
      fields:
        - name: id
          description: For OAuth requests this is the id of the OAuth client making the request. For SSWS token requests, this is the id of the agent making the request.
          type: string
        - name: userAgent
          description: The user agent used by an actor to perform an action
          type: object
          fields:
            - name: browser
              description: If the client is a web browser, this field identifies the type of web browser (e.g. CHROME, FIREFOX)
              type: string
            - name: os
              description: The Operating System the client runs on (e.g. Windows 10)
              type: string
            - name: rawUserAgent
              description: A raw string representation of the user agent, formatted according to section 5.5.3 of HTTP/1.1 Semantics and Content. Both the browser and the OS fields can be derived from this field.
              type: string
        - name: geographicalContext
          description: The physical location where the client made its request from
          type: object
          fields:
            - name: geolocation
              description: Contains the geolocation coordinates (latitude, longitude)
              type: object
              fields:
                - name: lat
                  description: Latitude
                  type: float
                - name: lon
                  description: Longitude
                  type: float
            - name: city
              description: The city encompassing the area containing the geolocation coordinates, if available (e.g. Seattle, San Francisco)
              type: string
            - name: state
              description: Full name of the state/province encompassing the area containing the geolocation coordinates (e.g. Montana, Incheon)
              type: string
            - name: country
              description: Full name of the country encompassing the area containing the geolocation coordinates (e.g. France, Uganda)
              type: string
            - name: postalCode
              description: Full name of the country encompassing the area containing the geolocation coordinates (e.g. France, Uganda)
              type: string
        - name: zone
          description: The name of the Zone that the client's location is mapped to
          type: string
        - name: ipAddress
          description: Ip address that the client made its request from
          type: string
          indicators:
            - ip
        - name: device
          description: Type of device that the client operated from (e.g. Computer)
          type: string
    - name: request
      description: The request that initiated an action
      type: object
      fields:
        - name: ipChain
          description: If the incoming request passes through any proxies, the IP addresses of those proxies will be stored here in the format (clientIp, proxy1, proxy2, ...).
          type: array
          element:
            type: object
            fields:
                - name: ip
                  description: IP address
                  type: string
                  indicators:
                    - ip
                - name: geographicalContext
                  description: Geographical context of the IP address
                  type: object
                  fields:
                    - name: geolocation
                      description: Contains the geolocation coordinates (latitude, longitude)
                      type: object
                      fields:
                        - name: lat
                          description: Latitude
                          type: float
                        - name: lon
                          description: Longitude
                          type: float
                    - name: city
                      description: The city encompassing the area containing the geolocation coordinates, if available (e.g. Seattle, San Francisco)
                      type: string
                    - name: state
                      description: Full name of the state/province encompassing the area containing the geolocation coordinates (e.g. Montana, Incheon)
                      type: string
                    - name: country
                      description: Full name of the country encompassing the area containing the geolocation coordinates (e.g. France, Uganda)
                      type: string
                    - name: postalCode
                      description: Full name of the country encompassing the area containing the geolocation coordinates (e.g. France, Uganda)
                      type: string
                - name: version
                  description: IP version
                  type: string
                - name: source
                  description: Details regarding the source
                  type: string
    - name: outcome
      description: The outcome of an action
      type: object
      fields:
        - name: result
          description: 'Result of the action: SUCCESS, FAILURE, SKIPPED, ALLOW, DENY, CHALLENGE, UNKNOWN'
          type: string
        - name: reason
          description: Reason for the result, for example INVALID_CREDENTIALS
          type: string
    - name: target
      description: Zero or more targets of an action
      type: array
      element:
        type: object
        fields:
            - name: id
              required: true
              description: ID of target
              type: string
            - name: type
              required: true
              description: Type of target
              type: string
            - name: alternateId
              description: Alternative id of the target
              type: string
            - name: displayName
              description: Display name of the target
              type: string
            - name: details
              description: Details about the target
              type: json
            - name: detailEntry
              description: Detail entry
              type: json
    - name: transaction
      description: The transaction details of an action
      type: object
      fields:
        - name: id
          description: Unique identifier for this transaction.
          type: string
        - name: type
          description: Describes the kind of transaction. WEB indicates a web request. JOB indicates an asynchronous task.
          type: string
        - name: detail
          description: Details for this transaction.
          type: json
    - name: debugContext
      description: The debug request data of an action
      type: object
      fields:
        - name: debugData
          description: Dynamic field containing miscellaneous information dependent on the event type.
          type: json
    - name: authenticationContext
      description: The authentication data of an action
      type: object
      fields:
        - name: authenticationProvider
          description: The system that proves the identity of an actor using the credentials provided to it
          type: string
        - name: authenticationStep
          description: The zero-based step number in the authentication pipeline. Currently unused and always set to 0.
          type: int
        - name: credentialProvider
          description: A credential provider is a software service that manages identities and their associated credentials. When authentication occurs via credentials provided by a credential provider, that credential provider will be recorded here.
          type: string
        - name: credentialType
          description: The underlying technology/scheme used in the credential
          type: string
        - name: issuer
          description: The specific software entity that created and issued the credential.
          type: object
          fields:
            - name: id
              description: Varies depending on the type of authentication. If authentication is SAML 2.0, id is the issuer in the SAML assertion. For social login, id is the issuer of the token.
              type: string
            - name: type
              description: Information regarding issuer and source of the SAML assertion or token.
              type: string
        - name: externalSessionId
          description: A proxy for the actor's session ID
          type: string
        - name: interface
          description: The third party user interface that the actor authenticates through, if any.
          type: string
        - name: authenticatorProvider
          description: 'DEPRECATED: This field is kept here for backwards compatibility.'
          type: string
    - name: securityContext
      description: The security data of an action
      type: object
      fields:
        - name: asNumber
          description: Autonomous system number associated with the autonomous system that the event request was sourced to
          type: bigint
        - name: asOrg
          description: Organization associated with the autonomous system that the event request was sourced to
          type: string
        - name: isp
          description: Internet service provider used to sent the event's request
          type: string
        - name: domain
          description: The domain name associated with the IP address of the inbound event request
          type: string
          indicators:
            - domain
        - name: isProxy
          description: Specifies whether an event's request is from a known proxy
          type: boolean
```
