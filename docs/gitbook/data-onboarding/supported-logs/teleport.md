---
description: Connecting Teleport logs to your Panther Console
---

# Teleport Logs

## Overview

Panther supports ingesting Teleport logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3 and SQS.

## How to onboard Teleport logs to Panther

To pull these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther’s documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
2. Configure your Data Transport source to pull in logs from Teleport.
   * See the Data Transport service provider's documentation for instructions on pulling in logs.

## Supported log types

### Gravitational.TeleportAudit

Required fields are in **bold**.

Teleport logs events like successful user logins along with the metadata like remote IP address, time and the session ID. Please see [Teleport's Cluster Administration Guide](https://goteleport.com/docs/setup/admin/#audit-log) for more information.

| Column               | Type                  | Description                                                                 |
| -------------------- | --------------------- | --------------------------------------------------------------------------- |
| **`event`**          | `string`              | Event type                                                                  |
| **`code`**           | `string`              | Event code                                                                  |
| **`time`**           | `timestamp`           | Event timestamp                                                             |
| **`uid`**            | `string`              | Event unique id                                                             |
| `user`               | `string`              | Teleport user name (event type is 'user.login')                             |
| `namespace`          | `string`              | Server namespace. This field is reserved for future use.                    |
| `server_id`          | `string`              | Unique server ID.                                                           |
| `sid`                | `string`              | Session ID. Can be used to replay the session.                              |
| `ei`                 | `int`                 | Event numeric id                                                            |
| `login`              | `string`              | OS login                                                                    |
| `addr_local`         | `string`              | Address of the SSH node                                                     |
| `addr_remote`        | `string`              | Address of the connecting client (user)                                     |
| `size`               | `string`              | Size of terminal                                                            |
| `success`            | `boolean`             | Authentication success (if event type is 'auth')                            |
| `error`              | `string`              | Authentication error (event type is 'auth')                                 |
| `command`            | `string`              | Command that was executed (event type is 'exec')                            |
| `exitCode`           | `int`                 | Exit code of the command (event type is 'exec')                             |
| `exitError`          | `string`              | Exit error of the command (event type is 'exec')                            |
| `pid`                | `bigint`              | Process id of command                                                       |
| `ppid`               | `bigint`              | Process id of the parent process                                            |
| `cgroup_id`          | `bigint`              | Control group id                                                            |
| `return_code`        | `int`                 | Return code of the command                                                  |
| `program`            | `string`              | Name of the command                                                         |
| `argv`               | `[string]`            | Arguments passed to command                                                 |
| `path`               | `string`              | Executable path or SCP action target file path (scp, session.command)       |
| `len`                | `bigint`              | SCP target file size (scp)                                                  |
| `action`             | `string`              | SCP action (scp)                                                            |
| `method`             | `string`              | Login method used (user.login)                                              |
| `attributes`         | `string`              | User login attributes (user.login)                                          |
| `roles`              | `[string]`            | Roles for the new user (user.create)                                        |
| `connector`          | `string`              | Connector that created the user (user.create)                               |
| `expires`            | `timestamp`           | Expiration date                                                             |
| `name`               | `string`              | Name of user or service (github.created, user.create, user.update)          |
| `tx`                 | `bigint`              | Number of bytes sent                                                        |
| `rx`                 | `bigint`              | Number of bytes received                                                    |
| `server_labels`      | `{   string:string }` | Server labels                                                               |
| `server_hostname`    | `string`              | Server hostname                                                             |
| `server_addr`        | `string`              | Server hostname                                                             |
| `session_start`      | `timestamp`           | Timestamp of session start                                                  |
| `session_stop`       | `timestamp`           | Timestamp of session end                                                    |
| `interactive`        | `boolean`             | Whether the session was interactive                                         |
| `enhanced_recording` | `boolean`             | Whether enhanced recording is enabled                                       |
| `participants`       | `[string]`            | Users that participated in the session                                      |
| `dst_addr`           | `string`              | Destination IP address                                                      |
| `src_addr`           | `string`              | Source IP address                                                           |
| `dst_port`           | `int`                 | Destination port                                                            |
| `version`            | `int`                 | Event version                                                               |
| **`p_event_time`**   | `timestamp`           | Panther added standardized event time (UTC)                                 |
| **`p_parse_time`**   | `timestamp`           | Panther added standardized log parse time (UTC)                             |
| **`p_log_type`**     | `string`              | Panther added field with type of log                                        |
| **`p_row_id`**       | `string`              | Panther added field with unique id (within table)                           |
| `p_source_id`        | `string`              | Panther added field with the source id                                      |
| `p_source_label`     | `string`              | Panther added field with the source label                                   |
| `p_any_ip_addresses` | `[string]`            | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]`            | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids`    | `[string]`            | Panther added field with collection of context trace identifiers            |
