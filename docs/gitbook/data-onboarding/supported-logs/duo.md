---
description: Panther supports pulling logs directly from Duo
---

# Duo

## Overview

Panther can collect the following Duo logs via the [Duo API](https://duo.com/docs/adminapi#logs):

* [Authentication Logs (v2)](https://duo.com/docs/adminapi#authentication-logs)
* [Administrator Logs](https://duo.com/docs/adminapi#administrator-logs)
* [Telephony Logs](https://duo.com/docs/adminapi#telephony-logs)
* [Offline Enrollment Logs](https://duo.com/docs/adminapi#offline-enrollment-logs)

In order for Panther to access the Duo API you need to create a new 'Duo App' and provide the app credentials to Panther.

## How to onboard Duo logs to Panther

### Create a Duo application

1.  Follow the instructions [here](https://duo.com/docs/adminapi#first-steps) to create a new Duo application.

    Note that only administrators with the Owner role can create or modify an Admin API application in the Duo Admin Panel.
2. Make sure to grant the application **Grant read log** permissions.

### Create a new Duo source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **Duo** from the list of available log sources. Click **Start Source Setup**.
5. On the next screen, enter a memorable name for the source e.g., `My Duo logs` and select the type of logs you want to monitor.
6. Click **Continue Setup.**
7. Fill in the fields below:
   * **Integration Key**: The integration key of the Duo app.
   * **Secret Key**: The secret key of the Duo app.
   * **API Hostname**: The API hostname of the Duo app.
8. Click **Continue Setup.**
9. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
10. Click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as "required: true" just below the "name" field.
{% endhint %}

### Duo.Administrator

Duo administrator log events.

Reference: [Duo Documentation on Administrator Logs.](https://duo.com/docs/adminapi#administrator-logs)

```yaml
schema: Duo.Administrator
parser:
    native:
        name: Duo.Administrator
description: Duo administrator log events.
referenceURL: https://duo.com/docs/adminapi#administrator-logs
version: 0
fields:
    - name: action
      required: true
      description: The type of change that was performed.
      type: string
    - name: description
      description: String detailing what changed, either as free-form text or serialized JSON.
      type: string
    - name: isotimestamp
      required: true
      description: ISO8601 timestamp of the event.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: object
      description: 'The object that was acted on. For example: "jsmith" (for users), "(555) 713-6275 x456" (for phones), or "HOTP 8-digit 123456" (for tokens).'
      type: string
    - name: timestamp
      description: Unix timestamp of the event.
      type: timestamp
      timeFormat: unix
    - name: username
      required: true
      description: 'The full name of the administrator who performed the action in the Duo Admin Panel. If the action was performed with the API this will be "API". Automatic actions like deletion of inactive users have "System" for the username. Changes synchronized from Directory Sync will have a username of the form (example) "AD Sync: name of directory".'
      type: string
      indicators:
        - username
```

### Duo.Authentication

Duo authentication log events(v2).

Reference: [Duo Documentation on Authentication Logs.](https://duo.com/docs/adminapi#authentication-logs)

```yaml
schema: Duo.Authentication
parser:
    native:
        name: Duo.Authentication
description: Duo authentication log events(v2).
referenceURL: https://duo.com/docs/adminapi#authentication-logs
version: 0
fields:
    - name: access_device
      description: Browser, plugin, and operating system information for the endpoint used to access the Duo-protected resource. Values present only when the application accessed features Duo’s inline browser prompt.
      type: object
      fields:
        - name: browser
          description: The web browser used for access.
          type: string
        - name: browser_version
          description: The browser version.
          type: string
        - name: flash_version
          description: The Flash plugin version used, if present, otherwise "uninstalled".
          type: string
        - name: hostname
          description: The hostname, if present, otherwise "null".
          type: string
          indicators:
            - hostname
        - name: ip
          description: The access device's IP address, if present, otherwise "null".
          type: string
          indicators:
            - ip
        - name: is_encryption_enabled
          description: Reports the disk encryption state as detected by the Duo Device Health app. One of "true", "false", or "unknown".
          type: string
        - name: is_firewall_enabled
          description: Reports the firewall state as detected by the Duo Device Health app. One of "true", "false", or "unknown".
          type: string
        - name: is_password_set
          description: Reports the system password state as detected by the Duo Device Health app. One of "true", "false", or "unknown".
          type: string
        - name: java_version
          description: The Java plugin version used, if present, otherwise "uninstalled".
          type: string
        - name: location
          description: The GeoIP location of the access device, if available. The response may not include all location parameters.
          type: object
          fields:
            - name: city
              description: The city name.
              type: string
            - name: country
              description: The country code.
              type: string
            - name: state
              description: The state, county, province, or prefecture.
              type: string
        - name: os
          description: The device operating system name.
          type: string
        - name: os_version
          description: The device operating system version.
          type: string
        - name: security_agents
          description: Reports the security agents present on the endpoint as detected by the Duo Device Health app.
          type: array
          element:
            type: json
    - name: alias
      description: The username alias used to log in. No value if the user logged in with their username instead of a username alias.
      type: string
      indicators:
        - username
    - name: application
      description: Information about the application accessed.
      type: object
      fields:
        - name: key
          description: The application's integration_key.
          type: string
        - name: name
          description: The application's name.
          type: string
    - name: auth_device
      description: Information about the device used to approve or deny authentication.
      type: object
      fields:
        - name: ip
          description: The IP address of the authentication device.
          type: string
          indicators:
            - ip
        - name: location
          description: The GeoIP location of the authentication device, if available. May not include all location parameters.
          type: object
          fields:
            - name: city
              description: The city name.
              type: string
            - name: country
              description: The country code.
              type: string
            - name: state
              description: The state, county, province, or prefecture.
              type: string
        - name: name
          description: The name of the authentication device.
          type: string
    - name: email
      description: The email address of the user, if known to Duo, otherwise none.
      type: string
      indicators:
        - email
    - name: event_type
      description: 'The type of activity logged. one of: "authentication" or "enrollment".'
      type: string
    - name: factor
      description: 'The authentication factor. One of: "phone_call", "passcode", "yubikey_passcode", "digipass_go_7_token", "hardware_token", "duo_mobile_passcode", "bypass_code", "sms_passcode", "sms_refresh", "duo_push", "u2f_token", "remembered_device", or "trusted_network".'
      type: string
    - name: isotimestamp
      required: true
      description: ISO8601 timestamp of the event.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: ood_software
      description: If authentication was denied due to out-of-date software, shows the name of the software, i.e. "Chrome", "Flash", etc. No value if authentication was successful or authentication denial was not due to out-of-date software.
      type: string
    - name: reason
      description: 'Provide the reason for the authentication attempt result. If result is "SUCCESS" then one of: "allow_unenrolled_user", "allowed_by_policy", "allow_unenrolled_user_on_trusted_network", "bypass_user", "remembered_device", "trusted_location", "trusted_network", "user_approved", "valid_passcode". If result is "FAILURE" then one of: "anonymous_ip", "anomalous_push", "could_not_determine_if_endpoint_was_trusted", "denied_by_policy", "denied_network", "deny_unenrolled_user", "endpoint_is_not_in_management_system", "endpoint_failed_google_verification", "endpoint_is_not_trusted", "factor_restricted", "invalid_management_certificate_collection_state", "invalid_device", "invalid_passcode", "invalid_referring_hostname_provided", "location_restricted", "locked_out", "no_activated_duo_mobile_account", "no_disk_encryption", "no_duo_certificate_present", "touchid_disabled", "no_referring_hostname_provided", "no_response", "no_screen_lock", "no_web_referer_match", "out_of_date", "platform_restricted", "rooted_device", "software_restricted", "user_cancelled", "user_disabled", "user_mistake", "user_not_in_permitted_group", "user_provided_invalid_certificate", or "version_restricted". If result is "ERROR" then: "error". If result is "FRAUD" then: "user_marked_fraud".'
      type: string
    - name: result
      description: 'The result of the authentication attempt. One of: "SUCCESS", "FAILURE", "ERROR", or "FRAUD".'
      type: string
    - name: timestamp
      description: Unix timestamp of the event.
      type: timestamp
      timeFormat: unix
    - name: txid
      required: true
      description: The transaction ID of the event.
      type: string
      indicators:
        - trace_id
    - name: user
      description: Information about the authenticating user.
      type: object
      fields:
        - name: groups
          description: Duo group membership information for the user.
          type: array
          element:
            type: string
        - name: key
          description: The user's user_id.
          type: string
        - name: name
          description: The user's username.
          type: string
          indicators:
            - username
```

### Duo.OfflineEnrollment

Duo Authentication for Windows Logon offline enrollment events.

Reference: [Duo Documentation on Offline Enrollment Logs.](https://duo.com/docs/adminapi#offline-enrollment-logs)

```yaml
schema: Duo.OfflineEnrollment
parser:
    native:
        name: Duo.OfflineEnrollment
description: Duo Authentication for Windows Logon offline enrollment events.
referenceURL: https://duo.com/docs/adminapi#offline-enrollment-logs
version: 0
fields:
    - name: action
      required: true
      description: The offline enrollment operation. One of "o2fa_user_provisioned", "o2fa_user_deprovisioned", or "o2fa_user_reenrolled".
      type: string
    - name: description
      description: Information about the Duo Windows Logon client system as reported by the application.
      type: string
    - name: isotimestamp
      required: true
      description: ISO8601 timestamp of the event.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: object
      required: true
      description: The Duo Windows Logon integration's name.
      type: string
    - name: timestamp
      description: Unix timestamp of the event.
      type: timestamp
      timeFormat: unix
    - name: username
      required: true
      description: The Duo username.
      type: string
      indicators:
        - username
```

### Duo.Telephony

Duo telephony log events.

Reference: [Duo Documentation on Telephony Logs.](https://duo.com/docs/adminapi#telephony-logs)

```yaml
schema: Duo.Telephony
parser:
    native:
        name: Duo.Telephony
description: Duo telephony log events.
referenceURL: https://duo.com/docs/adminapi#telephony-logs
version: 0
fields:
    - name: context
      description: 'How this telephony event was initiated. One of: "administrator login", "authentication", "enrollment", or "verify".'
      type: string
    - name: credits
      description: How many telephony credits this event cost.
      type: int
    - name: isotimestamp
      required: true
      description: ISO8601 timestamp of the event.
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: phone
      required: true
      description: The phone number that initiated this event.
      type: string
    - name: timestamp
      description: Unix timestamp of the event.
      type: timestamp
      timeFormat: unix
    - name: type
      required: true
      description: The event type. Either "sms" or "phone".
      type: string
```
