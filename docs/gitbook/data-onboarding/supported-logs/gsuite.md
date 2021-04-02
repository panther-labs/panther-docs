# GSuite

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## GSuite.Reports

Contains the activity events for a specific account and application such as the Admin console application or the Google Drive application. Reference: [https://developers.google.com/admin-sdk/reports/v1/reference/activities/list\#response](https://developers.google.com/admin-sdk/reports/v1/reference/activities/list#response)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`id`** | `{   "applicationName":string,   "customerId":string,   "time":timestamp,   "uniqueQualifier":string }` | Unique identifier for each activity record. |
| `actor` | `{   "email":string,   "profileId":string,   "callerType":string,   "key":string }` | User doing the action. |
| **`kind`** | `string` | The type of API resource. For an activity report, the value is reports\#activities. |
| `ownerDomain` | `string` | This is the domain that is affected by the report's event. For example domain of Admin console or the Drive application's document owner. |
| `ipAddress` | `string` | IP address of the user doing the action. This is the Internet Protocol \(IP\) address of the user when logging into G Suite which may or may not reflect the user's physical location. For example, the IP address can be the user's proxy server's address or a virtual private network \(VPN\) address. The API supports IPv4 and IPv6. |
| `events` | `[{   "type":string,   "name":string,   "parameters":[{     "name":string,     "value":string,     "intValue":bigint,     "boolValue":boolean,     "multiValue":[string],     "multiIntValue":[bigint],     "messageValue":string,     "multiMessageValue":[string] }] }]` | Activity events in the report. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_emails` | `[string]` | Panther added field with collection of email addresses associated with the row |

