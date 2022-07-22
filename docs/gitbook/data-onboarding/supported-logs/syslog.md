---
description: Connecting Syslog logs to your Panther Console
---

# Syslog Logs

## Overview

Panther supports ingesting Syslog logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3, SQS, and CloudWatch.

## How to onboard Syslog logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
     * [AWS CloudWatch](https://docs.panther.com/data-onboarding/data-transports/cwl-source)
2. Configure Syslog to push logs to the Data Transport source.
   * See Syslog documentation for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in all tables are in **bold**.
{% endhint %}

### Syslog.RFC3164

Syslog parser for the RFC3164 format (ie. BSD-syslog messages)&#x20;

Reference: [Syslog Documentation on RFC3164 BSD Protocol.](https://datatracker.ietf.org/doc/html/rfc3164)

| Column                | Type        | Description                                                                                                                      |
| --------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **`priority`**        | `smallint`  | Priority is calculated by (Facility \* 8 + Severity). The lower this value, the higher importance of the log message.            |
| **`facility`**        | `smallint`  | Facility value helps determine which process created the message. Eg: 0 = kernel messages, 3 = system daemons.                   |
| **`severity`**        | `smallint`  | Severity indicates how severe the message is. Eg: 0=Emergency to 7=Debug.                                                        |
| `timestamp`           | `timestamp` | Timestamp of the syslog message in UTC.                                                                                          |
| `hostname`            | `string`    | Hostname identifies the machine that originally sent the syslog message.                                                         |
| `appname`             | `string`    | Appname identifies the device or application that originated the syslog message.                                                 |
| `procid`              | `string`    | ProcID is often the process ID, but can be any value used to enable log analyzers to detect discontinuities in syslog reporting. |
| `msgid`               | `string`    | MsgID identifies the type of message. For example, a firewall might use the MsgID 'TCPIN' for incoming TCP traffic.              |
| `message`             | `string`    | Message contains free-form text that provides information about the event.                                                       |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                                                             |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                                                                |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                                                       |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                                                   |
| `p_source_id`         | `string`    | Panther added field with the source id                                                                                           |
| `p_source_label`      | `string`    | Panther added field with the source label                                                                                        |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                                                      |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                                                      |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                                       |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                                        |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                                    |

### Syslog.RFC5424

Syslog parser for the RFC5424 format.

Reference: [Syslog Documentation on RFC5424 Protocol.](https://datatracker.ietf.org/doc/html/rfc5424)

| Column                | Type                                 | Description                                                                                                                      |
| --------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| **`priority`**        | `smallint`                           | Priority is calculated by (Facility \* 8 + Severity). The lower this value, the higher importance of the log message.            |
| **`facility`**        | `smallint`                           | Facility value helps determine which process created the message. Eg: 0 = kernel messages, 3 = system daemons.                   |
| **`severity`**        | `smallint`                           | Severity indicates how severe the message is. Eg: 0=Emergency to 7=Debug.                                                        |
| **`version`**         | `int`                                | Version of the syslog message protocol. RFC5424 mandates that version cannot be 0, so a 0 value signals no version.              |
| `timestamp`           | `timestamp`                          | Timestamp of the syslog message in UTC.                                                                                          |
| `hostname`            | `string`                             | Hostname identifies the machine that originally sent the syslog message.                                                         |
| `appname`             | `string`                             | Appname identifies the device or application that originated the syslog message.                                                 |
| `procid`              | `string`                             | ProcID is often the process ID, but can be any value used to enable log analyzers to detect discontinuities in syslog reporting. |
| `msgid`               | `string`                             | MsgID identifies the type of message. For example, a firewall might use the MsgID 'TCPIN' for incoming TCP traffic.              |
| `structured_data`     | `{   string:{     string:string } }` | StructuredData provides a mechanism to express information in a well defined and easily parsable format.                         |
| `message`             | `string`                             | Message contains free-form text that provides information about the event.                                                       |
| **`p_log_type`**      | `string`                             | Panther added field with type of log                                                                                             |
| **`p_row_id`**        | `string`                             | Panther added field with unique id (within table)                                                                                |
| **`p_event_time`**    | `timestamp`                          | Panther added standardize event time (UTC)                                                                                       |
| **`p_parse_time`**    | `timestamp`                          | Panther added standardize log parse time (UTC)                                                                                   |
| `p_source_id`         | `string`                             | Panther added field with the source id                                                                                           |
| `p_source_label`      | `string`                             | Panther added field with the source label                                                                                        |
| `p_any_ip_addresses`  | `[string]`                           | Panther added field with collection of ip addresses associated with the row                                                      |
| `p_any_domain_names`  | `[string]`                           | Panther added field with collection of domain names associated with the row                                                      |
| `p_any_sha1_hashes`   | `[string]`                           | Panther added field with collection of SHA1 hashes associated with the row                                                       |
| `p_any_md5_hashes`    | `[string]`                           | Panther added field with collection of MD5 hashes associated with the row                                                        |
| `p_any_sha256_hashes` | `[string]`                           | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                                    |
