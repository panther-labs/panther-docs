# Microsoft365

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Microsoft365.Audit.AzureActiveDirectory

Azure Active Directory audit events. Reference: [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Id`** | `string` | Unique identifier of an audit record. |
| **`RecordType`** | `int` | The type of operation indicated by the record. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#auditlogrecordtype](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) for details on the types of audit log records. |
| **`CreationTime`** | `timestamp` | The date and time in Coordinated Universal Time \(UTC\) when the user performed the activity. |
| **`Operation`** | `string` | The name of the user or admin activity. |
| **`OrganizationId`** | `string` | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs. |
| **`UserType`** | `int` | The type of user that performed the operation. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#user-type](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#user-type) for details on the types of users. |
| **`UserKey`** | `string` | An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID \(PUID\) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts. |
| `Workload` | `string` | The Office 365 service where the activity occurred. |
| `ResultStatus` | `string` | Indicates whether the action \(specified in the Operation property\) was successful or not. Possible values are Succeeded, PartiallySucceeded, or Failed. For Exchange admin activity, the value is either True or False. |
| `ObjectId` | `string` | For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. |
| **`UserId`** | `string` | The UPN \(User Principal Name\) of the user who performed the action \(specified in the Operation property\) that resulted in the record being logged; for example, my\_name@my\_domain\_name. Note that records for activity performed by system accounts \(such as SHAREPOINT\system or NT AUTHORITY\SYSTEM\) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions \(such as search a SharePoint site or OneDrive account\) on behalf of a user, admin, or service. |
| `ClientIP` | `string` | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application \(for example, Office on the web apps\) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. |
| `Scope` | `string` | Was this event created by a hosted M365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to M365. |
| **`AzureActiveDirectoryEventType`** | `int` | The type of Azure AD event. |
| `ExtendedProperties` | `[{   "Name":string,   "Value":string }]` | The extended properties of the Azure AD event. |
| `ModifiedProperties` | `[string]` | This property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified property. |
| `Application` | `string` | The application that triggers the account login event, such as Office 15. |
| `Client` | `string` | Details about the client device, device OS, and device browser that was used for the of the account login event. |
| `LoginStatus` | `int` | This property is from OrgIdLogon.LoginStatus directly. The mapping of various interesting logon failures could be done by alerting algorithms. |
| `UserDomain` | `string` | The Tenant Identity Information \(TII\). |
| `Actor` | `[{   "ID":string,   "Type":int }]` | The user or service principal that performed the action. |
| `ActorContextId` | `string` | The GUID of the organization that the actor belongs to. |
| `ActorIpAddress` | `string` | The actor's IP address in IPV4 or IPV6 address format. |
| `InterSystemsId` | `string` | The GUID that track the actions across components within the Office 365 service. |
| `IntraSystemId` | `string` | The GUID that's generated by Azure Active Directory to track the action. |
| `SupportTicketId` | `string` | The customer support ticket ID for the action in "act-on-behalf-of" situations. |
| `Target` | `[{   "ID":string,   "Type":int }]` | The user that the action \(identified by the Operation property\) was performed on. |
| `TargetContextId` | `string` | The GUID of the organization that the targeted user belongs to. |
| `ApplicationId` | `string` | The GUID that represents the application that is requesting the login. The display name can be looked up via the Azure Active Directory Graph API. |
| `ErrorCode` | `string` | For failed logins \(where the value for the Operation property is UserLoginFailed\), this property contains the Azure Active Directory STS \(AADSTS\) error code. A value of 0 indicates a successful login. |
| `LogonError` | `string` | For failed logins, this property contains a user-readable description of the reason for the failed login. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Microsoft365.Audit.Exchange

Microsoft Exchange audit events. Reference: [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Id`** | `string` | Unique identifier of an audit record. |
| **`RecordType`** | `int` | The type of operation indicated by the record. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#auditlogrecordtype](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) for details on the types of audit log records. |
| **`CreationTime`** | `timestamp` | The date and time in Coordinated Universal Time \(UTC\) when the user performed the activity. |
| **`Operation`** | `string` | The name of the user or admin activity. |
| **`OrganizationId`** | `string` | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs. |
| **`UserType`** | `int` | The type of user that performed the operation. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#user-type](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#user-type) for details on the types of users. |
| **`UserKey`** | `string` | An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID \(PUID\) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts. |
| `Workload` | `string` | The Office 365 service where the activity occurred. |
| `ResultStatus` | `string` | Indicates whether the action \(specified in the Operation property\) was successful or not. Possible values are Succeeded, PartiallySucceeded, or Failed. For Exchange admin activity, the value is either True or False. |
| `ObjectId` | `string` | For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. |
| **`UserId`** | `string` | The UPN \(User Principal Name\) of the user who performed the action \(specified in the Operation property\) that resulted in the record being logged; for example, my\_name@my\_domain\_name. Note that records for activity performed by system accounts \(such as SHAREPOINT\system or NT AUTHORITY\SYSTEM\) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions \(such as search a SharePoint site or OneDrive account\) on behalf of a user, admin, or service. |
| `ClientIP` | `string` | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application \(for example, Office on the web apps\) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. |
| `Scope` | `string` | Was this event created by a hosted M365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to M365. |
| `ModifiedObjectResolvedName` | `string` | This is the user friendly name of the object that was modified by the cmdlet. This is logged only if the cmdlet modifies the object. |
| `Parameters` | `[{   "Name":string,   "Value":string }]` | The name and value for all parameters that were used with the cmdlet that is identified in the Operations property. |
| `ModifiedProperties` | `[string]` | The property is included for admin events. The property includes the name of the property that was modified, the new value of the modified property, and the previous value of the modified object. |
| **`ExternalAccess`** | `boolean` | Specifies whether the cmdlet was run by a user in your organization, by Microsoft datacenter personnel or a datacenter service account, or by a delegated administrator. The value False indicates that the cmdlet was run by someone in your organization. The value True indicates that the cmdlet was run by datacenter personnel, a datacenter service account, or a delegated administrator. |
| `OriginatingServer` | `string` | The name of the server from which the cmdlet was executed. |
| `OrganizationName` | `string` | The name of the tenant. |
| `LogonType` | `int` | Indicates the type of user who accessed the mailbox and performed the operation that was logged. |
| `InternalLogonType` | `int` | Reserved for internal use. |
| `MailboxGuid` | `string` | The Exchange GUID of the mailbox that was accessed. |
| `MailboxOwnerUPN` | `string` | The email address of the person who owns the mailbox that was accessed. |
| `MailboxOwnerSid` | `string` | The SID of the mailbox owner. |
| `MailboxOwnerMasterAccountSid` | `string` | Mailbox owner account's master account SID. |
| `LogonUserSid` | `string` | The SID of the user who performed the operation. |
| `LogonUserDisplayName` | `string` | The user-friendly name of the user who performed the operation. |
| `ClientInfoString` | `string` | Information about the email client that was used to perform the operation, such as a browser version, Outlook version, and mobile device information. |
| `ClientIPAddress` | `string` | The IP address of the device that was used when the operation was logged. The IP address is displayed in either an IPv4 or IPv6 address format. |
| `ClientMachineName` | `string` | The machine name that hosts the Outlook client. |
| `ClientProcessName` | `string` | The email client that was used to access the mailbox. |
| `ClientVersion` | `string` | The version of the email client. |
| `Folder` | `{   "Id":string,   "Path":string,   "FolderItems":string }` | The folder where a group of items is located. |
| `CrossMailboxOperations` | `boolean` | Indicates if the operation involved more than one mailbox. |
| `DestMailboxId` | `string` | Set only if the CrossMailboxOperations parameter is True. Specifies the target mailbox GUID. |
| `DestMailboxOwnerUPN` | `string` | Set only if the CrossMailboxOperations parameter is True. Specifies the UPN of the owner of the target mailbox. |
| `DestMailboxOwnerSid` | `string` | Set only if the CrossMailboxOperations parameter is True. Specifies the SID of the target mailbox. |
| `DestMailboxOwnerMasterAccountSid` | `string` | Set only if the CrossMailboxOperations parameter is True. Specifies the SID for the master account SID of the target mailbox owner. |
| `DestFolder` | `{   "Id":string,   "Path":string,   "FolderItems":string }` | The destination folder, for operations such as Move. |
| `Folders` | `[{   "Id":string,   "Path":string,   "FolderItems":string }]` | Information about the source folders involved in an operation; for example, if folders are selected and then deleted. |
| `AffectedItems` | `[{   "Id":string,   "Subject":string,   "ParentFolder":{     "Id":string,     "Path":string,     "FolderItems":string },   "Attachments":string }]` | Information about each item in the group. |
| `SendAsUserSmtp` | `string` | SMTP address of the user who is being impersonated. |
| `SendAsUserMailboxGuid` | `string` | The Exchange GUID of the mailbox that was accessed to send email as. |
| `SendOnBehalfOfUserSmtp` | `string` | SMTP address of the user on whose behalf the email is sent. |
| `SendOnBehalfOfUserMailboxGuid` | `string` | The Exchange GUID of the mailbox that was accessed to send mail on behalf of. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Microsoft365.Audit.General

General audit events not included in the other log types. Reference: [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Id`** | `string` | Unique identifier of an audit record. |
| **`RecordType`** | `int` | The type of operation indicated by the record. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#auditlogrecordtype](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) for details on the types of audit log records. |
| **`CreationTime`** | `timestamp` | The date and time in Coordinated Universal Time \(UTC\) when the user performed the activity. |
| **`Operation`** | `string` | The name of the user or admin activity. |
| **`OrganizationId`** | `string` | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs. |
| **`UserType`** | `int` | The type of user that performed the operation. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#user-type](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#user-type) for details on the types of users. |
| **`UserKey`** | `string` | An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID \(PUID\) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts. |
| `Workload` | `string` | The Office 365 service where the activity occurred. |
| `ResultStatus` | `string` | Indicates whether the action \(specified in the Operation property\) was successful or not. Possible values are Succeeded, PartiallySucceeded, or Failed. For Exchange admin activity, the value is either True or False. |
| `ObjectId` | `string` | For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. |
| **`UserId`** | `string` | The UPN \(User Principal Name\) of the user who performed the action \(specified in the Operation property\) that resulted in the record being logged; for example, my\_name@my\_domain\_name. Note that records for activity performed by system accounts \(such as SHAREPOINT\system or NT AUTHORITY\SYSTEM\) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions \(such as search a SharePoint site or OneDrive account\) on behalf of a user, admin, or service. |
| `ClientIP` | `string` | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application \(for example, Office on the web apps\) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. |
| `Scope` | `string` | Was this event created by a hosted M365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to M365. |
| `payload` | `string` | The full JSON payload of the event. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Microsoft365.Audit.SharePoint

Microsoft SharePoint audit events. Reference: [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Id`** | `string` | Unique identifier of an audit record. |
| **`RecordType`** | `int` | The type of operation indicated by the record. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#auditlogrecordtype](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) for details on the types of audit log records. |
| **`CreationTime`** | `timestamp` | The date and time in Coordinated Universal Time \(UTC\) when the user performed the activity. |
| **`Operation`** | `string` | The name of the user or admin activity. |
| **`OrganizationId`** | `string` | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs. |
| **`UserType`** | `int` | The type of user that performed the operation. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#user-type](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#user-type) for details on the types of users. |
| **`UserKey`** | `string` | An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID \(PUID\) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts. |
| `Workload` | `string` | The Office 365 service where the activity occurred. |
| `ResultStatus` | `string` | Indicates whether the action \(specified in the Operation property\) was successful or not. Possible values are Succeeded, PartiallySucceeded, or Failed. For Exchange admin activity, the value is either True or False. |
| `ObjectId` | `string` | For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. |
| **`UserId`** | `string` | The UPN \(User Principal Name\) of the user who performed the action \(specified in the Operation property\) that resulted in the record being logged; for example, my\_name@my\_domain\_name. Note that records for activity performed by system accounts \(such as SHAREPOINT\system or NT AUTHORITY\SYSTEM\) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions \(such as search a SharePoint site or OneDrive account\) on behalf of a user, admin, or service. |
| `ClientIP` | `string` | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application \(for example, Office on the web apps\) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. |
| `Scope` | `string` | Was this event created by a hosted M365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to M365. |
| `Site` | `string` | The GUID of the site where the file or folder accessed by the user is located. |
| `ItemType` | `string` | The type of object that was accessed or modified. |
| `EventSource` | `string` | Identifies that an event occurred in SharePoint. Possible values are SharePoint or ObjectModel. |
| `SourceName` | `string` | The entity that triggered the audited operation. Possible values are SharePoint or ObjectModel. |
| `UserAgent` | `string` | Information about the user's client or browser. This information is provided by the client or browser. |
| `MachineDomainInfo` | `string` | Information about device sync operations. This information is reported only if it's present in the request. |
| `MachineId` | `string` | Information about device sync operations. This information is reported only if it's present in the request. |
| `SiteUrl` | `string` | The URL of the site where the file or folder accessed by the user is located. |
| `SourceRelativeUrl` | `string` | The URL of the folder that contains the file accessed by the user. The combination of the values for the SiteURL, SourceRelativeURL, and SourceFileName parameters is the same as the value for the ObjectID property, which is the full path name for the file accessed by the user. |
| `SourceFileName` | `string` | The name of the file or folder accessed by the user. |
| `SourceFileExtension` | `string` | The file extension of the file that was accessed by the user. This property is blank if the object that was accessed is a folder. |
| `DestinationRelativeUrl` | `string` | The URL of the destination folder where a file is copied or moved. The combination of the values for SiteURL, DestinationRelativeURL, and DestinationFileName parameters is the same as the value for the ObjectID property, which is the full path name for the file that was copied. This property is displayed only for FileCopied and FileMoved events. |
| `DestinationFileName` | `string` | The name of the file that is copied or moved. This property is displayed only for FileCopied and FileMoved events. |
| `DestinationFileExtension` | `string` | The file extension of a file that is copied or moved. This property is displayed only for FileCopied and FileMoved events. |
| `UserSharedWith` | `string` | The user that a resource was shared with. |
| `SharingType` | `string` | The type of sharing permissions that were assigned to the user that the resource was shared with. This user is identified by the UserSharedWith parameter. |
| `TargetUserOrGroupName` | `string` | Stores the UPN or name of the target user or group that a resource was shared with. |
| `TargetUserOrGroupType` | `string` | Identifies whether the target user or group is a Member, Guest, Group, or Partner. |
| `EventData` | `string` | Conveys follow-up information about the sharing action that has occurred, such as adding a user to a group or granting edit permissions. |
| `CustomEvent` | `string` | Optional string for custom events. |
| `ModifiedProperties` | `[string]` | The property is included for admin events, such as adding a user as a member of a site or a site collection admin group. The property includes the name of the property that was modified \(for example, the Site Admin group\), the new value of the modified property \(such the user who was added as a site admin\), and the previous value of the modified object. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Microsoft365.DLP.All

DLP events for all workloads. Reference: [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#dlp-schema](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#dlp-schema)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`Id`** | `string` | Unique identifier of an audit record. |
| **`RecordType`** | `int` | The type of operation indicated by the record. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#auditlogrecordtype](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype) for details on the types of audit log records. |
| **`CreationTime`** | `timestamp` | The date and time in Coordinated Universal Time \(UTC\) when the user performed the activity. |
| **`Operation`** | `string` | The name of the user or admin activity. |
| **`OrganizationId`** | `string` | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs. |
| **`UserType`** | `int` | The type of user that performed the operation. See [https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema\#user-type](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-schema#user-type) for details on the types of users. |
| **`UserKey`** | `string` | An alternative ID for the user identified in the UserId property. For example, this property is populated with the passport unique ID \(PUID\) for events performed by users in SharePoint, OneDrive for Business, and Exchange. This property may also specify the same value as the UserID property for events occurring in other services and events performed by system accounts. |
| `Workload` | `string` | The Office 365 service where the activity occurred. |
| `ResultStatus` | `string` | Indicates whether the action \(specified in the Operation property\) was successful or not. Possible values are Succeeded, PartiallySucceeded, or Failed. For Exchange admin activity, the value is either True or False. |
| `ObjectId` | `string` | For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user. For Exchange admin audit logging, the name of the object that was modified by the cmdlet. |
| **`UserId`** | `string` | The UPN \(User Principal Name\) of the user who performed the action \(specified in the Operation property\) that resulted in the record being logged; for example, my\_name@my\_domain\_name. Note that records for activity performed by system accounts \(such as SHAREPOINT\system or NT AUTHORITY\SYSTEM\) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions \(such as search a SharePoint site or OneDrive account\) on behalf of a user, admin, or service. |
| `ClientIP` | `string` | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application \(for example, Office on the web apps\) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. |
| `Scope` | `string` | Was this event created by a hosted M365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to M365. |
| `SharePointMetaData` | `{   "From":string,   "itemCreationTime":timestamp,   "SiteCollectionGuid":string,   "SiteCollectionUrl":string,   "FileName":string,   "FileOwner":string,   "FilePathUrl":string,   "DocumentLastModifier":string,   "DocumentSharer":string,   "UniqueId":string,   "LastModifiedTime":timestamp }` | Describes metadata about the document in SharePoint or OneDrive for Business that contained the sensitive information. |
| `ExchangeMetaData` | `{   "MessageID":string,   "From":string,   "To":[string],   "CC":[string],   "BCC":[string],   "Subject":string,   "Sent":timestamp,   "RecipientCount":int }` | Describes metadata about the email message that contained the sensitive information. |
| `ExceptionInfo` | `string` | Identifies reasons why a policy no longer applies and/or any information about false positive and/or override noted by the end user. |
| **`PolicyDetails`** | `[{   "PolicyId":string,   "PolicyName":string,   "Rules":[{     "RuleId":string,     "RuleName":string,     "Actions":[string],     "OverriddenActions":[string],     "Severity":string,     "RuleMode":string,     "ConditionsMatched":{       "SensitiveInformation":{         "Confidence":bigint,         "Count":bigint,         "SensitiveType":string,         "SensitiveInformationDetections":{           "Detections":string,           "ResultsTruncated":boolean } },       "DocumentProperties":[{         "Name":string,         "Value":string }],       "OtherConditions":[{         "Name":string,         "Value":string }] } }] }]` | Information about 1 or more policies that triggered the DLP event. |
| **`SensitiveInfoDetectionIsIncluded`** | `boolean` | Indicates whether the event contains the value of the sensitive data type and surrounding context from the source content. Accessing sensitive data requires the "Read DLP policy events including sensitive details" permission in Azure Active Directory. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_emails` | `[string]` | Panther added field with collection of email addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

