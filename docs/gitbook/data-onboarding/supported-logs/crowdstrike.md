---
description: Onboarding CrowdStrike logs into your Panther Console
---

# CrowdStrike Logs

## Overview

Panther supports pulling logs directly from CrowdStrike events by integrating with the [CrowdStrike Falcon Data Replicator](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide).

## How to onboard CrowdStrike logs to Panther

### Prerequisites

* You must have an active subscription to [Falcon Data Replicator](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-overview-of-falcon-data-replicator) and it must be enabled in Crowdstrike.
* There is no minimum version of Falcon Data Replicator required.

### Step 1: Create FDR API Keys

1. Log in to your CrowdStrike Falcon console.
2. Navigate to the **API Clients and Keys** page.
3. Click **Create new credentials** under the **FDR AWS S3 Credentials and SQS Queue** section.
4. Copy down the Client ID, Secret ID, and SQS URL for the next steps.

![](<../../.gitbook/assets/image (5) (1) (1).png>)

### Step 2: Create a New CrowdStrike Source in Panther

1. Log in to your Panther Console.
2. In the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Create New.**
4. Select **CrowdStrike** from the list of available log sources. Click **Start Source Setup**.
5. Fill in the fields below:
   * **Name**: A memorable name for the source e.g. `CrowdStrike Falcon`.
   * **SQS Url**: The URL for the CrowdStrike-managed SQS queue, previously copied.
   * **AWS Access Key**, **AWS Access Secret**: The AWS access key and secret, previously copied.\
     \
     ![](<../../.gitbook/assets/image (24) (1) (1) (1) (1).png>)\

6. Click **Continue Setup.**
7. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
   * This feature sends an error message if logs aren't received within a specified time interval.
8. Click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schema are listed as **"required: true"**  just below the "name" field.
{% endhint %}

### Crowdstrike.AIDMaster

Sensor and Host information provided by Falcon Insight.

Reference: [CrowdStrike Documentation on Falcon Data Replicator.](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide)

```yaml
schema: Crowdstrike.AIDMaster
parser:
    native:
        name: Crowdstrike.AIDMaster
description: Sensor and Host information provided by Falcon Insight
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-aid-master
version: 0
fields:
    - name: Time
      required: true
      description: Timestamp of when the event was received by the CrowdStrike cloud. This is not to be confused with the time the event was generated locally on the system (the _timeevent). This is the timestamp of the event from the cloud's point of view. This value can be converted to any time format and can be used for calculations.
      type: timestamp
      timeFormat: unix
      isEventTime: true
    - name: AgentLoadFlags
      required: true
      description: 'Whether the sensor loaded during or after the Windows host''s boot process. Example values: 0, 1'
      type: int
    - name: AgentLocalTime
      required: true
      description: The local time for the sensor in epoch format.
      type: timestamp
      timeFormat: unix
    - name: AgentTimeOffset
      required: true
      description: The time since the last reboot in epoch format.
      type: float
    - name: AgentVersion
      required: true
      description: The version of the sensor running on a host.
      type: string
    - name: aid
      required: true
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: cid
      required: true
      description: The customer ID.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      required: true
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: BiosManufacturer
      description: The manufacturer of the host's BIOS.
      type: string
    - name: BiosVersion
      description: The version of the host's BIOS.
      type: string
    - name: ChassisType
      description: Type of system chassis, as defined in SMBIOS Standard.
      type: string
    - name: City
      description: The system's city of origin.
      type: string
    - name: Country
      description: The system's country of origin.
      type: string
    - name: Continent
      description: The sensor's continent, as seen from the CrowdStrike cloud.
      type: string
    - name: ComputerName
      description: The name of the host.
      type: string
    - name: ConfigBuild
      description: ConfigBuild field
      type: string
    - name: ConfigIDBuild
      description: Build number used as part of the ConfigID.
      type: string
    - name: event_platform
      description: 'The platform the sensor is running on. Example values: ''Win'', ''Lin'', ''Mac''.'
      type: string
    - name: FalconGroupingTags
      description: FalconGroupingTags field
      type: string
    - name: FirstSeen
      description: The first time the sensor was seen by the CrowdStrike cloud in epoch format.
      type: timestamp
      timeFormat: unix
    - name: MachineDomain
      description: The Windows domain name to which the host is currently joined.
      type: string
    - name: OU
      description: The organizational unit of the host as seen by the sensor (defined by system admin).
      type: string
    - name: PointerSize
      description: 'The processor architecture (in decimal, non-hex format): ''4'' for 32-bit, ''8'' for 64-bit, or ''none'' for unknown.'
      type: string
    - name: ProductType
      description: 'The type of product (in decimal, non-hex format). Example values: ''1'' (Workstation), ''2'' (Domain Controller), ''3'' (Server).'
      type: string
    - name: SensorGroupingTags
      description: SensorGroupingTags field
      type: string
    - name: ServicePackMajor
      description: 'The major version # of the OS Service Pack (in decimal, non-hex format).'
      type: string
    - name: SiteName
      description: The site name of the domain to which the host is joined (defined by system admin).
      type: string
    - name: SystemManufacturer
      description: The host's system manufacturer.
      type: string
    - name: SystemProductName
      description: The host's product name.
      type: string
    - name: Timezone
      description: The sensor's time zone, as seen from the CrowdStrike cloud.
      type: string
    - name: Version
      description: The host's system version.
      type: string
    - name: HostHiddenStatus
      description: Whether the host is visible or not.
      type: string
```

### Crowdstrike.ActivityAudit

Contains activity audit information.

Reference: [CrowdStrike Documentation on Streaming API Event Authentication.](https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events#section-authentication)

```yaml
schema: Crowdstrike.ActivityAudit
parser:
    native:
        name: Crowdstrike.ActivityAudit
description: Contains activity audit information
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events#section-authentication
version: 0
fields:
    - name: AgentIdString
      description: The Agent ID
      type: string
    - name: cid
      description: The customer ID. A 32-character (hex) identifier in the CrowdStrike cloud.
      type: string
      indicators:
        - md5
        - trace_id
    - name: ExternalApiType
      required: true
      description: The external API type
      type: string
    - name: Nonce
      description: The nonce
      type: bigint
    - name: ServiceName
      description: The service name
      type: string
    - name: UserId
      description: User that performed the operation, e.g. person that performed the operation to create a new user account.
      type: string
      indicators:
        - email
    - name: UserIp
      description: IP address of user that performs the operation.
      type: string
      indicators:
        - ip
    - name: CustomerIdString
      description: Unique ID assigned by CS for each customer.
      type: string
    - name: EventType
      required: true
      description: Will be Event_ExternalApiEvent
      type: string
    - name: OperationName
      description: The operation name
      type: string
    - name: UTCTimestamp
      description: The timestamp
      type: timestamp
      timeFormat: unix_ms
    - name: timestamp
      required: true
      description: The timestamp
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: AuditKeyValues
      description: The AuditKeyValues
      type: array
      element:
        type: object
        fields:
            - name: Key
              description: The Key
              type: string
            - name: ValueString
              description: The value as a string
              type: string
    - name: eid
      description: The EID
      type: bigint
    - name: Success
      description: If the operation was successful or not
      type: boolean
    - name: EventUUID
      description: The EventUUID
      type: string
```

### Crowdstrike.AppInfo

Detected Application Information provided by Falcon Discover.

Reference: [CrowdStrike Documentation on Falcon Data Replicator AppInfo.](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-appinfo)

```yaml
schema: Crowdstrike.AppInfo
parser:
    native:
        name: Crowdstrike.AppInfo
description: Detected Application Information provided by Falcon Discover
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-appinfo
version: 0
fields:
    - name: _time
      required: true
      description: The host's local time in epoch format.
      type: timestamp
      timeFormat: unix
      isEventTime: true
    - name: cid
      required: true
      description: The customer ID.
      type: string
      indicators:
        - md5
        - trace_id
    - name: CompanyName
      required: true
      description: The name of the company.
      type: string
    - name: detectioncount
      required: true
      description: The number of detections.
      type: bigint
    - name: FileName
      required: true
      description: The name of the file.
      type: string
    - name: SHA256HashData
      required: true
      description: The file hash bashed on SHA-256.
      type: string
      indicators:
        - sha256
    - name: FileDescription
      description: The description of the file, if any.
      type: string
    - name: FileVersion
      description: The version of the file.
      type: string
    - name: ProductName
      description: The name of the product.
      type: string
    - name: ProductVersion
      description: The version of the product.
      type: string
```

### Crowdstrike.CriticalFile

This event is generated every time a critical file is accessed or modified.

Reference: [CrowdStrike Documentation on CriticalFile.](https://falcon.us-2.crowdstrike.com/support/documentation/26/events-data-dictionary)

```yaml
schema: Crowdstrike.CriticalFile
parser:
    native:
        name: Crowdstrike.CriticalFile
description: This event is generated every time a critical file is accessed or modified
referenceURL: https://falcon.us-2.crowdstrike.com/support/documentation/26/events-data-dictionary
version: 0
fields:
    - name: event_simpleName
      required: true
      description: Event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: EffectiveTransmissionClass
      description: Effective transmission class
      type: bigint
    - name: GID
      description: The user Group ID
      type: bigint
    - name: TargetFileName
      description: The file that was accessed
      type: string
    - name: UID
      description: The User ID
      type: bigint
    - name: UnixMode
      description: The unix file permissions
      type: string
    - name: FileIdentifier
      description: The file identifier
      type: string
    - name: USN
      description: The USN
      type: bigint
```

### Crowdstrike.DNSRequest

This event is generated for every attempted DNS name resolution on a host.

Reference: [CrowdStrike Documentation on DNSRequest.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-DnsRequest)

```yaml
schema: Crowdstrike.DNSRequest
parser:
    native:
        name: Crowdstrike.DNSRequest
description: This event is generated for every attempted DNS name resolution on a host.
version: 0
fields:
    - name: event_simpleName
      required: true
      description: Event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: EffectiveTransmissionClass
      description: Effective transmission class
      type: bigint
    - name: DomainName
      description: The domain name requested
      type: string
      indicators:
        - domain
    - name: InterfaceIndex
      description: The network interface index (Windows only)
      type: bigint
    - name: DualRequest
      description: If the event is dual request (Windows only)
      type: bigint
    - name: DnsRequestCount
      description: The number of DNS requests (Windows only)
      type: bigint
    - name: AppIdentifier
      description: The identifier of the app that made the request (Android, iOS)
      type: string
    - name: IpAddress
      description: The device ip address (Android, iOS)
      type: string
      indicators:
        - ip
    - name: RequestType
      description: The DNS request type
      type: string
```

### Crowdstrike.DetectionSummary

Detection Summary events include multiple detections, when multiple malicious behaviors are detected.

Reference: [CrowdStrike Documentation on Streaming API Detection Summary.](https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events#section-detection-summary)

```yaml
schema: Crowdstrike.DetectionSummary
parser:
    native:
        name: Crowdstrike.DetectionSummary
description: Detection Summary events include multiple detections, when multiple malicious behaviors are detected.
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events#section-detection-summary
version: 0
fields:
    - name: cid
      description: Customer ID
      type: string
      indicators:
        - md5
        - trace_id
    - name: Technique
      description: The name of the technique associated to the behavior.
      type: string
    - name: ProcessId
      description: Process ID.
      type: bigint
    - name: AgentIdString
      description: Agent Id.
      type: string
    - name: DetectName
      description: 'NOTE: The DetectName field has been replaced by Objective, Tactic, and Technique as we have aligned with MITRE’s ATT&CK. DetectName will be deprecated January 16, 2019 - more information'
      type: string
    - name: ComputerName
      description: Host name.
      type: string
    - name: ProcessStartTime
      description: Timestamp of when a process started.
      type: timestamp
      timeFormat: unix
    - name: GrandparentCommandLine
      description: Effective transmission class
      type: string
    - name: MACAddress
      description: The MAC Address
      type: string
    - name: CommandLine
      description: The command line execution of the process.
      type: string
    - name: Objective
      description: The name of the objective associated to the behavior.
      type: string
    - name: Nonce
      description: The nonce.
      type: bigint
    - name: SHA256String
      description: SHA256 hash.
      type: string
      indicators:
        - sha256
    - name: ExternalApiType
      required: true
      description: The type of the External API
      type: string
    - name: PatternDispositionValue
      description: The pattern disposition value.
      type: bigint
    - name: DetectId
      description: 'The Detection ID for the detection. Can be used in other APIs, such as Detection Resolution and ThreatGraph. Example: ldt:05c0273d48f2432271b2f1d1b49264b5:4297692922'
      type: string
    - name: Severity
      description: The severity
      type: bigint
    - name: PatternDispositionDescription
      description: The description of the pattern associated to the action taken on the behavior.
      type: string
    - name: SeverityName
      description: The severity name.
      type: string
    - name: MD5String
      description: MD5 hash
      type: string
      indicators:
        - md5
    - name: EventUUID
      description: Event UUID
      type: string
    - name: UserName
      description: User name.
      type: string
      indicators:
        - username
    - name: FilePath
      description: Full path of the file, excluding the file name.
      type: string
    - name: timestamp
      description: The timestamp
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
    - name: ParentCommandLine
      description: The command line of the parent process.
      type: string
    - name: DetectDescription
      description: 'A description of what an adversary was trying to do in the environment and guidance on how to begin an investigation. NOTE: While these descriptions are robust and drive a helpful console experience, we encourage you to not use this field to drive workflows, as values are updated and added regularly.'
      type: string
    - name: LocalIP
      description: The local IP.
      type: string
      indicators:
        - ip
    - name: ProcessEndTime
      description: Timestamp of when a process ended in UNIX EPOCH time.
      type: timestamp
      timeFormat: unix
    - name: SHA1String
      description: SHA1 hash
      type: string
      indicators:
        - sha1
    - name: OriginSourceIpAddress
      description: The OriginSourceIpAddress.
      type: string
      indicators:
        - ip
    - name: GrandparentImageFileName
      description: The GrandparentImageFileName
      type: string
    - name: MachineDomain
      description: The Windows Domain Name to which the machine is currently joined.
      type: string
    - name: ParentImageFileName
      description: The ParentImageFileName
      type: string
    - name: FalconHostLink
      description: Link to view detection event in Falcon console.
      type: string
    - name: UTCTimestamp
      description: The UTC timestamp.
      type: timestamp
      timeFormat: unix_ms
    - name: FileName
      description: File name if a file is involved in the detection.
      type: string
    - name: ParentProcessId
      description: Parent Process ID.
      type: bigint
    - name: EventType
      required: true
      description: The EventType.
      type: string
    - name: CustomerIdString
      description: Unique ID assigned by CS for each customer.
      type: string
    - name: Tactic
      description: The name of the tactic associated to the behavior.
      type: string
    - name: SensorId
      description: Falcon sensor Agent ID.
      type: string
    - name: eid
      description: The EID.
      type: bigint
    - name: PatternDispositionFlags
      description: The pattern disposition flags
      type: json
```

### Crowdstrike.GroupIdentity

Provides the sensor boot unique mapping between GID, AuthenticationId, UserPrincipal, and UserSid. Available only for the Mac platform.

Reference: [CrowdStrike Documentation on Group Identity Events.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-GroupIdentity)

```yaml
schema: Crowdstrike.GroupIdentity
parser:
    native:
        name: Crowdstrike.GroupIdentity
description: Provides the sensor boot unique mapping between GID, AuthenticationId, UserPrincipal, and UserSid. Available only for the Mac platform.
referenceURL: https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-GroupIdentity
version: 0
fields:
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: event_simpleName
      required: true
      description: Event Name
      type: string
    - name: GID
      required: true
      description: The user Group ID.
      type: bigint
    - name: AuthenticationUuid
      required: true
      description: AuthenticationUUID field
      type: string
    - name: AuthenticationUuidAsString
      required: true
      description: AuthenticationUUIDAsString field
      type: string
    - name: AuthenticationId
      required: true
      description: 'Values: INVALID_LUID (0), NETWORK_SERVICE (996), LOCAL_SERVICE (997), SYSTEM (999), RESERVED_LUID_MAX (1000)'
      type: int
    - name: UserPrincipal
      required: true
      description: UserPrincipal field
      type: string
    - name: UserSid
      required: true
      description: The User Security Identifier (UserSID) of the user who executed the command. A UserSID uniquely identifies a user in a system.
      type: string
```

### Crowdstrike.ManagedAssets

Sensor and Host information provided by Falcon Insight (Network Information: IP Address, LAN/Ethernet Interface, Gateway Address, MAC Address).

Reference: [CrowdStrike Documentation on Falcon Data Replicator Managed Assets.](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-managedassets)

```yaml
schema: Crowdstrike.ManagedAssets
parser:
    native:
        name: Crowdstrike.ManagedAssets
description: 'Sensor and Host information provided by Falcon Insight (Network Information: IP Address, LAN/Ethernet Interface, Gateway Address, MAC Address)'
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-managedassets
version: 0
fields:
    - name: _time
      required: true
      description: The host's local time in epoch format.
      type: timestamp
      timeFormat: unix
      isEventTime: true
    - name: aid
      required: true
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: cid
      required: true
      description: The customer ID.
      type: string
      indicators:
        - md5
        - trace_id
    - name: GatewayIP
      description: The gateway of the system where the sensor is installed.
      type: string
      indicators:
        - ip
    - name: GatewayMAC
      description: The MAC address of the gateway.
      type: string
    - name: MACPrefix
      required: true
      description: An identifier unique to the organization.
      type: string
    - name: MAC
      required: true
      description: The MAC address of the system.
      type: string
    - name: LocalAddressIP4
      required: true
      description: The device's local IP address in IPv4 format.
      type: string
      indicators:
        - ip
    - name: InterfaceAlias
      description: The user-friendly name of the IP interface.
      type: string
    - name: InterfaceDescription
      description: The network adapter used for the IP interface.
      type: string
```

### Crowdstrike.NetworkConnect

This event is generated when an application attempts a remote connection on an interface.

Reference: [CrowdStrike Documentation on NetworkConnect.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-NetworkConnectIP4)

```yaml
schema: Crowdstrike.NetworkConnect
parser:
    native:
        name: Crowdstrike.NetworkConnect
description: This event is generated when an application attempts a remote connection on an interface
version: 0
fields:
    - name: event_simpleName
      required: true
      description: Event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: LocalAddressIP4
      description: Local IPv4 address for the connection
      type: string
      indicators:
        - ip
    - name: LocalAddressIP6
      description: Local IPv6 address for the connection
      type: string
      indicators:
        - ip
    - name: RemoteAddressIP4
      description: Remote IPv4 address for the connection
      type: string
      indicators:
        - ip
    - name: RemoteAddressIP6
      description: Remote IPv6 address for the connection
      type: string
      indicators:
        - ip
    - name: ConnectionFlags
      description: Connection flags (PROMISCUOUS_MODE_SIO_RCVALL = 2, RAW_SOCKET = 1, PROMISCUOUS_MODE_SIO_RCVALL_IGMPMCAST = 4, PROMISCUOUS_MODE_SIO_RCVALL_MCAST = 8)
      type: int
    - name: Protocol
      description: IP Protocol (ICMP = 1, TCP = 6, UDP = 17)
      type: int
    - name: LocalPort
      description: Connection local port
      type: int
    - name: RemotePort
      description: Connection remote port
      type: int
    - name: ConnectionDirection
      description: Direction of the connection (OUTBOUND = 0, INBOUND = 1, NEITHER = 2, BOTH = 3)
      type: int
    - name: IcmpType
      description: ICMP type (N/A on iOS)
      type: string
    - name: IcmpCode
      description: ICMP code (N/A on iOS)
      type: string
```

### Crowdstrike.NetworkListen

This event is generated when an application establishes a socket in listening mode.

Reference: [CrowdStrike Documentation on NetworkListen.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-NetworkListenIP4)

```yaml
schema: Crowdstrike.NetworkListen
parser:
    native:
        name: Crowdstrike.NetworkListen
description: This event is generated when an application establishes a socket in listening mode
version: 0
fields:
    - name: event_simpleName
      required: true
      description: event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: LocalAddressIP4
      description: Local IPv4 address for the connection
      type: string
      indicators:
        - ip
    - name: LocalAddressIP6
      description: Local IPv6 address for the connection
      type: string
      indicators:
        - ip
    - name: RemoteAddressIP4
      description: Remote IPv4 address for the connection
      type: string
      indicators:
        - ip
    - name: RemoteAddressIP6
      description: Remote IPv6 address for the connection
      type: string
      indicators:
        - ip
    - name: ConnectionFlags
      description: Connection flags (PROMISCUOUS_MODE_SIO_RCVALL = 2, RAW_SOCKET = 1, PROMISCUOUS_MODE_SIO_RCVALL_IGMPMCAST = 4, PROMISCUOUS_MODE_SIO_RCVALL_MCAST = 8)
      type: int
    - name: Protocol
      description: IP Protocol (ICMP = 1, TCP = 6, UDP = 17)
      type: int
    - name: LocalPort
      description: Connection local port
      type: int
    - name: RemotePort
      description: Connection remote port
      type: int
    - name: ConnectionDirection
      description: Direction of the connection (OUTBOUND = 0, INBOUND = 1, NEITHER = 2, BOTH = 3)
      type: int
```

### Crowdstrike.NotManagedAssets

Unmanaged Host discovery information provided by Falcon Insight.

Reference: [CrowdStrike Documentation on Falcon Data Replicator Notmanaged Assets.](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-notmanaged)

```yaml
schema: Crowdstrike.NotManagedAssets
parser:
    native:
        name: Crowdstrike.NotManagedAssets
description: Unmanaged Host discovery information provided by Falcon Insight
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-notmanaged
version: 0
fields:
    - name: _time
      required: true
      description: The host's local time in epoch format.
      type: timestamp
      timeFormat: unix
      isEventTime: true
    - name: aip
      required: true
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: aipCount
      required: true
      description: The number of public-facing IP addresses.
      type: bigint
    - name: localipCount
      required: true
      description: The number of local IP addresses.
      type: bigint
    - name: cid
      required: true
      description: The customer ID.
      type: string
      indicators:
        - md5
        - trace_id
    - name: CurrentLocalIP
      required: true
      description: The current local IP address of the machine, found via the IPv4 network discovery protocol.
      type: string
      indicators:
        - ip
    - name: subnet
      description: The subnet of the system.
      type: string
    - name: MAC
      required: true
      description: The MAC address of the system.
      type: string
    - name: MACPrefix
      required: true
      description: An identifier unique to the organization.
      type: string
    - name: discovererCount
      required: true
      description: The number of aid's that have discovered this system.
      type: bigint
    - name: discoverer_aid
      description: The agent IDs that have discovered this system.
      type: array
      element:
        type: string
    - name: discoverer_devicetype
      description: The type of device that discovered this system ('VM' or 'Server').
      type: string
    - name: FirstDiscoveredDate
      description: The first time the system was discovered in epoch format.
      type: timestamp
      timeFormat: unix
    - name: LastDiscoveredBy
      description: The host ID of the host that most recently discovered this device.
      type: string
    - name: LocalAddressIP4
      description: The device's local IP address in IPv4 format.
      type: string
      indicators:
        - ip
    - name: ComputerName
      description: The name of the host that discovered the neighbor.
      type: string
    - name: NeighborName
      description: The neighbor's host name.
      type: string
```

### Crowdstrike.ProcessRollup2

This event (often called "PR2" for short) is generated for a process that is running or has finished running on a host and contains information about that process.

Reference: [CrowdStrike Documentation on ProcessRollup2.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-ProcessRollup2)

```yaml
schema: Crowdstrike.ProcessRollup2
parser:
    native:
        name: Crowdstrike.ProcessRollup2
description: This event (often called "PR2" for short) is generated for a process that is running or has finished running on a host and contains information about that process.
version: 0
fields:
    - name: event_simpleName
      required: true
      description: Event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: TargetProcessId
      description: The unique ID of a target process
      type: bigint
    - name: SourceProcessId
      description: The unique ID of creating process.
      type: bigint
    - name: SourceThreadId
      description: The unique ID of thread from creating process.
      type: bigint
    - name: ParentProcessId
      description: The unique ID of the parent process.
      type: bigint
    - name: ImageFileName
      description: The full path to an executable (PE) file. The context of this field provides more information as to its meaning. For ProcessRollup2 events, this is the full path to the main executable for the created process
      type: string
    - name: CommandLine
      description: The command line used to create this process. May be empty in some circumstances
      type: string
    - name: RawProcessId
      description: The operating system’s internal PID. For matching, use the UPID fields which guarantee a unique process identifier
      type: bigint
    - name: ProcessStartTime
      description: The time the process began in UNIX epoch time (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix
    - name: ProcessEndTime
      description: The time the process finished (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix
    - name: SHA256HashData
      description: The SHA256 hash of a file. In most cases, the hash of the file referred to by the ImageFileName field.
      type: string
      indicators:
        - sha256
    - name: SHA1HashData
      description: The SHA1 hash of a file
      type: string
      indicators:
        - sha1
    - name: MD5HashData
      description: The MD5 hash of a file
      type: string
      indicators:
        - md5
    - name: ImageSubsystem
      description: Subsystem of the image filename (Windows only)
      type: string
    - name: UserSid
      description: The User Security Identifier (UserSID) of the user who executed the command. A UserSID uniquely identifies a user in a system. (Windows only)
      type: string
    - name: AuthenticationId
      description: The authentication identifier (Windows only)
      type: string
    - name: IntegrityLevel
      description: The integrity level (Windows only)
      type: string
    - name: ProcessCreateFlags
      description: Captured flags from original process create. This is a bitfield. (Windows only)
      type: string
    - name: ProcessParameterFlags
      description: Flags from the ‘NtCreateUserProcess’ API. This bitfield includes data like if DLL redirection is enabled. (Windows only)
      type: string
    - name: ProcessSxsFlags
      description: Flags from the communications path with the Windows Subsystem Process. This bitfield includes data like if there’s a manifest and if it’s local or not. (Windows only)
      type: string
    - name: ParentAuthenticationId
      description: The authentication identifier for the parent process (Windows only)
      type: string
    - name: TokenType
      description: The token type (Windows only)
      type: string
    - name: SessionId
      description: The id of the session (Windows only)
      type: string
    - name: WindowFlags
      description: Flags from the window (Windows only)
      type: string
    - name: ShowWindowFlags
      description: Window visibility flags (Windows only)
      type: string
    - name: WindowStartingPositionHorizontal
      description: Start horizontal position of the process window (Windows only)
      type: bigint
    - name: WindowStartingPositionVertical
      description: Start vertical position of the process window (Windows only)
      type: bigint
    - name: WindowStartingWidth
      description: Start width of the process window (Windows only)
      type: bigint
    - name: WindowStartingHeight
      description: Start height of the process window (Windows only)
      type: bigint
    - name: Desktop
      description: The desktop of the process window (Windows only)
      type: string
    - name: WindowStation
      description: The  process window station (Windows only)
      type: string
    - name: WindowTitle
      description: The title of the process window (WindowsOnly)
      type: string
    - name: LinkName
      description: Link name (Windows only)
      type: string
    - name: ApplicationUserModelId
      description: Application user model id (WindowsOnly)
      type: string
    - name: CallStackModuleNames
      description: Call stack module names (Windows only)
      type: string
    - name: CallStackModuleNamesVersion
      description: Call stack module names version (Windows only)
      type: string
    - name: RpcClientProcessId
      description: RPC client process id (Windows only)
      type: string
    - name: CsaProcessDataCollectionInstanceId
      description: CSA process data collection instance id (Windows only)
      type: string
    - name: OriginalCommandLine
      description: The original command line used to create this process (Windows only)
      type: string
    - name: CreateProcessType
      description: Create process type (Windows only)
      type: string
    - name: ZoneIdentifier
      description: Zone identifier (Windows only)
      type: string
    - name: HostUrl
      description: Host URL (Windows only)
      type: string
    - name: ReferrerUrl
      description: Referrer URL (Windows only)
      type: string
      indicators:
        - url
    - name: GrandParent
      description: Grant parent (Windows only)
      type: string
    - name: BaseFileName
      description: Base file name (Windows only)
      type: string
    - name: Tags
      description: Process tags comma separated list (Windows, Mac)
      type: string
    - name: ParentBaseFileName
      description: Parent process base file name (Windows, Mac)
      type: string
    - name: ProcessGroupId
      description: Process group id (Windows, Mac)
      type: bigint
    - name: UID
      description: UID (Mac, Linux, Android)
      type: bigint
    - name: RUID
      description: RUID (Mac, Linux, Android)
      type: bigint
    - name: SVUID
      description: SVUID (Mac, Linux, Android)
      type: bigint
    - name: GID
      description: GID (Mac, Linux, Android)
      type: bigint
    - name: RGID
      description: RGID (Mac, Linux, Android)
      type: bigint
    - name: SVGID
      description: SVGID (Mac, Linux, Android)
      type: bigint
    - name: SessionProcessId
      description: Session process id (Mac, Linux)
      type: bigint
    - name: MachOSubType
      description: MachOSubType (Mac only)
      type: string
    - name: TtyName
      description: TTY name (Linux only)
      type: string
    - name: OciContainerId
      description: OCI Container id (Linux only)
      type: string
    - name: SourceAndroidComponentName
      description: Source component name (Android only)
      type: string
    - name: TargetAndroidComponentName
      description: Target component name (Android only)
      type: string
    - name: TargetAndroidComponentType
      description: Target component type (Android only)
      type: string
```

### Crowdstrike.ProcessRollup2Stats

When a process finishes running, the sensor generates and sends a ProcessRollup2 event. Mac and Linux sensors send far more ProcessRollup2 events than Windows (roughly 20x as many), so rather than send events for every process on those hosts, the sensor sends an initial ProcessRollup2 event, followed 10 minutes later by a ProcessRollup2Stats event with a SHA256 hash and the count of how many times the hash executed in the last 10 minutes.

Reference: [CrowdStrike Documentation on ProcessRollup2Stats.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-ProcessRollup2Stats)

```yaml
schema: Crowdstrike.ProcessRollup2Stats
parser:
    native:
        name: Crowdstrike.ProcessRollup2Stats
description: When a process finishes running, the sensor generates and sends a ProcessRollup2 event. Mac and Linux sensors send far more ProcessRollup2 events than Windows (roughly 20x as many), so rather than send events for every process on those hosts, the sensor sends an initial ProcessRollup2 event, followed 10 minutes later by a ProcessRollup2Stats event with a SHA256 hash and the count of how many times the hash executed in the last 10 minutes.
version: 0
fields:
    - name: event_simpleName
      required: true
      description: event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: EffectiveTransmissionClass
      description: Effective transmission class
      type: bigint
    - name: SHA256HashData
      description: The SHA256 hash of a file. In most cases, the hash of the file referred to by the ImageFileName field.
      type: string
      indicators:
        - sha256
    - name: CommandLine
      description: The command line used to create this process. May be empty in some circumstances
      type: string
    - name: UID
      description: UID (Mac)
      type: bigint
    - name: ProcessCount
      description: The ProcessCount.
      type: bigint
    - name: Timeout
      description: The timeout
      type: bigint
    - name: ParentProcessId
      description: The unique ID of the parent process.
      type: bigint
    - name: SuppressType
      description: 'Values: GLOBAL (0) PARENT (1) UID (2) UIDNORMALIZED (3) PREFILTER (4) TIMEOUT_CHECK (5)'
      type: bigint
    - name: BoundedCount
      description: The bounded count
      type: bigint
```

### Crowdstrike.SyntheticProcessRollup2

A synthetic version of the process rollup (PR2) event.

Reference: [CrowdStrike Documentation on SyntheticProcessRollup2.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-SyntheticProcessRollup2)

```yaml
schema: Crowdstrike.SyntheticProcessRollup2
parser:
    native:
        name: Crowdstrike.SyntheticProcessRollup2
description: A synthetic version of the process rollup (PR2) event
version: 0
fields:
    - name: event_simpleName
      required: true
      description: event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: TargetProcessId
      description: The unique ID of a target process
      type: bigint
    - name: SourceProcessId
      description: The unique ID of creating process.
      type: bigint
    - name: SourceThreadId
      description: The unique ID of thread from creating process.
      type: bigint
    - name: ParentProcessId
      description: The unique ID of the parent process.
      type: bigint
    - name: ImageFileName
      description: The full path to an executable (PE) file. The context of this field provides more information as to its meaning. For ProcessRollup2 events, this is the full path to the main executable for the created process
      type: string
    - name: CommandLine
      description: The command line used to create this process. May be empty in some circumstances
      type: string
    - name: RawProcessId
      description: The operating system’s internal PID. For matching, use the UPID fields which guarantee a unique process identifier
      type: bigint
    - name: ProcessStartTime
      description: The time the process began in UNIX epoch time (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix
    - name: ProcessEndTime
      description: The time the process finished (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix
    - name: SHA256HashData
      description: The SHA256 hash of a file. In most cases, the hash of the file referred to by the ImageFileName field.
      type: string
      indicators:
        - sha256
    - name: SHA1HashData
      description: The SHA1 hash of a file
      type: string
      indicators:
        - sha1
    - name: MD5HashData
      description: The MD5 hash of a file
      type: string
      indicators:
        - md5
    - name: SyntheticPR2Flags
      description: PR2 flags (PROCESS_RUNDOWN = 0, PROCESS_HOLLOWED = 1, IMAGEHASH_FAILURE = 4, FILE_PATH_EXCLUDED = 8, PROCESS_FORK_FOLDING = 16, APP_MONITORING = 2)
      type: int
    - name: ImageSubsystem
      description: Subsystem of the image filename (Windows only)
      type: string
    - name: UserSid
      description: The User Security Identifier (UserSID) of the user who executed the command. A UserSID uniquely identifies a user in a system. (Windows only)
      type: string
    - name: AuthenticationId
      description: The authentication identifier (Windows only)
      type: string
    - name: IntegrityLevel
      description: The integrity level (Windows only)
      type: string
    - name: ProcessGroupId
      description: Process group id (Mac)
      type: bigint
    - name: UID
      description: UID (Mac)
      type: bigint
    - name: RUID
      description: RUID (Mac)
      type: bigint
    - name: SVUID
      description: SVUID (Mac)
      type: bigint
    - name: GID
      description: GID (Mac)
      type: bigint
    - name: RGID
      description: RGID (Mac)
      type: bigint
    - name: SVGID
      description: SVGID (Mac)
      type: bigint
    - name: SessionProcessId
      description: Session process id (Mac)
      type: bigint
```

### Crowdstrike.Unknown

This schema contains all the Crowdstrike events that don't match to any of the registered types.

Reference: [CrowdStrike Documentation on API Event Types.](https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events)

```yaml
schema: Crowdstrike.Unknown
parser:
    native:
        name: Crowdstrike.Unknown
description: This table contains all the Crowdstrike events that don't match to any of the registered types
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events
version: 0
fields:
    - name: event_simpleName
      description: Event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: unknown_payload
      required: true
      description: The full JSON payload of the event
      type: json
```

### Crowdstrike.UserIdentity

The UserIdentity event is generated when a user logs in to a host. It conveys important security-related characteristics associated with a user to the CrowdStrike cloud, such as the user name. It’s normally generated once per security principal, and is thus not on its own a sign of a suspicious activity. Available for Mac & Windows platforms.

Reference: [CrowdStrike Documentation on User Identity Events.](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-UserIdentity)

```yaml
schema: Crowdstrike.UserIdentity
parser:
    native:
        name: Crowdstrike.UserIdentity
description: The UserIdentity event is generated when a user logs in to a host. It conveys important security-related characteristics associated with a user to the CrowdStrike cloud, such as the user name. It’s normally generated once per security principal, and is thus not on its own a sign of a suspicious activity. Available for Mac & Windows platforms.
referenceURL: https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-UserIdentity
version: 0
fields:
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: event_simpleName
      required: true
      description: Event Name
      type: string
    - name: AuthenticationId
      required: true
      description: 'Values: INVALID_LUID (0), NETWORK_SERVICE (996), LOCAL_SERVICE (997), SYSTEM (999), RESERVED_LUID_MAX (1000)'
      type: int
    - name: UserPrincipal
      required: true
      description: UserPrincipal field
      type: string
    - name: UserSid
      required: true
      description: The User Security Identifier (UserSID) of the user who executed the command. A UserSID uniquely identifies a user in a system.
      type: string
    - name: AuthenticationUuid
      description: AuthenticationUUID field
      type: string
    - name: AuthenticationUuidAsString
      description: AuthenticationUUIDAsString field
      type: string
    - name: UID
      description: The User ID.
      type: bigint
    - name: UserName
      description: UserName field
      type: string
      indicators:
        - username
    - name: UserCanonical
      description: UserCanonical field
      type: string
    - name: LogonId
      description: LogonID field
      type: string
    - name: LogonDomain
      description: LogonDomain field
      type: string
    - name: AuthenticationPackage
      description: AuthenticationPackage field
      type: string
    - name: LogonType
      description: 'Values: INTERACTIVE (2), NETWORK (3), BATCH (4), SERVICE (5), PROXY (6), UNLOCK (7), NETWORK_CLEARTEXT (8), CACHED_UNLOCK (13), NEW_CREDENTIALS (9), REMOTE_INTERACTIVE (10), CACHED_INTERACTIVE (11), CACHED_REMOTE_INTERACTIVE (12)'
      type: int
    - name: LogonTime
      description: LogonTime field
      type: timestamp
      timeFormat: unix
    - name: LogonServer
      description: LogonServer field
      type: string
    - name: UserFlags
      description: 'Values: LOGON_OPTIMIZED (0x4000), LOGON_WINLOGON (0x8000), LOGON_PKINIT (0x10000), LOGON_NOT_OPTIMIZED (0x20000)'
      type: bigint
    - name: PasswordLastSet
      description: PasswordLastSet field
      type: timestamp
      timeFormat: unix
    - name: RemoteAccount
      description: RemoteAccount field
      type: int
    - name: UserIsAdmin
      description: UserIsAdmin field
      type: int
    - name: SessionId
      description: SessionID field
      type: string
      indicators:
        - trace_id
    - name: UserLogonFlags
      description: 'Values: LOGON_IS_SYNTHETIC (0x00000001), USER_IS_ADMIN (0x00000002), USER_IS_LOCAL (0x00000004), USER_IS_BUILT_IN (0x00000008), USER_IDENTITY_MISSING (0x00000010)'
      type: int
```

### Crowdstrike.UserInfo

User Account & Logon information provided by Falcon Discover.

Reference: [CrowdStrike Documentation on Falcon Data Replicator UserInfo.](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-userinfo)

```yaml
schema: Crowdstrike.UserInfo
parser:
    native:
        name: Crowdstrike.UserInfo
description: User Account & Logon information provided by Falcon Discover
referenceURL: https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-userinfo
version: 0
fields:
    - name: _time
      required: true
      description: The host's local time in epoch format.
      type: timestamp
      timeFormat: unix
      isEventTime: true
    - name: cid
      required: true
      description: The customer ID.
      type: string
      indicators:
        - md5
        - trace_id
    - name: AccountType
      required: true
      description: 'The type of account set for the user: ''Domain User'', ''Domain Administrator'', ''Local User''.'
      type: string
    - name: DomainUser
      required: true
      description: 'Indicates if the user''s credentials are part of a domain controller: ''Yes'', ''No''.'
      type: string
    - name: UserName
      required: true
      description: The username of the system.
      type: string
      indicators:
        - username
    - name: UserSid_readable
      required: true
      description: The user SID associated with this process.
      type: string
    - name: LastLoggedOnHost
      description: The host that was last logged into the system.
      type: string
    - name: LocalAdminAccess
      description: 'Indicates whether a local user is an admin: ''Yes'', ''No''.'
      type: string
    - name: LoggedOnHostCount
      description: The number of hosts logged in at _time.
      type: int
    - name: LogonInfo
      description: The login information.
      type: string
    - name: LogonTime
      description: The last login time by this user in epoch format.
      type: timestamp
      timeFormat: unix
    - name: LogonType
      description: 'Values defined as follows, INTERACTIVE: The security principal is logging on interactively, NETWORK: The security principal is logging on using a network, TERMINAL SERVER: The security principal has logged in via a terminal server.'
      type: string
    - name: monthsincereset
      description: The number of months since this user's password was last reset.
      type: int
    - name: PasswordLastSet
      description: The last time in epoch format that this user's password in the system was set.
      type: timestamp
      timeFormat: unix
    - name: User
      description: A system username with domain.
      type: string
    - name: UserIsAdmin
      description: Indicates whether the user account has administrator privileges.
      type: smallint
    - name: UserLogonFlags_decimal
      description: A bitfield for various bits of a UserLogon, or failed user logon.
      type: int
```

### Crowdstrike.UserLogonLogoff

Contains the UserLogon and UserLogoff events.

Reference: [CrowdStrike Documentation on User Logon Logoff.](https://falcon.us-2.crowdstrike.com/login/?next=%2Fdocumentation%2F26%2Fevents-data-dictionary)

```yaml
schema: Crowdstrike.UserLogonLogoff
parser:
    native:
        name: Crowdstrike.UserLogonLogoff
description: Contains the UserLogon and UserLogoff events
referenceURL: https://falcon.us-2.crowdstrike.com/support/documentation/26/events-data-dictionary
version: 0
fields:
    - name: event_simpleName
      required: true
      description: event name
      type: string
    - name: name
      required: true
      description: The event name
      type: string
    - name: aid
      description: The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time.
      type: string
      indicators:
        - md5
        - trace_id
    - name: aip
      description: The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network.
      type: string
      indicators:
        - ip
    - name: cid
      description: CID
      type: string
      indicators:
        - md5
        - trace_id
    - name: id
      description: ID
      type: string
    - name: event_platform
      description: The platform the sensor was running on
      type: string
    - name: timestamp
      description: Timestamp when the event was received by the CrowdStrike cloud.
      type: timestamp
      timeFormat: unix_ms
      isEventTime: true
    - name: _time
      description: Timestamp when the event was received by the CrowdStrike cloud (human readable)
      type: timestamp
      timeFormat: layout=01/02/2006 15:04:05.999
    - name: ComputerName
      description: The name of the host.
      type: string
      indicators:
        - hostname
    - name: ConfigBuild
      description: Config build
      type: string
    - name: ConfigStateHash
      description: Config state hash
      type: string
    - name: Entitlements
      description: Entitlements
      type: string
    - name: TreeId
      description: If this event is part of a detection tree, the tree ID it is part of
      type: string
      indicators:
        - trace_id
    - name: TreeId_decimal
      description: If this event is part of a detection tree, the tree ID it is part of. (in decimal, non-hex format)
      type: bigint
    - name: ContextThreadId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextThreadId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: ContextTimeStamp
      description: The time at which an event occurred on the system, as seen by the sensor.
      type: timestamp
      timeFormat: unix
    - name: ContextTimeStamp_decimal
      description: The time at which an event occurred on the system, as seen by the sensor (in decimal, non-hex format).
      type: timestamp
      timeFormat: unix_ms
    - name: ContextProcessId
      description: The unique ID of a process that was spawned by another process.
      type: string
    - name: ContextProcessId_decimal
      description: The unique ID of a process that was spawned by another process (in decimal, non-hex format).
      type: bigint
    - name: InContext
      description: In context (N/A on iOS)
      type: string
    - name: UserIsAdmin
      description: Indicates whether the user account has administrator privileges
      type: int
    - name: UserLogonFlags
      description: 'Values: LOGON_IS_SYNTHETIC (0x00000001), USER_IS_ADMIN (0x00000002), USER_IS_LOCAL (0x00000004), USER_IS_BUILT_IN (0x00000008), USER_IDENTITY_MISSING (0x00000010)'
      type: bigint
    - name: UserName
      description: The username
      type: string
      indicators:
        - username
    - name: UserPrincipal
      description: The user principal
      type: string
    - name: UserSid
      description: The User Security Identifier (UserSID) of the user who executed the command. A UserSID uniquely identifies a user in a system
      type: string
    - name: LogonTime
      description: The logon time
      type: timestamp
      timeFormat: unix
    - name: LogonType
      description: 'Values: INTERACTIVE (2) NETWORK (3) BATCH (4) SERVICE (5) PROXY (6) UNLOCK (7) NETWORK_CLEARTEXT (8) CACHED_UNLOCK (13) REMOTE_INTERACTIVE (10) NEW_CREDENTIALS (9) CACHED_INTERACTIVE (11) CACHED_REMOTE_INTERACTIVE (12)'
      type: bigint
    - name: PasswordLastSet
      description: The time the password was last set
      type: timestamp
      timeFormat: unix
    - name: RawProcessId
      description: The operating system’s internal PID. For matching, use the UPID fields which guarantee a unique process identifier
      type: bigint
    - name: UID
      description: The User ID
      type: bigint
    - name: UserGroupsBitmask
      description: The user group bitmask
      type: bigint
    - name: EffectiveTransmissionClass
      description: The user principal
      type: bigint
    - name: AuthenticationId
      description: The authentication identifier
      type: string
    - name: LogoffTime
      description: The logoff time
      type: timestamp
      timeFormat: unix
    - name: UserLogoffType
      description: 'Values: LOGOFF_EVENT_SOURCE (0x01) LOGOFF_PROFILE_UNLOAD (0x02) ETW (0x03) SYNTHETIC (0x04)'
      type: bigint
```

