---
description: Deliver raw logs from files to S3 using Fluentd
---

# General log forwarding via Fluentd

## Overview

This is a guide for delivering raw logs from files to Panther via S3 and Fluentd. The Fluentd configurations below do not pre-process the data, instead they forward the contents of the file(s) as-is. This guide will walk you through how to do the following:

* Install Fluentd on your device.
* Edit your Fluentd configuration via an AWS Firehose or an S3 plugin.
* Launch and verify your Fluentd instance is running correctly.

### Prerequisites

An S3 bucket or AWS Firehose is required. If you need to create either of these resources, please see the [Getting Started with Fluentd](resource-guide.md) guide. You can also adapt the guide below to fit your needs if you already have the resources provisioned.

## Setup Fluentd to deliver raw logs to Panther

### Step 1: Install Fluentd

Follow the [Fluentd install guide](https://docs.fluentd.org/installation) for the server environment you want to collect syslog messages from. Once installed, you may proceed with the terminal configurations below to properly configure Fluentd.

### Step 2: Edit Fluentd Configuration

{% hint style="info" %}
You have two options when configuring Fluentd – Firehouse plugin or S3 plugin. We recommend the Firehose plugin option, as it is the more performant option with Panther. However, both will deliver your logs to S3. \


Two different authentication types are shown in the configuration – assume roles or access keys. Use the authentication type that best suits your environment.
{% endhint %}

Be aware of the below plugin configurations with Fluentd:

* The `<source>` section uses the tail plugin to read from a log file.
* The `<parse>` section instructs the Fluentd to not perform any parsing with `@type none`.
* Within the `match format` section, the `single_value` type is used.&#x20;
* The combination of `none` parsing and `single_value` format tells Fluentd to output the data as-is.&#x20;

#### **Configuration 1: Via Firehose Plugin (recommended)**

The Firehose plugin for Fluentd must be installed to leverage the `@type kinesis plugin`**.**

1.  **Install** the following [Fluentd plugin](https://github.com/awslabs/aws-fluent-plugin-kinesis#installation) with the command below.\


    ```yaml
    td-agent-gem install fluent-plugin-kinesis
    ```


2.  **Edit** the Fluentd configuration located at `/etc/td-agent/td-agent.conf` with the configuration below. Make sure to update **** the `region`, `delivery_stream_name`, and `role_arn`:\


    ```
    <source>  
      @type tail
      tag syslog
      path /path/to/file/*.log
      pos_file /var/log/td-agent/pos_file.pos
      <parse>
        @type none
      </parse>
    </source>

    <match syslog.**>
      @type kinesis_firehose
      region <FIREHOSE-REGION>
      delivery_stream_name <FIREHOSE-NAME>

      <assume_role_credentials>
        duration_seconds 3600
        role_arn <FIREHOSE-ROLE-ARN>
        role_session_name "#{Socket.gethostname}-panther-audit"
      </assume_role_credentials>
      <format>
        @type single_value
      </format>
    </match>
    ```

#### **Configuration 2: Via S3 Plugin**

1.  **Edit** the Fluentd configuration located at `/etc/td-agent/td-agent.conf,` with the configuration below. Make sure to update **** the `s3_bucket`, `s3_region`, `aws_key_id`, and `aws_sec_key`:\


    ```
    <source>  
      @type tail
      tag syslog
      path /path/to/file/*.log
      pos_file /var/log/td-agent/pos_file.pos
      <parse>
        @type none
      </parse>
    </source>

    <match syslog.**>
      @type s3
      aws_key_id <ACCESS-KEY-ID>
      aws_sec_key <SECRET-KEY>
      s3_bucket <BUCKET-NAME>
      s3_region <BUCKET-REGION>
      path syslog/%Y/%m/%d/
      store_as gzip
      <buffer tag,time>
        @type file
        path /var/log/td-agent/buffer/s3
        timekey 300 # 5 min partition
        timekey_wait 2m
        timekey_use_utc true # use utc
        chunk_limit_size 256m
      </buffer>
      <format>
        @type single_value
      </format>
    </match>
    ```

### Step 3: Start Fluentd

1.  After configuring Fluentd, run the below command in your terminal:\


    ```
    $ sudo systemctl start td-agent.service 
    ```


2.  To verify that Fluentd is running correctly, run the below command in your terminal:\


    ```
    $ sudo systemctl status td-agent.service
    ```

{% hint style="info" %}
If `systemctl` is not available in your Fluentd environment, see the [Fluentd install guide](https://docs.fluentd.org/installation).
{% endhint %}
