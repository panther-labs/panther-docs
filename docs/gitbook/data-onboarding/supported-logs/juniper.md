---
description: Connecting Juniper logs to your Panther Console
---

# Juniper Logs

## Overview

Panther supports ingesting Juniper logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3 and SQS.

## How to onboard Juniper logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
2. Configure Juniper to push logs to the Data Transport source.
   * See Juniper's documentation for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in all the tables are in **bold.**
{% endhint %}

### Juniper.Access

Juniper.Access logs for all traffic coming to and from the box.&#x20;

Reference: **** [Juniper Documentation on Access Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-access-log.html)

| Column                | Type        | Description                                                                                                            |
| --------------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------- |
| `timestamp`           | `timestamp` | Log entry timestamp                                                                                                    |
| `hostname`            | `string`    | The hostname of the appliance                                                                                          |
| `log_level`           | `string`    | The importance level of a log entry. Can be TRACE, DEBUG, INFO, WARN, or ERROR.                                        |
| `thread`              | `string`    | The specific thread that is handling the request or response.                                                          |
| `unique_request_key`  | `string`    | This is a key used to uniquely identify requests.                                                                      |
| `type`                | `string`    | Whether the HTTP packet is a client request, or a server response (REQUEST,RESPONSE).                                  |
| `stage`               | `string`    | Whether the HTTP packet is being logged before or after Security Engine processes it (and potentially manipulates it). |
| `proxy_client_ip`     | `string`    | The incoming client IP. Since WebApp Secure works around a Nginx proxy, the client IP will most-likely be '127.0.0.1'. |
| `url`                 | `string`    | The full request or response URL.                                                                                      |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                                                   |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                                                      |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                                             |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                                         |
| `p_source_id`         | `string`    | Panther added field with the source id                                                                                 |
| `p_source_label`      | `string`    | Panther added field with the source label                                                                              |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                                            |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                                            |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                             |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                              |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                          |

### Juniper.Audit

The audit log contains log entries that indicate non-idempotent (state changing) actions performed on WebApp Secure.&#x20;

Reference: [Juniper Documentation on Audit Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-incident-log-format.html)

| Column                | Type        | Description                                                                                   |
| --------------------- | ----------- | --------------------------------------------------------------------------------------------- |
| `timestamp`           | `timestamp` | Log entry timestamp                                                                           |
| `hostname`            | `string`    | The hostname of the appliance                                                                 |
| `log_level`           | `string`    | The importance level of a log entry. Can be TRACE, DEBUG, INFO, WARN, or ERROR.               |
| `message`             | `string`    | The message. Can indicate any of the previously mentioned actions.                            |
| `api_key`             | `string`    | The key used to perform the action described in the message.                                  |
| `login_ip`            | `string`    | The IP address the user performed logged in from                                              |
| `username`            | `string`    | The user that performed the login                                                             |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                          |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                             |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                    |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                |
| `p_source_id`         | `string`    | Panther added field with the source id                                                        |
| `p_source_label`      | `string`    | Panther added field with the source label                                                     |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                   |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                   |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                    |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                     |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row |

### Juniper.Firewall

Juniper.Firewall stores information about dropped packets from the iptables firewall.&#x20;

Reference: [Juniper Documentation on Firewall Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-profile-log-format.html)

| Column                | Type        | Description                                                                                   |
| --------------------- | ----------- | --------------------------------------------------------------------------------------------- |
| **`timestamp`**       | `timestamp` | Log timestamp                                                                                 |
| `hostname`            | `string`    | Hostname                                                                                      |
| `event`               | `string`    | Event name                                                                                    |
| `DST`                 | `string`    | Destination IP address                                                                        |
| `DPT`                 | `int`       | Destination port                                                                              |
| `SRC`                 | `string`    | Source IP address                                                                             |
| `SPT`                 | `int`       | Source port                                                                                   |
| `TTL`                 | `bigint`    | IP TTL in milliseconds                                                                        |
| `ID`                  | `bigint`    | Packet id                                                                                     |
| `MAC`                 | `string`    | MAC address                                                                                   |
| `LEN`                 | `int`       | Packet length                                                                                 |
| `TOS`                 | `string`    | Packet Type of Service field                                                                  |
| `PREC`                | `string`    | Packet precedence bits                                                                        |
| `RST`                 | `boolean`   | Packet is RST                                                                                 |
| `SYN`                 | `boolean`   | Packet is SYN                                                                                 |
| `DF`                  | `boolean`   | Packet has do not fragment flag                                                               |
| `IN`                  | `string`    | Input interface                                                                               |
| `OUT`                 | `string`    | Output interface                                                                              |
| `PROTO`               | `string`    | Protocol                                                                                      |
| `WINDOW`              | `int`       | Transmit window                                                                               |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                          |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                             |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                    |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                |
| `p_source_id`         | `string`    | Panther added field with the source id                                                        |
| `p_source_label`      | `string`    | Panther added field with the source label                                                     |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                   |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                   |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                    |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                     |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row |

### Juniper.MWS

Juniper.MWS is the main log file for most WebApp Secure logging needs. All messages that don't have a specific log location are sent, by default, to mws.log.&#x20;

Reference: [Juniper Documentation on MWS Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-mws-log.html)&#x20;

| Column                | Type        | Description                                                                                                                                                  |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `timestamp`           | `timestamp` | The date of the log entry, in UTC.                                                                                                                           |
| `hostname`            | `string`    | The appliance hostname.                                                                                                                                      |
| `log_level`           | `string`    | The importance level of a log entry. Can be TRACE, DEBUG, INFO, WARN, or ERROR.                                                                              |
| `service_name`        | `string`    | The WebApp Secure service that generated the log entry.                                                                                                      |
| `service_component`   | `string`    | The specific component that is issuing the log message.                                                                                                      |
| `log_message`         | `string`    | The message. This can be anything, but usually contains information to help you narrow down problems or confirm certain events have occurred as they should. |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                                                                                         |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                                                                                            |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                                                                                   |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                                                                               |
| `p_source_id`         | `string`    | Panther added field with the source id                                                                                                                       |
| `p_source_label`      | `string`    | Panther added field with the source label                                                                                                                    |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                                                                                  |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                                                                                  |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                                                                   |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                                                                    |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                                                                |

### Juniper.Postgres

Juniper.Postgres contains logs of manipulations on the schema of the database that WebApp Secure uses, as well as any errors that occurred during database operations.&#x20;

Reference: [Juniper Documentation on Postgres Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-postgres-log.html)

| Column                | Type        | Description                                                                                    |
| --------------------- | ----------- | ---------------------------------------------------------------------------------------------- |
| `timestamp`           | `timestamp` | Log entry timestamp                                                                            |
| `hostname`            | `string`    | The hostname of the appliance                                                                  |
| `pid`                 | `int`       | The process ID of the Postgres instance.                                                       |
| `group_id_major`      | `int`       | Group id major number                                                                          |
| `group_id_minor`      | `int`       | Group id minor number                                                                          |
| `sql_error_code`      | `string`    | The SQL error code.                                                                            |
| `session_id`          | `string`    | A somewhat unique session identifier that can be used to search for specific lines in the log. |
| `message_type`        | `string`    | The type of the message. Can be LOG, WARNING, ERROR, or STATEMENT.                             |
| `message`             | `string`    | The message.                                                                                   |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                           |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                              |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                     |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                 |
| `p_source_id`         | `string`    | Panther added field with the source id                                                         |
| `p_source_label`      | `string`    | Panther added field with the source label                                                      |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                    |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                    |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                     |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                      |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row  |

### Juniper.Security

Juniper.Security Webapp Secure is configured to log security incidents to mws-security.log. All security alerts should be sent to security.log (previously named security-alert.log). There are different types of security incidents that will be a part of this log: new profiles, security incidents, new counter responses.

Reference: [Juniper Documentation on Security Log Format.](https://www.juniper.net/documentation/en\_US/webapp5.6/topics/reference/w-a-s-log-format.html)

| Column                | Type        | Description                                                                                                                                                                           |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `timestamp`           | `timestamp` | Log entry timestamp                                                                                                                                                                   |
| `hostname`            | `string`    | The hostname of the appliance                                                                                                                                                         |
| `log_level`           | `string`    | The importance level of a log entry. Can be TRACE, DEBUG, INFO, WARN, or ERROR.                                                                                                       |
| `service`             | `string`    | The WebApp Secure service that triggered the security log entry.                                                                                                                      |
| `category`            | `string`    | Log entry category                                                                                                                                                                    |
| `profile_id`          | `string`    | The numerical ID assigned to the Profile that caused the security alert, or the profile ID that received a Response.                                                                  |
| `profile_name`        | `string`    | The friendly name assigned to the Profile that caused the security alert, or the Profile that received a Response.                                                                    |
| `pubkey`              | `string`    | The Public ID that can be used in conjunction with the Support\_Processor to unblock Profiles.                                                                                        |
| `incident`            | `string`    | The name of the incident that triggered this security alert.                                                                                                                          |
| `severity`            | `smallint`  | The numerical severity of the incident that triggered this security alert. This can be a number from 0 to 4, inclusive.                                                               |
| `source_ip`           | `string`    | The IP the request that generated this alert originated from.                                                                                                                         |
| `user_agent`          | `string`    | The client's user agent string that generated this alert.                                                                                                                             |
| `url`                 | `string`    | The request URL that generated this alert.                                                                                                                                            |
| `count`               | `int`       | The number of times the profile triggered this incident. This is used for certain incidents to decide whether or not to elevate the profile or increase the responses on the profile. |
| `fake_response`       | `boolean`   | Whether or not (true or false) the response sent back to the client was a fake one created by WebApp Secure.                                                                          |
| `response_code`       | `string`    | The numerical code for the response issued.                                                                                                                                           |
| `response_name`       | `string`    | The friendly name for the response issued on the profile indicated in the alert.                                                                                                      |
| `created_date`        | `timestamp` | The date and time the response was created.                                                                                                                                           |
| `delay_date`          | `timestamp` | The date and time the response is set to be delayed until.                                                                                                                            |
| `expiration_date`     | `timestamp` | The date and time the response is set to expire.                                                                                                                                      |
| `response_config`     | `string`    | The configuration used in this response. Displayed as an XML-like node.                                                                                                               |
| `silent_running`      | `boolean`   | Whether or not this Counter Response was set to be silent with the Silent Running service at the time of activation.                                                                  |
| **`p_log_type`**      | `string`    | Panther added field with type of log                                                                                                                                                  |
| **`p_row_id`**        | `string`    | Panther added field with unique id (within table)                                                                                                                                     |
| **`p_event_time`**    | `timestamp` | Panther added standardize event time (UTC)                                                                                                                                            |
| **`p_parse_time`**    | `timestamp` | Panther added standardize log parse time (UTC)                                                                                                                                        |
| `p_source_id`         | `string`    | Panther added field with the source id                                                                                                                                                |
| `p_source_label`      | `string`    | Panther added field with the source label                                                                                                                                             |
| `p_any_ip_addresses`  | `[string]`  | Panther added field with collection of ip addresses associated with the row                                                                                                           |
| `p_any_domain_names`  | `[string]`  | Panther added field with collection of domain names associated with the row                                                                                                           |
| `p_any_sha1_hashes`   | `[string]`  | Panther added field with collection of SHA1 hashes associated with the row                                                                                                            |
| `p_any_md5_hashes`    | `[string]`  | Panther added field with collection of MD5 hashes associated with the row                                                                                                             |
| `p_any_sha256_hashes` | `[string]`  | Panther added field with collection of SHA256 hashes of any algorithm associated with the row                                                                                         |
