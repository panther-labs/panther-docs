# Syslog to S3 via Fluentd

## Overview

This guide provides a method to deliver syslog messages to S3 using Fluentd. There are two different pipeline flows, using an AWS Firehose delivery stream and directly to an AWS S3 bucket.

### Pre-reqs

This general guide assumes an S3 bucket or Firehose has already been created.  If you need to create either of these, please see the [Getting Started with Fluentd](resource-guide.md) guide. If you already have the resources provisioned, you can adapt the guide below to fit your needs. 

## Setup Fluentd

### Step 1. Install Fluentd

Follow the [Fluentd install guide](https://docs.fluentd.org/installation) for the environment of the server you are wanting to collect syslog messages from.

### Step 2. Edit Fluentd Configuration

You have two options in configuring Fluentd: using the Firehouse plugin or S3 plugin. Below are the configuration files for both options. We recommend the Firehose plugin option as it is the more performant of the two. Both will deliver the logs to S3 nonetheless. Two different authentication types are shown in the configuration, assume role and access keys. Use the authentication type that best suits your environment.

#### **Via Firehose Plugin \(recommended\)**

The following Fluentd [plugin](https://github.com/awslabs/aws-fluent-plugin-kinesis#installation) needs to be installed.

```text
td-agent-gem install fluent-plugin-kinesis
```

Edit the Fluentd configuration`/etc/td-agent/td-agent.conf` with the below config. This will allow Fluentd to listen for syslog over udp port 5140 and output to Kinesis Firehose. Update the `region`, `delivery_stream_name` and `role_arn` in the configuration below.

```text
<source>
  @type syslog
  port 5140
  bind 0.0.0.0
  tag syslog
  <parse>
    message_format auto
  </parse>
</source>

<filter syslog.** >
  @type record_transformer
  <record>
    tag ${tag}
    time ${time}
  </record>
</filter>

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
    @type json
  </format>
</match>
```

#### **Via S3 Plugin**

Edit the Fluentd configuration `/etc/td-agent/td-agent.conf` with the below config. This will allow Fluentd to listen for syslog over udp port 5140 and output to a S3 bucket. Update the `s3_bucket`, `s3_region`, `aws_key_id`, and `aws_sec_key` in the configuration below.

```text
<source>
  @type syslog
  port 5140
  bind 0.0.0.0
  tag syslog
  <parse>
    message_format auto
  </parse>
</source>

<filter syslog.** >
  @type record_transformer
  <record>
    tag ${tag}
    time ${time}
  </record>
</filter>

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
    @type json
  </format>
</match>
```

### Step 3: Start Fluentd

After configuring Fluentd, start it by running the below command:

```text
$ sudo systemctl start td-agent.service 
```

Verify Fluentd is running:

```text
$ sudo systemctl status td-agent.service
```

See the [Fluentd install guide](https://docs.fluentd.org/installation) on starting Fluentd in your environment if `systemctl` is not available.

## Configure rsyslog

### Step 1. Configure rsyslog to forward to local Fluentd

Configure `rsyslog` to forward messages to the local Fluentd daemon by adding these two lines to the bottom of `/etc/rsyslog.d/50-default.conf` or `/etc/rsyslog.conf` in some environments:

```text
# Send log messages to Fluentd
*.* @127.0.0.1:5140
```

Restart rsyslog with the below command:

```text
$ sudo systemctl restart rsyslog.service
```

{% hint style="info" %}
This step can be duplicated for other servers to forward syslog to the Fluentd server that was configured previously in this guide. Just replace the local address `*.* @127.0.0.1:5140` with the IP address of the Fluentd server. You may need to update security groups or host based firewalls to allow sending udp/5140 traffic to the server.
{% endhint %}

## Verify Logging

After 5-10 minutes have passed, verify that syslog messages are being logged to the S3 bucket. Logs should be showing up under the `syslog/` prefix within the bucket.

You can now onboard the data in the Panther UI by onboarding the S3 bucket and using the `Fluentd.Syslog3164` log type.

