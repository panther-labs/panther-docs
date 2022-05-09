---
description: Onboarding Fastly logs to your Panther Console
---

# Fastly

## Overview

Panther supports ingesting Fastly logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3 and SQS.

## How to onboard Fastly logs to Panther

To pull these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
2. Configure your Data Transport source to pull in logs from Fastly.
   * See the Data Transport service provider's documentation for instructions on pulling in logs.

## Supported log types

{% hint style="info" %}
Required fields in the table are in **bold.**
{% endhint %}

### Fastly.Access

To ensure Panther can parse the logs, make sure to select "Blank" in the "Log line format" field when creating an S3 logging endpoint for your Fastly service.&#x20;

Reference: [Fastly Documentation on Common Log Format. ](https://docs.fastly.com/en/guides/useful-log-formats#common-log-format-clf)

| Column                     | Type        | Description                                                                                                                                                                                                         |
| -------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `remote_host_ip_address`   | `string`    | This is the IP address of the client (remote host) which made the request to the server. If HostnameLookups is set to On, then the server will try to determine the hostname and log it in place of the IP address. |
| `client_identity_rfc_1413` | `string`    | The RFC 1413 identity of the client determined by identd on the clients machine.                                                                                                                                    |
| `request_user`             | `string`    | The userid of the person requesting the document as determined by HTTP authentication.                                                                                                                              |
| `request_time`             | `timestamp` | The time that the request was received.                                                                                                                                                                             |
| `request_method`           | `string`    | The HTTP request method                                                                                                                                                                                             |
| `request_uri`              | `string`    | The HTTP request URI                                                                                                                                                                                                |
| `request_protocol`         | `string`    | The HTTP request protocol                                                                                                                                                                                           |
| `response_status`          | `smallint`  | The HTTP status of the response                                                                                                                                                                                     |
| `response_size`            | `bigint`    | The size of the HTTP response in bytes                                                                                                                                                                              |
| **`p_log_type`**           | `string`    | Panther added field with type of log                                                                                                                                                                                |
| **`p_row_id`**             | `string`    | Panther added field with unique id (within table)                                                                                                                                                                   |
| **`p_event_time`**         | `timestamp` | Panther added standardize event time (UTC)                                                                                                                                                                          |
| **`p_parse_time`**         | `timestamp` | Panther added standardize log parse time (UTC)                                                                                                                                                                      |
| `p_source_id`              | `string`    | Panther added field with the source id                                                                                                                                                                              |
| `p_source_label`           | `string`    | Panther added field with the source label                                                                                                                                                                           |
| `p_any_ip_addresses`       | `[string]`  | Panther added field with collection of ip addresses associated with the row                                                                                                                                         |
| `p_any_domain_names`       | `[string]`  | Panther added field with collection of domain names associated with the row                                                                                                                                         |
| `p_any_sha1_hashes`        | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                                                                                                                          |
| `p_any_md5_hashes`         | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                                                                                                                           |
| `p_any_sha256_hashes`      | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                                                                                                                       |
