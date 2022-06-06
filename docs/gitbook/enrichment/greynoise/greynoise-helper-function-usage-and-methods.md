# GreyNoise Helper Function Usage and Methods

## Overview

Panther has integrated helper functions to streamline the use of GreyNoise data. This page describes how to use these functions, and what methods are available in the objects they create.

## Creating GreyNoise Object in Rules

There are individual helpers for both GreyNoise datasets (Noise and RIOT). These functions create objects with methods that can be called to return relevant data from the dataset. Below is an example code snippet that shows the creation of these objects:

```python
from panther_greynoise_helpers import (
    GetGreyNoiseObject, GetGreyNoiseRiotObject
    )

def rule(event):
    global noise
    global riot
    noise = GetGreyNoiseObject(event)
    riot = GetGreyNoiseRiotObject(event)
```

Note that the `global` statements are only needed if you intend to use the objects outside of the function in which they were declared.

## Calling Methods on the GreyNoise Objects

The various components of the GreyNoise datasets are available via methods on the Noise and RIOT objects. It's possible for one event that your rule is processing to have multiple fields (such as IP addresses, source and destination IP in a network log). When calling the GreyNoise objects, make sure to specify which field you are looking for.&#x20;

The example below demonstrates calling the `classification` method on the `noise` object we created in the previous example, to determine if the source IP address (`src`) is malicious and if the destination ip (`dest`) is in the RIOT dataset (meaning it is a known safe entity).

```python
if noise.classification('src') == 'malicious':
    return True
if riot.is_riot('dest'):
    return False
```

## Available Methods

### Noise Dataset

The following table shows the available methods for the GreyNoise Noise Object, their expected return values, and if they are available in the Basic or Advanced GreyNoise subscriptions.&#x20;

All methods take the argument of the field you are searching for (`src` or `dest` in the example above) unless otherwise noted.

| Method              | Basic / Advanced | Return Type and Description                                                                                                                                              |
| ------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| subscription\_level | Both             | <p><strong>Takes no arguments</strong></p><p>Returns String "advanced" or "basic" depending on Subscription level</p>                                                    |
| ip\_address         | Both             | <p>Returns String <br>IP address that information is about.</p>                                                                                                          |
| actor               | Both             | The confirmed owner/operator of this IP address.                                                                                                                         |
| classification      | Both             | <p>Returns String</p><p>IP Classification - possible options: benign, unknown, malicious.</p>                                                                            |
| url                 | Both             | <p>Returns String<br>Url to the GreyNoise entry for this IP</p>                                                                                                          |
| is\_bot             | Advanced         | <p>Returns Boolean<br>IP is associated with known bot activity.</p>                                                                                                      |
| cve\_string         | Advanced         | <p>Returns String</p><p>Additional, Optional Argument: limit Default value: 10<br>Space separated string of CVEs the IP has been observed scanning for or exploiting</p> |
| cve\_list           | Advanced         | <p>Returns List<br>Returns all CVEs the IP has been observed scanning for or exploiting</p>                                                                              |
| first\_seen         | Advanced         | <p>Returns Datetime.date object<br>Date of the first observed behavior on the GreyNoise Sensor network</p>                                                               |
| last\_seen          | Advanced         | <p>Returns Datetime.date object<br>Date of last observed behavior on the GreyNoise Sensor network</p>                                                                    |
| asn                 | Advanced         | <p>Returns String<br>ASN number of IP</p>                                                                                                                                |
| category            | Advanced         | <p>Returns String<br>Category of service that IP falls into, such as "hosting"</p>                                                                                       |
| city                | Advanced         | <p>Returns String<br>City where IP is physically located</p>                                                                                                             |
| country             | Advanced         | <p>Returns String<br>Country where IP is Physically located</p>                                                                                                          |
| country\_code       | Advanced         | <p>Returns String<br>Two letter Country Code where IP is Physically located</p>                                                                                          |
| organization        | Advanced         | <p>Returns String<br>Organization that owns the IP</p>                                                                                                                   |
| operating\_system   | Advanced         | <p>Returns String<br>Operating System that has been observed using the IP</p>                                                                                            |
| region              | Advanced         | <p>Returns String <br>State, Province, or other Regional identifier associated with the IP<br></p>                                                                       |
| is\_tor             | Advanced         | <p>Returns Boolean<br>If IP is a part of the TOR network, usually an exit node</p>                                                                                       |
| tags\_list          | Advanced         | <p>Returns List<br>GreyNoise tags associated with IP</p>                                                                                                                 |
| tags\_string        | Advanced         | <p>Returns String<br>Optional Argument: limit<br>Default: 10<br>Space separated string of GreyNoise tags associated with the IP</p>                                      |
| is\_vpn             | Advanced         | <p>Returns Boolean<br>If IP is associated with a VPN service</p>                                                                                                         |
| vpn\_service        | Advanced         | <p>Returns String<br>VPN service associated with IP, if any</p>                                                                                                          |

### RIOT Dataset

The following table shows the available methods for the GreyNoise RIOT object, their expected return values, and if they are available in the Basic or Advanced GreyNoise subscriptions.&#x20;

All methods take the argument of the field you are searching for (`src` or `dest` in the example above) unless otherwise noted.

| Method              | Basic/Advanced | Return Type and Description                                                                                                                                                                                                                    |
| ------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| subscription\_level | Both           | <p><strong>Takes no Arguments</strong><br><strong></strong>Returns String "advanced" or "basic" depending on Subscription level</p>                                                                                                            |
| is\_riot            | Both           | <p>Returns Boolean<br>Indicates if an IP is part of the Riot dataset</p>                                                                                                                                                                       |
| ip\_address         | Both           | <p>Returns String<br>IP that was matched</p>                                                                                                                                                                                                   |
| name                | Both           | <p>Returns String<br>The name of the Provider or service associated with the IP</p>                                                                                                                                                            |
| url                 | Both           | <p>Returns String<br>Url to the GreyNoise entry for this IP</p>                                                                                                                                                                                |
| last\_updated       | Both           | <p>Returns Datetime.date object<br>Date and time when this record was last updated from its source</p>                                                                                                                                         |
| description         | Advanced       | <p>Returns String<br>A description of the provider and what they do.</p>                                                                                                                                                                       |
| explanation         | Advanced       | <p>Returns String</p><p>An explanation of the category type and what may be expected from this provider and category.</p>                                                                                                                      |
| reference           | Advanced       | <p>Returns String<br>Reference URL for information about this provider and/or service</p>                                                                                                                                                      |
| trust\_level        | Advanced       | <p>Returns Integer<br>Defines the trust level assigned to this IP/provider, either 1 or 2. Additional information on trust levels can be found <a href="https://docs.greynoise.io/docs/understanding-greynoise-riot-trust-levels">here</a></p> |
|                     |                |                                                                                                                                                                                                                                                |
