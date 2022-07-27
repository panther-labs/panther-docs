---
description: Connecting JAMF Pro logs to your Panther Console
---

# JAMF Pro Logs

## Overview

Panther supports ingesting JAMF Pro logs via Amazon Web Services (AWS) S3 as a [Data Transport](../data-transports/).

{% hint style="info" %}
**Note:** A [JAMF Premium Cloud add-on](https://resources.jamf.com/documents/products/documentation/jamf-premium-cloud.pdf) is required to connect JAMF Pro logs to Panther.
{% endhint %}

## How to onboard JAMF Pro logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option via an [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3).
2. Configure JAMF Pro to push logs to the Data Transport source.
   * See JAMF's documentation for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Jamfpro.Login

Login events into JAMF Pro itself.

Reference: [JAMF Documentation on Event Logs](https://docs.jamf.com/10.35.0/jamf-pro/documentation/Event\_Logs.html).

```yaml
fields:
  - name: ipAddress
    type: string
    description: IP Address that started the request
    indicators:
      - ip
  - name: username
    required: true
    description: Username of the account
    indicators:
      - username
    type: string
  - name: status
    required: true
    type: string
    description: The status of the login request
  - name: entryPoint
    required: true
    type: string
    description: The method used to login. Either Single Sign On, Universal API or Unknown
  - name: timestamp
    required: true
    type: timestamp
    description: Login timestamp
    isEventTime: true
    timeFormat: '%Y-%m-%dT%H:%M:%S,%f'
```
