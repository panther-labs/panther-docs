---
description: Connecting Fluentd logs to your Panther Console
---

# Fluentd

## Overview

Panther supports ingesting Fluentd logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3 and SQS.

## How to onboard Fluentd logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
2. Configure Fluentd to push logs to the Data Transport source.
   * See Fluentd's documentation for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in all the tables are in **bold.**
{% endhint %}

### Fluentd.Syslog3164

Fluentd syslog parser for the RFC3164 format (ie. BSD-syslog messages)&#x20;

Reference: [Fluentd Documentation on Syslog RFC-3164 Parser.](https://docs.fluentd.org/parser/syslog#rfc3164-log)

| Column                | Type        | Description                                                                                                                      |
| --------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `pri`                 | `smallint`  | Priority is calculated by (Facility \* 8 + Severity). The lower this value, the higher importance of the log message.            |
| **`host`**            | `string`    | Hostname identifies the machine that originally sent the syslog message.                                                         |
| **`ident`**           | `string`    | Appname identifies the device or application that originated the syslog message.                                                 |
| `pid`                 | `bigint`    | ProcID is often the process ID, but can be any value used to enable log analyzers to detect discontinuities in syslog reporting. |
| **`message`**         | `string`    | Message contains free-form text that provides information about the event.                                                       |
| **`time`**            | `timestamp` | Timestamp of the syslog message in UTC.                                                                                          |
| **`tag`**             | `string`    | Tag of the syslog message                                                                                                        |
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

### Fluentd.Syslog5424

Fluentd syslog parser for the RFC5424 format (ie. BSD-syslog messages)&#x20;

Reference: [Fluentd Documentation for Syslog RFC-5424 Parser.](https://docs.fluentd.org/parser/syslog#rfc5424-log)

| Column                | Type        | Description                                                                                                                      |
| --------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `pri`                 | `smallint`  | Priority is calculated by (Facility \* 8 + Severity). The lower this value, the higher importance of the log message.            |
| **`host`**            | `string`    | Hostname identifies the machine that originally sent the syslog message.                                                         |
| **`ident`**           | `string`    | Appname identifies the device or application that originated the syslog message.                                                 |
| **`pid`**             | `bigint`    | ProcID is often the process ID, but can be any value used to enable log analyzers to detect discontinuities in syslog reporting. |
| **`msgid`**           | `string`    | MsgID identifies the type of message. For example, a firewall might use the MsgID 'TCPIN' for incoming TCP traffic.              |
| **`extradata`**       | `string`    | ExtraData contains syslog strucured data as string                                                                               |
| **`message`**         | `string`    | Message contains free-form text that provides information about the event.                                                       |
| **`time`**            | `timestamp` | Timestamp of the syslog message in UTC.                                                                                          |
| **`tag`**             | `string`    | Tag of the syslog message                                                                                                        |
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
