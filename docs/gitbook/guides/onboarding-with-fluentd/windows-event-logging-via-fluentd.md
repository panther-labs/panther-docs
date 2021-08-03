# Windows Event Logs via Fluentd

## Overview

This guide provides a method to deliver Windows Event Logs to S3 using Fluentd. There are two different pipeline flows: via an AWS Firehose delivery stream and directly to an AWS S3 bucket.

### Prerequisites

This guide assumes that an S3 bucket or Firehose has already been created. If you need to create either of these resources, please see the [Getting Started with Fluentd](resource-guide.md) guide. If you have already provisioned the resources, you can adapt the guide below to fit your needs.

## Setup Fluentd

### Step 1. Install Fluentd

Follow the Fluentd installation [instructions](https://docs.fluentd.org/installation/install-by-msi) for the Windows server from which you want to collect Windows Event Logs. See the installation instructions to make sure that Fluentd is running as a service.

### Step 2. Edit Fluentd Configuration

Edit the Fluentd configuration with the below configuration. This will configure Fluentd to use the `windows_eventlog2` plugin to read the events and output to S3. Update the `s3_bucket`, `s3_region`, `aws_key_id`, and `aws_sec_key` in the configuration below:

```text
C:\opt\td-agent\etc\td-agent\td-agent.conf
```

```text
<source>
  @type windows_eventlog2
  @id windows_eventlog2
  channels application,system,security
  tag system
  render_as_xml true
  <storage>
    persistent false
  </storage>
  parse_description false
  read_existing_events false
</source>

<match system.**>	
  @type s3

  s3_bucket <BUCKET-NAME>
  s3_region <BUCKET-REGION>
  path winevent/%Y/%m/%d/
  store_as gzip	

  ## There are two authentication methods below. 
  ## If this is running on EC2, you can use the assume role credentials instead of a token key

  ## Secret Token Authentication
  #aws_key_id <ACCESS-KEY-ID>
  #aws_sec_key <SECRET-KEY>

  ## Assume Role Authentication
  <assume_role_credentials>
    duration_seconds 3600
    role_arn <ROLE-ARN>
    role_session_name "#{Socket.gethostname}-panther-audit"
  </assume_role_credentials>

  <buffer tag,time>
    @type file
    path /var/log/fluent/s3
    timekey 300 # 5 min partition
    timekey_wait 2m
    timekey_use_utc true # use utc
    chunk_limit_size 256m
  </buffer>
  <format>
    @type json
  </format>
</match>

#<match system.**>
#  @type kinesis_firehose
#  region <STREAM-REGION>
#  delivery_stream_name <FIREHOSE-STREAM-NAME>
#
#  <assume_role_credentials>
#    duration_seconds 3600
#    role_arn <ROLE-ARN>
#    role_session_name "#{Socket.gethostname}-panther-audit"
#  </assume_role_credentials>
#  <format>
#    @type json
#  </format>
#</match>
```

You can read more about the `windows_eventlog2` plugin [here](https://github.com/fluent/fluent-plugin-windows-eventlog#in_windows_eventlog2).

### Step 3. Start Fluentd 

From the command prompt, start or restart the service with the below commands. You may need to stop/start the service if it had been previously running.

```text
sc stop fluentdwinsvc
sc start fluentdwinsvc
```

Check to make sure the service is running with the following command:

```text
sc query fluentdwinsvc
```

Expected output of the service running:

```text
C:\Users\Administrator>sc query fluentdwinsvc

SERVICE_NAME: fluentdwinsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

You can check for Fluentd runtime logs under `C:\opt\td-agent\td-agent.log`

To troubleshoot, you can also run td-agent from the command line and review the realtime output for issues via `C:\opt\td-agent\bin\td-agent -vv`. Stop the Fluentd service before running it manually.

### Step 4. Verify Logging

After a few minutes have passed, verify that events are being logged to the S3 bucket. Logs should be showing up under the `winevent/` prefix within the bucket.

## Panther UI

### Step 1. Create a Custom Schema

Go to **Log Analysis &gt; Custom Schema &gt; + New Schema** and enter the below values in the schema fields:

**Name:** Custom.WindowsEventLogs2  
**Description:** Windows Event Logs for Application, Security, System

```yaml
#Schema for windows_eventlog2 type from fluentd
version: 0
fields:
- name: TimeCreated
  required: true
  type: timestamp
  timeFormat: rfc3339
  isEventTime: true
- name: ActivityID
  required: true
  type: string
- name: Channel
  required: true
  type: string
- name: Computer
  required: true
  type: string
- name: Description
  required: true
  type: string
- name: EventData
  required: true
  type: array
  element:
    type: string
- name: EventID
  required: true
  type: bigint
- name: EventRecordID
  required: true
  type: bigint
- name: Keywords
  required: true
  type: string
- name: Level
  required: true
  type: bigint
- name: Opcode
  required: true
  type: string
- name: ProcessID
  required: true
  type: string
- name: ProviderGUID
  required: true
  type: string
- name: ProviderName
  required: true
  type: string
- name: Qualifiers
  required: true
  type: string
- name: RelatedActivityID
  required: true
  type: string
- name: Task
  required: true
  type: bigint
- name: ThreadID
  required: true
  type: string
- name: UserID
  required: true
  type: string
- name: Version
  required: true
  type: string
```

The above schema was generated using logs from the Fluentd source directive config provided earlier in this guide.

### Step 2. Onboard the S3 bucket <a id="Step-2.-Onboard-the-S3-bucket"></a>

Follow the [S3 source](../../data-onboarding/data-transports/s3.md) onboarding documentation and use the S3 Bucket used in the previous setup.

Select the log type `Custom.WindowsEventLogs2` and prefix `winevent/` in the onboarding steps. After completing the bucket onboarding, data should now be flowing into Panther!

