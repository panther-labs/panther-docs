---
description: Connecting Lacework logs to your Panther Console
---

# Lacework Logs

## Overview

Panther supports ingesting Lacework logs via common [Data Transport](https://docs.panther.com/data-onboarding/data-transports) options: Amazon Web Services (AWS) S3 and SQS.

## How to onboard Lacework logs to Panther

To connect these logs into Panther:

1. Set up your Data Transport in the Panther Console.
   * Please follow Panther's documentation for configuring the Data Transport option you will use:
     * [AWS S3 bucket](https://docs.panther.com/data-onboarding/data-transports/s3)
     * [AWS SQS](https://docs.panther.com/data-onboarding/data-transports/sqs)
2. Configure Lacework to push logs to the Data Transport source.
   * See [Lacework's documentation](https://docs.lacework.com/console/category/data-shares--export) for instructions on pushing logs to your selected Data Transport source.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"** just below the "name" field.
{% endhint %}

### Lacework.AgentManagement

Lacework.AgentManagement gathers Lacework agent management information.

Reference: [Lacework Documentation on AgentManagement](https://docs.lacework.com/console/agentmanagementv-view).

```yaml
fields:
  - name: AGENT_VERSION
    required: true
    type: string
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: HOSTNAME
    required: true
    type: string
  - name: IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: LAST_UPDATE
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: MID
    required: true
    type: bigint
  - name: MODE
    required: true
    type: string
  - name: OS
    required: true
    type: string
  - name: STATUS
    required: true
    type: string
  - name: TAGS
    type: json
```

### Lacework.AllFiles

Lacework.AllFiles tracks every time Lacework detects a file.

Reference: [Lacework Documentation on AllFiles](https://docs.lacework.com/console/allfilesv-view).

```yaml
fields:
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: FILEDATA_HASH
    required: true
    type: string
    indicators:
      - sha256
  - name: FILE_PATH
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: MTIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: SIZE
    required: true
    type: bigint
```

### Lacework.ChangeFiles

Lacework.ChangeFiles tracks every time a file is changed in your environment.

Reference: [Lacework Documentation on ChangeFiles](https://docs.lacework.com/console/changefilesv-view).

```yaml
fields:
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: FILEDATA_HASH
    required: true
    type: string
    indicators:
      - sha256
  - name: FILE_PATH
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: MTIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: SIZE
    required: true
    type: bigint
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.Cmdline

Lacework.Cmdline monitors any command line invocations in your environment.

Reference: [Lacework Documentation on Cmdline](https://docs.lacework.com/console/cmdlinev-view).

```yaml
fields:
  - name: CMDLINE
    required: true
    type: string
  - name: CMDLINE_HASH
    required: true
    type: string
    indicators:
      - md5
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
```

### Lacework.Connections

Lacework.Connections monitors for connections in your environment.

Reference: [Lacework Documentation on Connections](https://docs.lacework.com/console/connectionsv-view).

```yaml
fields:
  - name: DST_ENTITY_ID
    required: true
    type: object
    fields:
      - name: pid_hash
        type: string
      - name: port
        type: bigint
      - name: protocol
        type: string
      - name: mid
        type: bigint
  - name: DST_ENTITY_TYPE
    required: true
    type: string
  - name: DST_IN_BYTES
    required: true
    type: bigint
  - name: DST_OUT_BYTES
    required: true
    type: bigint
  - name: ENDPOINT_DETAILS
    required: true
    type: array
    element:
      type: object
      fields:
        - name: dst_ip_addr
          required: true
          type: string
          indicators:
            - ip
        - name: dst_port
          required: true
          type: bigint
        - name: protocol
          required: true
          type: string
        - name: src_ip_addr
          required: true
          type: string
          indicators:
            - ip
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: NUM_CONNS
    required: true
    type: bigint
  - name: SRC_ENTITY_ID
    required: true
    type: object
    fields:
      - name: mid
        required: true
        type: bigint
      - name: pid_hash
        required: true
        type: string
  - name: SRC_ENTITY_TYPE
    required: true
    type: string
  - name: SRC_IN_BYTES
    required: true
    type: bigint
  - name: SRC_OUT_BYTES
    required: true
    type: bigint
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'              
```

### Lacework.ContainerSummary

Lacework.ContainerSummary monitors for containers in your environment.

Reference: [Lacework Documentation on ContainerSummary](https://docs.lacework.com/console/containersummaryv-view).

```yaml
fields:
  - name: POD_NAME
    type: string
  - name: CONTAINER_NAME
    required: true
    type: string
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: IMAGE_ID
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: PROPS_CONTAINER
    required: true
    type: object
    fields:
      - name: VOLUME_MAP
        type: json
      - name: POD_IP_ADDR
        type: string
        indicators:
          - ip
      - name: LISTEN_PORT_MAP
        type: json
      - name: POD_TYPE
        type: string
      - name: PROPS_LABEL
        type: json
      - name: CONTAINER_START_TIME
        required: true
        type: timestamp
        timeFormat: unix_ms
      - name: CONTAINER_TYPE
        required: true
        type: string
      - name: IMAGE_AUTHOR
        required: true
        type: string
      - name: IMAGE_CREATED_TIME
        required: true
        type: timestamp
        timeFormat: unix_ms
      - name: IMAGE_ID
        required: true
        type: string
      - name: IMAGE_PARENT_ID
        required: true
        type: string
      - name: IMAGE_REPO
        required: true
        type: string
      - name: IMAGE_SIZE
        required: true
        type: bigint
      - name: IMAGE_TAG
        required: true
        type: string
      - name: IMAGE_VERSION
        required: true
        type: string
      - name: IMAGE_VIRTUAL_SIZE
        required: true
        type: bigint
      - name: IPV4
        required: true
        type: string
        indicators:
          - ip
      - name: NAME
        required: true
        type: string
      - name: NETWORK_MODE
        required: true
        type: string
      - name: PID_MODE
        required: true
        type: string
      - name: PRIVILEGED
        required: true
        type: bigint
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: TAGS
    required: true
    type: json
```

### Lacework.ContainerVulnDetails

Lacework.ContainerVulnDetails monitors for container vulnerabilities in your environment.

Reference: [Lacework Documentation on ContainerVulnDetails](https://docs.lacework.com/console/containervulndetailsv-view).

```yaml
fields:
  - name: SEVERITY
    type: string
  - name: VULN_ID
    type: string
  - name: EVAL_CTX
    required: true
    type: object
    fields:
      - name: cve_batch_info
        required: true
        type: array
        element:
          type: object
          fields:
            - name: cve_batch_id
              required: true
              type: string
            - name: cve_created_time
              required: true
              type: timestamp
              timeFormat: '%Y-%m-%d %H:%M:%S.%f000'
      - name: image_info
        required: true
        type: object
        fields:
          - name: created_time
            required: true
            type: timestamp
            timeFormat: unix_ms
          - name: digest
            required: true
            type: string
          - name: id
            required: true
            type: string
          - name: registry
            required: true
            type: string
          - name: repo
            required: true
            type: string
          - name: scan_created_time
            required: true
            type: timestamp
            timeFormat: unix
          - name: size
            required: true
            type: bigint
          - name: status
            required: true
            type: string
          - name: tags
            required: true
            type: array
            element:
              type: string
          - name: type
            required: true
            type: string
      - name: integration_props
        required: true
        type: object
        fields:
          - name: INTG_GUID
            type: string
          - name: NAME
            type: string
          - name: REGISTRY_TYPE
            type: string
      - name: is_reeval
        required: true
        type: boolean
      - name: request_source
        required: true
        type: string
      - name: scan_batch_id
        required: true
        type: string
      - name: scan_request_props
        required: true
        type: object
        fields:
          - name: reqId
            type: string
          - name: data_format_version
            required: true
            type: string
          - name: props
            required: true
            type: object
            fields:
              - name: data_format_version
                required: true
                type: string
              - name: scanner_version
                required: true
                type: string
          - name: scanCompletionUtcTime
            required: true
            type: timestamp
            timeFormat: unix
          - name: scan_start_time
            required: true
            type: timestamp
            timeFormat: unix
          - name: scanner_version
            required: true
            type: string
      - name: vuln_batch_id
        required: true
        type: string
      - name: vuln_created_time
        required: true
        type: timestamp
        timeFormat: '%Y-%m-%d %H:%M:%S.%f000'
  - name: FEATURE_KEY
    required: true
    type: object
    fields:
      - name: name
        required: true
        type: string
      - name: namespace
        required: true
        type: string
      - name: version
        required: true
        type: string
  - name: FEATURE_PROPS
    required: true
    type: object
    fields:
      - name: introduced_in
        required: true
        type: string
      - name: layer
        required: true
        type: string
      - name: src
        required: true
        type: string
      - name: version_format
        required: true
        type: string
  - name: FIX_INFO
    required: true
    type: object
    fields:
      - name: compare_result
        required: true
        type: string
      - name: fix_available
        required: true
        type: string
      - name: fixed_version
        required: true
        type: string
  - name: IMAGE_ID
    required: true
    type: string
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: STATUS
    required: true
    type: string
```

### Lacework.DNSQuery

Lacework.DNSQuery monitors for any DNS queries in your environment.

Reference: [Lacework Documentation on DNSQuery](https://docs.lacework.com/console/dnsqueryv-view).

```yaml
fields:
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: DNS_SERVER_IP
    required: true
    type: string
    indicators:
      - ip
  - name: FQDN
    required: true
    type: string
    indicators:
      - domain
  - name: HOST_IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: MID
    required: true
    type: bigint
  - name: TTL
    required: true
    type: bigint
```

### Lacework.Events

Lacework.Events represents the content of an exported Lacework Alert S3 Object.&#x20;

Reference: [Lacework Documentation on Events](https://www.lacework.com/platform/).

```yaml
- name: EVENT_CATEGORY
      required: true
      description: The category the event falls into
      type: string
    - name: EVENT_DETAILS
      required: true
      description: The event details
      type: object
      fields:
        - name: data
          description: The array of event data
          type: array
          element:
            type: object
            fields:
                - name: START_TIME
                  description: The event start time.
                  type: timestamp
                  timeFormat: rfc3339
                - name: END_TIME
                  description: The event end time.
                  type: timestamp
                  timeFormat: rfc3339
                - name: EVENT_TYPE
                  description: The event type description eg - launched new binary.
                  type: string
                - name: EVENT_ID
                  description: The event alert ID.
                  type: string
                - name: EVENT_ACTOR
                  description: The origin of the event eg - AWS, User.
                  type: string
                - name: EVENT_MODEL
                  description: The model that triggered an alert.
                  type: string
                - name: ENTITY_MAP
                  description: The map of related fields to the detection alert.
                  type: object
                  fields:
                    - name: User
                      description: Any user based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: MACHINE_HOSTNAME
                              description: Hostname field
                              type: string
                            - name: USERNAME
                              description: Username field
                              type: string
                              indicators:
                                - username
                    - name: Application
                      description: Any application based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: APPLICATION
                              description: Application field
                              type: string
                            - name: HAS_EXTERNAL_CONNS
                              description: HasExternalConns field
                              type: bigint
                            - name: IS_CLIENT
                              description: IsClient field
                              type: bigint
                            - name: IS_SERVER
                              description: IsServer field
                              type: bigint
                            - name: EARLIEST_KNOWN_TIME
                              description: EarliestKnownTime field
                              type: timestamp
                              timeFormat: rfc3339
                    - name: Machine
                      description: Any machine based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: HOSTNAME
                              description: Hostname field
                              type: string
                            - name: EXTERNAL_IP
                              description: ExternalIP field
                              type: string
                              indicators:
                                - ip
                            - name: INSTANCE_ID
                              description: InstanceID field
                              type: string
                            - name: INSTANCE_NAME
                              description: InstanceName field
                              type: string
                            - name: CPU_PERCENTAGE
                              description: CPUPercentage field
                              type: float
                            - name: INTERNAL_IP_ADDR
                              description: InternalIPAddress field
                              type: string
                              indicators:
                                - ip
                            - name: IS_EXTERNAL
                              description: IsExternal field
                              type: bigint
                    - name: Container
                      description: Any container based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: IMAGE_REPO
                              description: ImageRepo field
                              type: string
                            - name: IMAGE_TAG
                              description: ImageTag field
                              type: string
                            - name: HAS_EXTERNAL_CONNS
                              description: HasExternalConns field
                              type: bigint
                            - name: IS_CLIENT
                              description: IsClient field
                              type: bigint
                            - name: IS_SERVER
                              description: IsServer field
                              type: bigint
                            - name: FIRST_SEEN_TIME
                              description: FirstSeenTime field
                              type: timestamp
                              timeFormat: rfc3339
                            - name: POD_NAMESPACE
                              description: PodNamespace field
                              type: string
                            - name: POD_IP_ADDR
                              description: PodIPAddress field
                              type: string
                              indicators:
                                - ip
                    - name: DnsName
                      description: Any dns based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: HOSTNAME
                              description: Hostname field
                              type: string
                            - name: PORT_LIST
                              description: PortList field
                              type: array
                              element:
                                type: int
                            - name: TOTAL_IN_BYTES
                              description: TotalINBytes field
                              type: float
                            - name: TOTAL_OUT_BYTES
                              description: TotalOUTBytes field
                              type: float
                    - name: IpAddress
                      description: Any ip based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: IP_ADDRESS
                              description: SourceIPAddress field
                              type: string
                              indicators:
                                - ip
                            - name: TOTAL_IN_BYTES
                              description: TotalINBytes field
                              type: float
                            - name: TOTAL_OUT_BYTES
                              description: TotalOUTBytes field
                              type: float
                            - name: THREAT_TAGS
                              description: ThreatTags field
                              type: array
                              element:
                                type: string
                            - name: THREAT_SOURCE
                              description: ThreatSource field
                              type: json
                            - name: COUNTRY
                              description: Country field
                              type: string
                            - name: REGION
                              description: Region field
                              type: string
                            - name: PORT_LIST
                              description: PortList field
                              type: array
                              element:
                                type: int
                            - name: FIRST_SEEN_TIME
                              description: FirstSeenTime field
                              type: string
                    - name: Process
                      description: Any process based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: HOSTNAME
                              description: Hostname field
                              type: string
                            - name: PROCESS_ID
                              description: ProcessID field
                              type: bigint
                            - name: PROCESS_START_TIME
                              description: ProcessStartTime field
                              type: timestamp
                              timeFormat: rfc3339
                            - name: CMDLINE
                              description: CommandLine field
                              type: string
                            - name: CPU_PERCENTAGE
                              description: CPUPercentage field
                              type: float
                    - name: FileDataHash
                      description: Any filehash based info involved in an alert.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: FILEDATA_HASH
                              description: FiledataHash field
                              type: string
                            - name: MACHINE_COUNT
                              description: MachineCount field
                              type: bigint
                            - name: EXE_PATH_LIST
                              description: EXEPathList field
                              type: array
                              element:
                                type: string
                            - name: FIRST_SEEN_TIME
                              description: FirstSeenTime field
                              type: timestamp
                              timeFormat: rfc3339
                            - name: IS_KNOWN_BAD
                              description: ISKnownBad field
                              type: bigint
                    - name: FileExePath
                      description: Any executable filepath information.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: EXE_PATH
                              description: EXEPath field
                              type: string
                            - name: FIRST_SEEN_TIME
                              description: FirstSeenTime field
                              type: timestamp
                              timeFormat: rfc3339
                            - name: LAST_FILEDATA_HASH
                              description: LastFileDataHash field
                              type: string
                            - name: LAST_PACKAGE_NAME
                              description: LastPackageName field
                              type: string
                            - name: LAST_VERSION
                              description: LastVersion field
                              type: string
                            - name: LAST_FILE_OWNER
                              description: LastFileOwner field
                              type: string
                    - name: SourceIpAddress
                      description: Source IP based information.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: IP_ADDRESS
                              description: SourceIPAddress field
                              type: string
                              indicators:
                                - ip
                            - name: REGION
                              description: Region field
                              type: string
                            - name: COUNTRY
                              description: Country field
                              type: string
                    - name: API
                      description: The service and endpoint.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: SERVICE
                              description: EventSource field
                              type: string
                            - name: API
                              description: EventName field
                              type: string
                    - name: Region
                      description: Regional based information.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: REGION
                              description: Region field
                              type: string
                            - name: ACCOUNT_LIST
                              description: RecipientAccountID field
                              type: array
                              element:
                                type: string
                    - name: CT_User
                      description: Cloudtrail user information.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: USERNAME
                              description: Username field
                              type: string
                              indicators:
                                - username
                            - name: ACCOUNT_ID
                              description: AccountID field
                              type: string
                            - name: MFA
                              description: MFA field
                              type: bigint
                            - name: API_LIST
                              description: APIList field
                              type: array
                              element:
                                type: string
                            - name: REGION_LIST
                              description: RegionList field
                              type: array
                              element:
                                type: string
                            - name: PRINCIPAL_ID
                              description: AccessKeyID field
                              type: string
                    - name: Resource
                      description: Resource values.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: NAME
                              description: Name field
                              type: string
                            - name: VALUE
                              description: Value field
                              type: string
                    - name: RecId
                      description: Receiver account info.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: REC_ID
                              description: RECID field
                              type: string
                            - name: ACCOUNT_ID
                              description: RecipientAccountID field
                              type: string
                            - name: ACCOUNT_ALIAS
                              description: AccountAlias field
                              type: string
                            - name: TITLE
                              description: Title field
                              type: string
                            - name: STATUS
                              description: Status field
                              type: string
                            - name: EVAL_TYPE
                              description: EVALType field
                              type: string
                            - name: EVAL_GUID
                              description: EVALGUID field
                              type: string
                    - name: CustomRule
                      description: Custom Rule info.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: LAST_UPDATED_TIME
                              description: LastUpdatedTime field
                              type: timestamp
                              timeFormat: rfc3339
                            - name: LAST_UPDATED_USER
                              description: LastUpdatedUser field
                              type: string
                            - name: DISPLAY_FILTER
                              description: DisplayFilter field
                              type: string
                            - name: RULE_GUID
                              description: RuleGUID field
                              type: string
                    - name: NewViolation
                      description: Violation Ref.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: REC_ID
                              description: RECID field
                              type: string
                            - name: REASON
                              description: Reason field
                              type: string
                            - name: RESOURCE
                              description: Resource field
                              type: string
                    - name: ViolationReason
                      description: A reason for the violation.
                      type: array
                      element:
                        type: object
                        fields:
                            - name: REC_ID
                              description: RECID field
                              type: string
                            - name: REASON
                              description: Reason field
                              type: string
    - name: SEVERITY
      required: true
      description: The severity level of the alert
      type: bigint
    - name: START_TIME
      required: true
      description: The event start time.
      type: timestamp
      timeFormat: strftime=%d %b %Y %H:%M %Z
      isEventTime: true
    - name: SUMMARY
      required: true
      description: The alert title and quick summary
      type: string
    - name: EVENT_TYPE
      required: true
      description: The type of event
      type: string
    - name: EVENT_NAME
      required: true
      description: The event name
      type: string
    - name: LINK
      required: true
      description: A link to the Lacework dashboard for the event
      type: string
    - name: EVENT_ID
      required: true
      description: The eventID reference
      type: bigint
    - name: ACCOUNT
      required: true
      description: The Lacework tenant that created the event
      type: string
    - name: SOURCE
      required: true
      description: The data source the event triggered on
      type: string
```

### Lacework.HostVulnDetails

Lacework.HostVulnDetails provides details around any vulnerabilities on hosts across your environment.

Reference: [Lacework Documentation on HostVulnDetails](https://docs.lacework.com/console/hostvulndetailsv-view).

```yaml
fields:
  - name: FIX_INFO
    type: object
    fields:
      - name: compare_result
        required: true
        type: string
      - name: eval_status
        required: true
        type: string
      - name: fix_available
        required: true
        type: string
      - name: fixed_version
        required: true
        type: string
      - name: fixed_version_comparison_infos
        required: true
        type: array
        element:
          type: object
          fields:
            - name: curr_fix_ver
              required: true
              type: string
            - name: is_curr_fix_ver_greater_than_other_fix_ver
              required: true
              type: string
            - name: other_fix_ver
              required: true
              type: string
      - name: fixed_version_comparison_score
        required: true
        type: bigint
      - name: version_installed
        required: true
        type: string
  - name: SEVERITY
    type: string
  - name: STATUS
    type: string
  - name: VULN_ID
    type: string
  - name: CVE_PROPS
    required: true
    type: object
    fields:
      - name: cve_batch_id
        type: string
      - name: description
        type: string
      - name: link
        type: string
        indicators:
          - url
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: EVAL_CTX
    required: true
    type: object
    fields:
      - name: data_source
        required: true
        type: string
      - name: hostname
        required: true
        type: string
      - name: mc_eval_guid
        required: true
        type: string
  - name: FEATURE_KEY
    required: true
    type: object
    fields:
      - name: name
        required: true
        type: string
      - name: namespace
        required: true
        type: string
      - name: package_active
        required: true
        type: boolean
      - name: package_path
        required: true
        type: string
      - name: version_installed
        required: true
        type: string
  - name: MACHINE_TAGS
    required: true
    type: json
  - name: MID
    required: true
    type: bigint
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.Image

Lacework.Image provides details about any container images in your environment.

Reference: [Lacework Documentation on Images](https://docs.lacework.com/console/imagev-view).

```yaml
fields:
  - name: CONTAINER_TYPE
    required: true
    type: string
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: IMAGE_ID
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: REPO
    required: true
    type: string
  - name: SIZE
    required: true
    type: bigint
  - name: TAG
    required: true
    type: string
```

### Lacework.Interfaces

Lacework.Interfaces monitors any discovered network interfaces across your environment.

Reference: [Lacework Documentation on Interfaces](https://docs.lacework.com/console/interfacesv-view).

```yaml
fields:
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: HW_ADDR
    required: true
    type: string
  - name: IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: MID
    required: true
    type: bigint
  - name: NAME
    required: true
    type: string
```

### Lacework.InternalIPA

Lacework.InternalIPA monitors any internal IP addresses across your environment.

Reference: [Lacework Documentation on InternalIPA](https://docs.lacework.com/console/internalipav-view).

```yaml
fields:
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: MID
    required: true
    type: bigint
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.MachineDetails

Lacework.MachineDetails aggregates historical data about any machines found in your environment.

Reference: [Lacework Documentation on MachineDetails](https://docs.lacework.com/console/machinedetailsv-view).

```yaml
fields:
  - name: AWS_INSTANCE_ID
    type: string
    indicators:
      - aws_instance_id
  - name: AWS_ZONE
    type: string
  - name: TAGS
    type: json
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: DOMAIN
    required: true
    type: string
    indicators:
      - domain
  - name: HOSTNAME
    required: true
    type: string
    indicators:
      - hostname
  - name: KERNEL
    required: true
    type: string
  - name: KERNEL_RELEASE
    required: true
    type: string
  - name: KERNEL_VERSION
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: OS
    required: true
    type: string
  - name: OS_VERSION
    required: true
    type: string
```

### Lacework.MachineSummary

Lacework.MachineSummary summarizes and aggregates details about machines in your environment.

Reference: [Lacework Documentation on MachineSummary](https://docs.lacework.com/console/machinesummaryv-view).

```yaml
fields:
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: ENTITY_TYPE
    required: true
    type: string
  - name: HOSTNAME
    required: true
    type: string
  - name: MACHINE_TAGS
    required: true
    type: json
  - name: MID
    required: true
    type: bigint
  - name: PRIMARY_IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.NewHashes

Lacework.NewHashes tracks any new file hashes in your environment.

Reference: [Lacework Documentation on NewHashes](https://docs.lacework.com/console/newhashesv-view).

```yaml
fields:
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: FILEDATA_HASH
    required: true
    type: string
    indicators:
      - sha256
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.Package

Lacework.Package tracks any packages in your environment.

Reference: [Lacework Documentation on Packages](https://docs.lacework.com/console/packagev-view).

```yaml
fields:
  - name: ARCH
    required: true
    type: string
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: MID
    required: true
    type: bigint
  - name: PACKAGE_NAME
    required: true
    type: string
  - name: VERSION
    required: true
    type: string
```

### Lacework.PodSummary

Lacework.PodSummary tracks any pods (collections of one or more containers) in your environment.

Reference: [Lacework Documentation on PodSummary](https://docs.lacework.com/console/podsummaryv-view).

```yaml
fields:
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: MID
    required: true
    type: bigint
  - name: POD_NAME
    required: true
    type: string
  - name: PRIMARY_IP_ADDR
    required: true
    type: string
    indicators:
      - ip
  - name: PROPS_CONTAINER
    required: true
    type: json
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
```

### Lacework.ProcessSummary

Lacework.ProcessSummary tracks any processes running in your environment.

Reference: [Lacework Documentation on ProcessSummary](https://docs.lacework.com/console/processsummaryv-view).

```yaml
fields:
  - name: POD_NAME
    type: string
  - name: CONTAINER_ID
    type: string
  - name: CMDLINE_HASH
    required: true
    type: string
    indicators:
      - md5
  - name: END_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: FILE_PATH
    required: true
    type: string
  - name: MID
    required: true
    type: bigint
  - name: PID
    required: true
    type: bigint
  - name: PPID
    required: true
    type: bigint
  - name: PROCESS_START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: START_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
  - name: UID
    required: true
    type: bigint
  - name: USERNAME
    required: true
    type: string
    indicators:
      - username
```

### Lacework.UserDetails

Lacework.UserDetails tracks historical data about any users in your environment.

Reference: [Lacework Documentation on UserDetails](https://docs.lacework.com/console/userdetailsv-view).

```yaml
fields:
  - name: OTHER_GROUP_NAMES
    type: array
    element:
      type: string
  - name: CREATED_TIME
    required: true
    type: timestamp
    timeFormat: '%Y-%m-%d %H:%M:%S.%f'
    isEventTime: true
  - name: MID
    required: true
    type: bigint
  - name: PRIMARY_GROUP_NAME
    required: true
    type: string
  - name: UID
    required: true
    type: bigint
  - name: USERNAME
    required: true
    type: string
    indicators:
      - username
```
