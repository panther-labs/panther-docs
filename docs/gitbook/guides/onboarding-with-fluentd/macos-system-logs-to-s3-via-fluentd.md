# MacOS System Logs to S3 via Fluentd

## Overview

This guide provides a method to deliver MacOS System Logs to S3 using Fluentd. There are two different pipeline flows: via an AWS Firehose delivery stream and directly to an AWS S3 bucket

### Prerequisites

This guide assumes that an S3 bucket or Firehose has already been created. If you need to create either of these resources, please see the [Getting Started with Fluentd](resource-guide.md) guide. If you have already provisioned the resources, you can adapt the guide below to fit your needs.

## Setup Fluentd

### Step 1. Install Fluentd (td-agent)

Follow the Fluentd installation [instructions](https://docs.fluentd.org/installation/install-by-dmg) for the machine from which you want to collect MacOS System Logs. This guide will specifically cover using td-agent as the service to collect logs.

### Step 2. Install the Fluent Plugin for MacOS Logs

Use the command below to install the Fluentd MacOS  plugin.

```
sudo /opt/td-agent/bin/fluent-gem install fluent-plugin-macos-log
```

Further documentation about this plugin can be found on [Github](https://github.com/loggly/fluent-plugin-macos-log).

### Step 3. Edit Fluentd Configuration

The configuration information that is included by default can be removed if not in use. Use the Fluentd configuration below and add your `aws_key_id`, `aws_sec_key`, `s3_bucket`, and `s3_region` information.

Fluentd and td-agent will attempt to run services on conflicting ports. If this is a new installation you will need to change the ports in the configuration file or remove the default configuration from the file.&#x20;

```
/etc/td-agent/td-agent.conf
```

```
<source>
  @type macoslog
  style ndjson
  tag macos
  pos_file last-starttime.log
  run_interval 10s
  <parse>
    @type json
    time_type string
    time_key timestamp
    time_format %Y-%m-%d %H:%M:%S.%L%z
  </parse>
</source>

<match **>
  @type s3
  aws_key_id <Key ID>
  aws_sec_key <Key>
  s3_bucket <Bucket>
  s3_region <Region>
  path macoslog/%Y/%m/%d/
  store_as gzip
  <buffer tag,time>
    @type file
    path /var/log/fluent/s3
    timekey 300 # 5 min partition to post to S3
    timekey_wait 2m
    timekey_use_utc true # use utc
    chunk_limit_size 256m
  </buffer>
  <format>
    @type json
  </format>
</match>
```

### Step 4. Point Fluentd to Configuration File and Validate

```
# Point fluentd to configuration file
fluentd -c /etc/td-agent/td-agent.conf

# Validate configuration
/opt/td-agent/usr/sbin/td-agent --dry-run
```

### Step 5. Verify Logging

After a few minutes have passed, verify that events are being logged to the S3 bucket. Logs should be showing up under the `macos/` prefix within the bucket.

## Panther UI

### Step 1. Create a Custom Schema

Go to **Log Analysis > Custom Schema > + New Schema** and enter the below values in the schema fields:

**Name:** Custom.MacOSSystemLogs\
**Description:** MacOS System Logs for Application, Security, System

```
version: 0
fields:
- name: pid
  type: bigint
- name: ppid
  type: bigint
- name: message
  type: string
- name: worker
  type: bigint
- name: creatorActivityID
  type: float
- name: messageType
  type: string
- name: activityIdentifier
  type: bigint
- name: backtrace
  type: object
  fields:
  - name: frames
    required: true
    type: array
    element:
      type: object
      fields:
      - name: imageOffset
        required: true
        type: bigint
      - name: imageUUID
        required: true
        type: string
- name: bootUUID
  type: string
- name: category
  type: string
- name: eventMessage
  type: string
- name: eventType
  type: string
- name: formatString
  type: string
- name: machTimestamp
  type: bigint
- name: parentActivityIdentifier
  type: bigint
- name: processID
  type: bigint
- name: processImagePath
  type: string
- name: processImageUUID
  type: string
- name: senderImagePath
  type: string
- name: senderImageUUID
  type: string
- name: senderProgramCounter
  type: bigint
- name: subsystem
  type: string
- name: threadID
  type: bigint
- name: timezoneName
  type: string
- name: traceID
  type: float
```

### Step 2. Onboard the S3 bucket <a href="step-2.-onboard-the-s3-bucket" id="step-2.-onboard-the-s3-bucket"></a>

Follow the [S3 source](../../data-onboarding/data-transports/s3.md) onboarding documentation and use the S3 Bucket used in the previous setup.

Select the log type `Custom.MacOSSystemLogs` and prefix `macos/` in the onboarding steps. After completing the bucket onboarding, data should now be flowing into Panther!

