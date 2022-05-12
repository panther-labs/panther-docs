---
description: Onboarding Nginx logs to your Panther Console
---

# Nginx

## Overview&#x20;

Panther supports ingesting Nginx logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3, SQS, and CloudWatch.

## How to onboard Nginx logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
     * [AWS CloudWatch](https://docs.panther.com/data-onboarding/data-transports/cwl-source)
2. Configure Nginx to push logs to the Data Transport source.
   * See Nginx's documentation for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in the table are in **bold.**
{% endhint %}

### Nginx.Access

Access Logs for your Nginx server. Panther supports Nginx 'combined' format.&#x20;

Reference: [Nginx Documentation on Log Formatting. ](http://nginx.org/en/docs/http/ngx\_http\_log\_module.html#log\_format)

| Column                | Type        | Description                                                                                                   |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------------------------- |
| `remoteAddr`          | `string`    | The IP address of the client (remote host) which made the request to the server.                              |
| `remoteUser`          | `string`    | The userid of the person making the request. Usually empty unless .htaccess has requested authentication.     |
| **`time`**            | `timestamp` | The time that the request was received (UTC).                                                                 |
| `request`             | `string`    | The request line from the client. It includes the HTTP method, the resource requested, and the HTTP protocol. |
| `status`              | `smallint`  | The HTTP status code returned to the client.                                                                  |
| `bodyBytesSent`       | `bigint`    | The size of the object returned to the client, measured in bytes.                                             |
| `httpReferer`         | `string`    | The HTTP referrer if any.                                                                                     |
| `httpUserAgent`       | `string`    | The agent the user used when making the request.                                                              |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                                          |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                                             |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                                    |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                                |
| `p_source_id`         | `string`    | Panther added field with the source id                                                                        |
| `p_source_label`      | `string`    | Panther added field with the source label                                                                     |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                                   |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                                   |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                    |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                     |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                 |
