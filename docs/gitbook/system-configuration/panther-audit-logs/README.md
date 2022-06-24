---
description: Logs of audited activity within your instance of Panther
---

# Panther Audit Logs (Beta)

{% hint style="info" %}
Panther Audit Logs are currently in public Beta. Please share any bug reports and feature requests with your account team.&#x20;
{% endhint %}

## Overview

Panther audit logs provide a read-only history of activity within your Panther deployment.  With Panther audit logs as a log source enabled, you can write detections or query the data lake for audit logs the same way you would with any other security events ingested by Panther.&#x20;

Learn [how to query and write Detections for Panther audit logs](querying-and-writing-detections-for-panther-audit-logs.md) in the documentation.

Audit logging does not currently include an exhaustive list of all activity in Panther (such references to specific log sources, cloud accounts, and destinations).&#x20;

If you have any questions about this feature, reach out to Panther Support.

### Enabling audit logs

Audit logs are automatically generated, but must be enabled by your Panther support team to use them as a log source.

### Retention

Audit logs are retained by default for 5 years in AWS S3.

### Schema

The fields of the audit log are listed below along with information on the fields type and whether it is a required field.

| Attribute           | Description                                                                |                                                       | Required |
| ------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------- | -------- |
| `actionName`        | The action that was attempted.                                             | String                                                | true     |
| `actionDescription` | An optional brief description of the action attempted.                     | String                                                | false    |
| `actionResult`      | The result of the action that was attempted.                               | String - `SUCCEEDED`, `FAILED`, or `PARTIALLY_FAILED` | true     |
| `actionParams`      | The parameters supplied that were relevant to the action being attempted.  | Dict                                                  | false    |
| `actor`             | Identifying information about the actor.                                   | Dict                                                  | true     |
| `actor.id`          | The ID of the actor that attempted the action.                             | String                                                | true     |
| `actor.type`        | The type of actor (user/token).                                            | String -`USER` or `TOKEN`                             | true     |
| `actor.name`        | The name of the actor that attempted the action.                           | String                                                | false    |
| `actor.attributes`  | The attributes of the actor that attempted the action.                     | Dict                                                  | false    |
| `errors`            | Errors encountered while performing the action.                            | List                                                  | false    |
| `errors.message`    | The error message for the error encountered.                               | String                                                | false    |
| `sourceIP`          | The IP address from which the request originated.                          | String                                                | false    |
| `XForwardedFor`     | All IP addresses included in the X-Forwarded-Header.                       | List                                                  | false    |
| `userAgent`         | Information about the actor's browser.                                     | String                                                | false    |
| `timestamp`         | The date/time at which the action was attempted.                           | String                                                | true     |
| `pantherVersion`    | The version of this Panther instance at the time the action was attempted. | String                                                | true     |

