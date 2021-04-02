# Crowdstrike

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Crowdstrike.AIDMaster

Sensor and Host information provided by Falcon Insight Reference: [https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide\#section-aid-master](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-aid-master)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Time`** | `timestamp` | Timestamp of when the event was received by the CrowdStrike cloud. This is not to be confused with the time the event was generated locally on the system \(the \_timeevent\). This is the timestamp of the event from the cloud's point of view. This value can be converted to any time format and can be used for calculations. |
| **`AgentLoadFlags`** | `int` | Whether the sensor loaded during or after the Windows host's boot process. Example values: 0, 1 |
| **`AgentLocalTime`** | `timestamp` | The local time for the sensor in epoch format. |
| **`AgentTimeOffset`** | `double` | The time since the last reboot in epoch format. |
| **`AgentVersion`** | `string` | The version of the sensor running on a host. |
| **`aid`** | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| **`cid`** | `string` | The customer ID. |
| **`aip`** | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `BiosManufacturer` | `string` | The manufacturer of the host's BIOS. |
| `BiosVersion` | `string` | The version of the host's BIOS. |
| `ChassisType` | `string` | Type of system chassis, as defined in SMBIOS Standard. |
| `City` | `string` | The system's city of origin. |
| `Country` | `string` | The system's country of origin. |
| `Continent` | `string` | The sensor's continent, as seen from the CrowdStrike cloud. |
| `ComputerName` | `string` | The name of the host. |
| `ConfigIDBuild` | `string` | Build number used as part of the ConfigID. |
| `event_platform` | `string` | The platform the sensor is running on. Example values: 'Win', 'Lin', 'Mac'. |
| `FirstSeen` | `timestamp` | The first time the sensor was seen by the CrowdStrike cloud in epoch format. |
| `MachineDomain` | `string` | The Windows domain name to which the host is currently joined. |
| `OU` | `string` | The organizational unit of the host as seen by the sensor \(defined by system admin\). |
| `PointerSize` | `string` | The processor architecture \(in decimal, non-hex format\): '4' for 32-bit, '8' for 64-bit, or 'none' for unknown. |
| `ProductType` | `string` | The type of product \(in decimal, non-hex format\). Example values: '1' \(Workstation\), '2' \(Domain Controller\), '3' \(Server\). |
| `ServicePackMajor` | `string` | The major version \# of the OS Service Pack \(in decimal, non-hex format\). |
| `SiteName` | `string` | The site name of the domain to which the host is joined \(defined by system admin\). |
| `SystemManufacturer` | `string` | The host's system manufacturer. |
| `SystemProductName` | `string` | The host's product name. |
| `Timezone` | `string` | The sensor's time zone, as seen from the CrowdStrike cloud. |
| `Version` | `string` | The host's system version. |
| `HostHiddenStatus` | `string` | Whether the host is visible or not. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |

## Crowdstrike.ActivityAudit

Contains activity audit information Reference: [https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events\#section-authentication](https://developer.crowdstrike.com/crowdstrike/docs/streaming-api-events#section-authentication)

| Column | Type | Description |
| :--- | :--- | :--- |
| `AgentIdString` | `string` | The Agent ID |
| `cid` | `string` | The customer ID. A 32-character \(hex\) identifier in the CrowdStrike cloud. |
| **`ExternalApiType`** | `string` | The external API type |
| `Nonce` | `bigint` | The nonce |
| `ServiceName` | `string` | The service name |
| `UserId` | `string` | User that performed the operation, e.g. person that performed the operation to create a new user account. |
| `UserIp` | `string` | IP address of user that performs the operation. |
| `CustomerIdString` | `string` | Unique ID assigned by CS for each customer. |
| **`EventType`** | `string` | Will be Event\_ExternalApiEvent |
| `OperationName` | `string` | The operation name |
| `UTCTimestamp` | `timestamp` | The timestamp |
| **`timestamp`** | `timestamp` | The timestamp |
| `AuditKeyValues` | `[{   "Key":string,   "ValueString":string }]` | The AuditKeyValues |
| `eid` | `bigint` | The EID |
| `Success` | `boolean` | If the operation was successful or not |
| `EventUUID` | `string` | The EventUUID |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_emails` | `[string]` | Panther added field with collection of email addresses associated with the row |

## Crowdstrike.AppInfo

Detected Application Information provided by Falcon Discover Reference: [https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide\#section-appinfo](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-appinfo)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`_time`** | `timestamp` | The host's local time in epoch format. |
| **`cid`** | `string` | The customer ID. |
| **`CompanyName`** | `string` | The name of the company. |
| **`detectioncount`** | `bigint` | The number of detections. |
| **`FileName`** | `string` | The name of the file. |
| **`SHA256HashData`** | `string` | The file hash bashed on SHA-256. |
| `FileDescription` | `string` | The description of the file, if any. |
| `FileVersion` | `string` | The version of the file. |
| `ProductName` | `string` | The name of the product. |
| `ProductVersion` | `string` | The version of the product. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_sha256_hashes` | `[string]` | Panther added field with collection of MD5 hashes associated with the row |

## Crowdstrike.DNSRequest

This event is generated for every attempted DNS name resolution on a host.

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | Event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| `EffectiveTransmissionClass` | `bigint` | Effective transmission class |
| `DomainName` | `string` | The domain name requested |
| `InterfaceIndex` | `bigint` | The network interface index \(Windows only\) |
| `DualRequest` | `bigint` | If the event is dual request \(Windows only\) |
| `DnsRequestCount` | `bigint` | The number of DNS requests \(Windows only\) |
| `AppIdentifier` | `string` | The identifier of the app that made the request \(Android, iOS\) |
| `IpAddress` | `string` | The device ip address \(Android, iOS\) |
| `RequestType` | `string` | The DNS request type |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.GroupIdentity

Provides the sensor boot unique mapping between GID, AuthenticationId, UserPrincipal, and UserSid. Available only for the Mac platform. Reference: [https://developer.crowdstrike.com/crowdstrike/page/event-explorer\#section-event-GroupIdentity](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-GroupIdentity)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| **`event_simpleName`** | `string` | Event Name |
| **`GID`** | `bigint` | The user Group ID. |
| **`AuthenticationUuid`** | `string` |  |
| **`AuthenticationUuidAsString`** | `string` |  |
| **`AuthenticationId`** | `int` | Values: INVALID\_LUID \(0\), NETWORK\_SERVICE \(996\), LOCAL\_SERVICE \(997\), SYSTEM \(999\), RESERVED\_LUID\_MAX \(1000\) |
| **`UserPrincipal`** | `string` |  |
| **`UserSid`** | `string` | The User Security Identifier \(UserSID\) of the user who executed the command. A UserSID uniquely identifies a user in a system. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.ManagedAssets

Sensor and Host information provided by Falcon Insight \(Network Information: IP Address, LAN/Ethernet Interface, Gateway Address, MAC Address\) Reference: [https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide\#section-managedassets](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-managedassets)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`_time`** | `timestamp` | The host's local time in epoch format. |
| **`aid`** | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| **`cid`** | `string` | The customer ID. |
| **`GatewayIP`** | `string` | The gateway of the system where the sensor is installed. |
| **`GatewayMAC`** | `string` | The MAC address of the gateway. |
| **`MacPrefix`** | `string` | An identifier unique to the organization. |
| **`MAC`** | `string` | The MAC address of the system. |
| **`LocalAddressIP4`** | `string` | The device's local IP address in IPv4 format. |
| `InterfaceAlias` | `string` | The user-friendly name of the IP interface. |
| `InterfaceDescription` | `string` | The network adapter used for the IP interface. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |

## Crowdstrike.NetworkConnect

This event is generated when an application attempts a remote connection on an interface

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | Event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| `LocalAddressIP4` | `string` | Local IPv4 address for the connection |
| `LocalAddressIP6` | `string` | Local IPv6 address for the connection |
| `RemoteAddressIP4` | `string` | Remote IPv4 address for the connection |
| `RemoteAddressIP6` | `string` | Remote IPv6 address for the connection |
| `ConnectionFlags` | `int` | Connection flags \(PROMISCUOUS\_MODE\_SIO\_RCVALL = 2, RAW\_SOCKET = 1, PROMISCUOUS\_MODE\_SIO\_RCVALL\_IGMPMCAST = 4, PROMISCUOUS\_MODE\_SIO\_RCVALL\_MCAST = 8\) |
| `Protocol` | `int` | IP Protocol \(ICMP = 1, TCP = 6, UDP = 17\) |
| `LocalPort` | `int` | Connection local port |
| `RemotePort` | `int` | Connection remote port |
| `ConnectionDirection` | `int` | Direction of the connection \(OUTBOUND = 0, INBOUND = 1, NEITHER = 2, BOTH = 3\) |
| `IcmpType` | `string` | ICMP type \(N/A on iOS\) |
| `IcmpCode` | `string` | ICMP code \(N/A on iOS\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.NetworkListen

This event is generated when an application establishes a socket in listening mode

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| `LocalAddressIP4` | `string` | Local IPv4 address for the connection |
| `LocalAddressIP6` | `string` | Local IPv6 address for the connection |
| `RemoteAddressIP4` | `string` | Remote IPv4 address for the connection |
| `RemoteAddressIP6` | `string` | Remote IPv6 address for the connection |
| `ConnectionFlags` | `int` | Connection flags \(PROMISCUOUS\_MODE\_SIO\_RCVALL = 2, RAW\_SOCKET = 1, PROMISCUOUS\_MODE\_SIO\_RCVALL\_IGMPMCAST = 4, PROMISCUOUS\_MODE\_SIO\_RCVALL\_MCAST = 8\) |
| `Protocol` | `int` | IP Protocol \(ICMP = 1, TCP = 6, UDP = 17\) |
| `LocalPort` | `int` | Connection local port |
| `RemotePort` | `int` | Connection remote port |
| `ConnectionDirection` | `int` | Direction of the connection \(OUTBOUND = 0, INBOUND = 1, NEITHER = 2, BOTH = 3\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.NotManagedAssets

Unmanaged Host discovery information provided by Falcon Insight Reference: [https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide\#section-notmanaged](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-notmanaged)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`_time`** | `timestamp` | The host's local time in epoch format. |
| **`aip`** | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| **`aipcount`** | `bigint` | The number of public-facing IP addresses. |
| **`localipCount`** | `bigint` | The number of local IP addresses. |
| **`cid`** | `string` | The customer ID. |
| **`CurrentLocalIP`** | `string` | The current local IP address of the machine, found via the IPv4 network discovery protocol. |
| `subnet` | `string` | The subnet of the system. |
| **`MAC`** | `string` | The MAC address of the system. |
| **`MacPrefix`** | `string` | An identifier unique to the organization. |
| **`discovererCount`** | `bigint` | The number of aid's that have discovered this system. |
| `discoverer_aid` | `[string]` | The agent IDs that have discovered this system. |
| `discoverer_devicetype` | `string` | The type of device that discovered this system \('VM' or 'Server'\). |
| `FirstDiscoveredDate` | `timestamp` | The first time the system was discovered in epoch format. |
| `LastDiscoveredBy` | `timestamp` | The most recent time the system was discovered in epoch format. |
| `LocalAddressIP4` | `string` | The device's local IP address in IPv4 format. |
| `ComputerName` | `string` | The name of the host that discovered the neighbor. |
| `NeighborName` | `string` | The neighbor's host name. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |

## Crowdstrike.ProcessRollup2

This event \(often called "PR2" for short\) is generated for a process that is running or has finished running on a host and contains information about that process.

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | Event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `TargetProcessId` | `bigint` | The unique ID of a target process |
| `SourceProcessId` | `bigint` | The unique ID of creating process. |
| `SourceThreadId` | `bigint` | The unique ID of thread from creating process. |
| `ParentProcessId` | `bigint` | The unique ID of the parent process. |
| `ImageFileName` | `string` | The full path to an executable \(PE\) file. The context of this field provides more information as to its meaning. For ProcessRollup2 events, this is the full path to the main executable for the created process |
| `CommandLine` | `string` | The command line used to create this process. May be empty in some circumstances |
| `RawProcessId` | `bigint` | The operating system’s internal PID. For matching, use the UPID fields which guarantee a unique process identifier |
| `ProcessStartTime` | `timestamp` | The time the process began in UNIX epoch time \(in decimal, non-hex format\). |
| `ProcessEndTime` | `timestamp` | The time the process finished \(in decimal, non-hex format\). |
| `SHA256HashData` | `string` | The SHA256 hash of a file. In most cases, the hash of the file referred to by the ImageFileName field. |
| `SHA1HashData` | `string` | The SHA1 hash of a file |
| `MD5HashData` | `string` | The MD5 hash of a file |
| `ImageSubsystem` | `string` | Subsystem of the image filename \(Windows only\) |
| `UserSid` | `string` | The User Security Identifier \(UserSID\) of the user who executed the command. A UserSID uniquely identifies a user in a system. \(Windows only\) |
| `AuthenticationId` | `string` | The authentication identifier \(Windows only\) |
| `IntegrityLevel` | `string` | The integrity level \(Windows only\) |
| `ProcessCreateFlags` | `string` | Captured flags from original process create. This is a bitfield. \(Windows only\) |
| `ProcessParameterFlags` | `string` | Flags from the ‘NtCreateUserProcess’ API. This bitfield includes data like if DLL redirection is enabled. \(Windows only\) |
| `ProcessSxsFlags` | `string` | Flags from the communications path with the Windows Subsystem Process. This bitfield includes data like if there’s a manifest and if it’s local or not. \(Windows only\) |
| `ParentAuthenticationId` | `string` | The authentication identifier for the parent process \(Windows only\) |
| `TokenType` | `string` | The token type \(Windows only\) |
| `SessionId` | `string` | The id of the session \(Windows only\) |
| `WindowFlags` | `string` | Flags from the window \(Windows only\) |
| `ShowWindowFlags` | `string` | Window visibility flags \(Windows only\) |
| `WindowStartingPositionHorizontal` | `bigint` | Start horizontal position of the process window \(Windows only\) |
| `WindowStartingPositionVertical` | `bigint` | Start vertical position of the process window \(Windows only\) |
| `WindowStartingWidth` | `bigint` | Start width of the process window \(Windows only\) |
| `WindowStartingHeight` | `bigint` | Start height of the process window \(Windows only\) |
| `Desktop` | `string` | The desktop of the process window \(Windows only\) |
| `WindowStation` | `string` | The process window station \(Windows only\) |
| `WindowTitle` | `string` | The title of the process window \(WindowsOnly\) |
| `LinkName` | `string` | Link name \(Windows only\) |
| `ApplicationUserModelId` | `string` | Application user model id \(WindowsOnly\) |
| `CallStackModuleNames` | `string` | Call stack module names \(Windows only\) |
| `CallStackModuleNamesVersion` | `string` | Call stack module names version \(Windows only\) |
| `RpcClientProcessId` | `string` | RPC client process id \(Windows only\) |
| `CsaProcessDataCollectionInstanceId` | `string` | CSA process data collection instance id \(Windows only\) |
| `OriginalCommandLine` | `string` | The original command line used to create this process \(Windows only\) |
| `CreateProcessType` | `string` | Create process type \(Windows only\) |
| `ZoneIdentifier` | `string` | Zone identifier \(Windows only\) |
| `HostUrl` | `string` | Host URL \(Windows only\) |
| `ReferrerUrl` | `string` | Referrer URL \(Windows only\) |
| `GrandParent` | `string` | Grant parent \(Windows only\) |
| `BaseFileName` | `string` | Base file name \(Windows only\) |
| `Tags` | `string` | Process tags comma separated list \(Windows, Mac\) |
| `ParentBaseFileName` | `string` | Parent process base file name \(Windows, Mac\) |
| `ProcessGroupId` | `bigint` | Process group id \(Windows, Mac\) |
| `UID` | `bigint` | UID \(Mac, Linux, Android\) |
| `RUID` | `bigint` | RUID \(Mac, Linux, Android\) |
| `SVUID` | `bigint` | SVUID \(Mac, Linux, Android\) |
| `GID` | `bigint` | GID \(Mac, Linux, Android\) |
| `RGID` | `bigint` | RGID \(Mac, Linux, Android\) |
| `SVGID` | `bigint` | SVGID \(Mac, Linux, Android\) |
| `SessionProcessId` | `bigint` | Session process id \(Mac, Linux\) |
| `MachOSubType` | `string` | MachOSubType \(Mac only\) |
| `TtyName` | `string` | TTY name \(Linux only\) |
| `OciContainerId` | `string` | OCI Container id \(Linux only\) |
| `SourceAndroidComponentName` | `string` | Source component name \(Android only\) |
| `TargetAndroidComponentName` | `string` | Target component name \(Android only\) |
| `TargetAndroidComponentType` | `string` | Target component type \(Android only\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_md5_hashes` | `[string]` | Panther added field with collection of SHA256 hashes of any algorithm associated with the row |
| `p_any_sha1_hashes` | `[string]` | Panther added field with collection of SHA1 hashes associated with the row |
| `p_any_sha256_hashes` | `[string]` | Panther added field with collection of MD5 hashes associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.SyntheticProcessRollup2

A synthetic version of the process rollup \(PR2\) event

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| `TargetProcessId` | `bigint` | The unique ID of a target process |
| `SourceProcessId` | `bigint` | The unique ID of creating process. |
| `SourceThreadId` | `bigint` | The unique ID of thread from creating process. |
| `ParentProcessId` | `bigint` | The unique ID of the parent process. |
| `ImageFileName` | `string` | The full path to an executable \(PE\) file. The context of this field provides more information as to its meaning. For ProcessRollup2 events, this is the full path to the main executable for the created process |
| `CommandLine` | `string` | The command line used to create this process. May be empty in some circumstances |
| `RawProcessId` | `bigint` | The operating system’s internal PID. For matching, use the UPID fields which guarantee a unique process identifier |
| `ProcessStartTime` | `timestamp` | The time the process began in UNIX epoch time \(in decimal, non-hex format\). |
| `ProcessEndTime` | `timestamp` | The time the process finished \(in decimal, non-hex format\). |
| `SHA256HashData` | `string` | The SHA256 hash of a file. In most cases, the hash of the file referred to by the ImageFileName field. |
| `SHA1HashData` | `string` | The SHA1 hash of a file |
| `MD5HashData` | `string` | The MD5 hash of a file |
| `SyntheticPR2Flags` | `int` | PR2 flags \(PROCESS\_RUNDOWN = 0, PROCESS\_HOLLOWED = 1, IMAGEHASH\_FAILURE = 4, FILE\_PATH\_EXCLUDED = 8, PROCESS\_FORK\_FOLDING = 16, APP\_MONITORING = 2\) |
| `ImageSubsystem` | `string` | Subsystem of the image filename \(Windows only\) |
| `UserSid` | `string` | The User Security Identifier \(UserSID\) of the user who executed the command. A UserSID uniquely identifies a user in a system. \(Windows only\) |
| `AuthenticationId` | `string` | The authentication identifier \(Windows only\) |
| `IntegrityLevel` | `string` | The integrity level \(Windows only\) |
| `ProcessGroupId` | `bigint` | Process group id \(Mac\) |
| `UID` | `bigint` | UID \(Mac\) |
| `RUID` | `bigint` | RUID \(Mac\) |
| `SVUID` | `bigint` | SVUID \(Mac\) |
| `GID` | `bigint` | GID \(Mac\) |
| `RGID` | `bigint` | RGID \(Mac\) |
| `SVGID` | `bigint` | SVGID \(Mac\) |
| `SessionProcessId` | `bigint` | Session process id \(Mac\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_md5_hashes` | `[string]` | Panther added field with collection of SHA256 hashes of any algorithm associated with the row |
| `p_any_sha1_hashes` | `[string]` | Panther added field with collection of SHA1 hashes associated with the row |
| `p_any_sha256_hashes` | `[string]` | Panther added field with collection of MD5 hashes associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.Unknown

This event is used to store all unknown crowdstrike log events

| Column | Type | Description |
| :--- | :--- | :--- |
| **`event_simpleName`** | `string` | Event name |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| **`unknown_payload`** | `string` | The full JSON payload of the event |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Crowdstrike.UserIdentity

The UserIdentity event is generated when a user logs in to a host. It conveys important security-related characteristics associated with a user to the CrowdStrike cloud, such as the user name. It’s normally generated once per security principal, and is thus not on its own a sign of a suspicious activity. Available for Mac & Windows platforms. Reference: [https://developer.crowdstrike.com/crowdstrike/page/event-explorer\#section-event-UserIdentity](https://developer.crowdstrike.com/crowdstrike/page/event-explorer#section-event-UserIdentity)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`name`** | `string` | The event name |
| `aid` | `string` | The sensor ID. This value is unique to each installation of a Falcon sensor. When a sensor is updated or reinstalled, the host gets a new aid. In those situations, a single host could have multiple aid values over time. |
| `aip` | `string` | The sensor’s IP, as seen from the CrowdStrike cloud. This is typically the public IP of the sensor. This helps determine the location of a computer, depending on your network. |
| `cid` | `string` | CID |
| `id` | `string` | ID |
| `event_platform` | `string` | The platform the sensor was running on |
| `timestamp` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud. |
| `_time` | `timestamp` | Timestamp when the event was received by the CrowdStrike cloud \(human readable\) |
| `ComputerName` | `string` | The name of the host. |
| `ConfigBuild` | `string` | Config build |
| `ConfigStateHash` | `string` | Config state hash |
| `Entitlements` | `string` | Entitlements |
| `TreeId` | `string` | If this event is part of a detection tree, the tree ID it is part of |
| `TreeId_decimal` | `bigint` | If this event is part of a detection tree, the tree ID it is part of. \(in decimal, non-hex format\) |
| `ContextThreadId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextThreadId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `ContextTimeStamp` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor. |
| `ContextTimeStamp_decimal` | `timestamp` | The time at which an event occurred on the system, as seen by the sensor \(in decimal, non-hex format\). |
| `ContextProcessId` | `string` | The unique ID of a process that was spawned by another process. |
| `ContextProcessId_decimal` | `bigint` | The unique ID of a process that was spawned by another process \(in decimal, non-hex format\). |
| `InContext` | `string` | In context \(N/A on iOS\) |
| **`event_simpleName`** | `string` | Event Name |
| **`AuthenticationId`** | `int` | Values: INVALID\_LUID \(0\), NETWORK\_SERVICE \(996\), LOCAL\_SERVICE \(997\), SYSTEM \(999\), RESERVED\_LUID\_MAX \(1000\) |
| **`UserPrincipal`** | `string` |  |
| **`UserSid`** | `string` | The User Security Identifier \(UserSID\) of the user who executed the command. A UserSID uniquely identifies a user in a system. |
| `AuthenticationUuid` | `string` |  |
| `AuthenticationUuidAsString` | `string` |  |
| `UID` | `bigint` | The User ID. |
| `UserName` | `string` |  |
| `UserCanonical` | `string` |  |
| `LogonId` | `string` |  |
| `LogonDomain` | `string` |  |
| `AuthenticationPackage` | `string` |  |
| `LogonType` | `int` | Values: INTERACTIVE \(2\), NETWORK \(3\), BATCH \(4\), SERVICE \(5\), PROXY \(6\), UNLOCK \(7\), NETWORK\_CLEARTEXT \(8\), CACHED\_UNLOCK \(13\), NEW\_CREDENTIALS \(9\), REMOTE\_INTERACTIVE \(10\), CACHED\_INTERACTIVE \(11\), CACHED\_REMOTE\_INTERACTIVE \(12\) |
| `LogonTime` | `timestamp` |  |
| `LogonServer` | `string` |  |
| `UserFlags` | `bigint` | Values: LOGON\_OPTIMIZED \(0x4000\), LOGON\_WINLOGON \(0x8000\), LOGON\_PKINIT \(0x10000\), LOGON\_NOT\_OPTIMIZED \(0x20000\) |
| `PasswordLastSet` | `timestamp` |  |
| `RemoteAccount` | `int` |  |
| `UserIsAdmin` | `int` |  |
| `SessionId` | `string` |  |
| `UserLogonFlags` | `int` | Values: LOGON\_IS\_SYNTHETIC \(0x00000001\), USER\_IS\_ADMIN \(0x00000002\), USER\_IS\_LOCAL \(0x00000004\), USER\_IS\_BUILT\_IN \(0x00000008\), USER\_IDENTITY\_MISSING \(0x00000010\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Crowdstrike.UserInfo

User Account & Logon information provided by Falcon Discover Reference: [https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide\#section-userinfo](https://developer.crowdstrike.com/crowdstrike/docs/falcon-data-replicator-guide#section-userinfo)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`_time`** | `timestamp` | The host's local time in epoch format. |
| **`cid`** | `string` | The customer ID. |
| **`AccountType`** | `string` | The type of account set for the user: 'Domain User', 'Domain Administrator', 'Local User'. |
| **`DomainUser`** | `string` | Indicates if the user's credentials are part of a domain controller: 'Yes', 'No'. |
| **`UserName`** | `string` | The username of the system. |
| **`UserSid_readable`** | `string` | The user SID associated with this process. |
| `LastLoggedOnHost` | `string` | The host that was last logged into the system. |
| `LocalAdminAccess` | `string` | Indicates whether a local user is an admin: 'Yes', 'No'. |
| `LoggedOnHostCount` | `int` | The number of hosts logged in at \_time. |
| `LogonInfo` | `string` | The login information. |
| `LogonTime` | `timestamp` | The last login time by this user in epoch format. |
| `LogonType` | `string` | Values defined as follows, INTERACTIVE: The security principal is logging on interactively, NETWORK: The security principal is logging on using a network, TERMINAL SERVER: The security principal has logged in via a terminal server. |
| `monthsincereset` | `int` | The number of months since this user's password was last reset. |
| `PasswordLastSet` | `timestamp` | The last time in epoch format that this user's password in the system was set. |
| `User` | `string` | A system username with domain. |
| `UserIsAdmin` | `tinyint` | Indicates whether the user account has administrator privileges. |
| `UserLogonFlags_decimal` | `int` | A bitfield for various bits of a UserLogon, or failed user logon. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

