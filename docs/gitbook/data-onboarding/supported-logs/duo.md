# Duo

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Duo.Administrator

Duo administrator log events. Reference: [https://duo.com/docs/adminapi\#administrator-logs](https://duo.com/docs/adminapi#administrator-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`action`** | `string` | The type of change that was performed. |
| `description` | `string` | String detailing what changed, either as free-form text or serialized JSON. |
| **`isotimestamp`** | `timestamp` | ISO8601 timestamp of the event. |
| **`object`** | `string` | The object that was acted on. For example: "jsmith" \(for users\), "\(555\) 713-6275 x456" \(for phones\), or "HOTP 8-digit 123456" \(for tokens\). |
| `timestamp` | `timestamp` | Unix timestamp of the event. |
| **`username`** | `string` | The full name of the administrator who performed the action in the Duo Admin Panel. If the action was performed with the API this will be "API". Automatic actions like deletion of inactive users have "System" for the username. Changes synchronized from Directory Sync will have a username of the form \(example\) "AD Sync: name of directory". |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Duo.Authentication

Duo authentication log events\(v2\). Reference: [https://duo.com/docs/adminapi\#authentication-logs](https://duo.com/docs/adminapi#authentication-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| `access_device` | `{   "browser":string,   "browser_version":string,   "flash_version":string,   "hostname":string,   "ip":string,   "is_encryption_enabled":string,   "is_firewall_enabled":string,   "is_password_set":string,   "java_version":string,   "location":{     "city":string,     "country":string,     "state":string },   "os":string,   "os_version":string,   "security_agents":[string] }` | Browser, plugin, and operating system information for the endpoint used to access the Duo-protected resource. Values present only when the application accessed features Duo’s inline browser prompt. |
| `alias` | `string` | The username alias used to log in. No value if the user logged in with their username instead of a username alias. |
| `application` | `{   "key":string,   "name":string }` | Information about the application accessed. |
| `auth_device` | `{   "ip":string,   "location":{     "city":string,     "country":string,     "state":string },   "name":string }` | Information about the device used to approve or deny authentication. |
| `email` | `string` | The email address of the user, if known to Duo, otherwise none. |
| `event_type` | `string` | The type of activity logged. one of: "authentication" or "enrollment". |
| `factor` | `string` | The authentication factor. One of: "phone\_call", "passcode", "yubikey\_passcode", "digipass\_go\_7\_token", "hardware\_token", "duo\_mobile\_passcode", "bypass\_code", "sms\_passcode", "sms\_refresh", "duo\_push", "u2f\_token", "remembered\_device", or "trusted\_network". |
| **`isotimestamp`** | `timestamp` | ISO8601 timestamp of the event. |
| `ood_software` | `string` | If authentication was denied due to out-of-date software, shows the name of the software, i.e. "Chrome", "Flash", etc. No value if authentication was successful or authentication denial was not due to out-of-date software. |
| `reason` | `string` | Provide the reason for the authentication attempt result. If result is "SUCCESS" then one of: "allow\_unenrolled\_user", "allowed\_by\_policy", "allow\_unenrolled\_user\_on\_trusted\_network", "bypass\_user", "remembered\_device", "trusted\_location", "trusted\_network", "user\_approved", "valid\_passcode". If result is "FAILURE" then one of: "anonymous\_ip", "anomalous\_push", "could\_not\_determine\_if\_endpoint\_was\_trusted", "denied\_by\_policy", "denied\_network", "deny\_unenrolled\_user", "endpoint\_is\_not\_in\_management\_system", "endpoint\_failed\_google\_verification", "endpoint\_is\_not\_trusted", "factor\_restricted", "invalid\_management\_certificate\_collection\_state", "invalid\_device", "invalid\_passcode", "invalid\_referring\_hostname\_provided", "location\_restricted", "locked\_out", "no\_activated\_duo\_mobile\_account", "no\_disk\_encryption", "no\_duo\_certificate\_present", "touchid\_disabled", "no\_referring\_hostname\_provided", "no\_response", "no\_screen\_lock", "no\_web\_referer\_match", "out\_of\_date", "platform\_restricted", "rooted\_device", "software\_restricted", "user\_cancelled", "user\_disabled", "user\_mistake", "user\_not\_in\_permitted\_group", "user\_provided\_invalid\_certificate", or "version\_restricted". If result is "ERROR" then: "error". If result is "FRAUD" then: "user\_marked\_fraud". |
| `result` | `string` | The result of the authentication attempt. One of: "SUCCESS", "FAILURE", "ERROR", or "FRAUD". |
| `timestamp` | `timestamp` | Unix timestamp of the event. |
| **`txid`** | `string` | The transaction ID of the event. |
| `user` | `{   "groups":[string],   "key":string,   "name":string }` | Information about the authenticating user. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_emails` | `[string]` | Panther added field with collection of email addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Duo.OfflineEnrollment

Duo Authentication for Windows Logon offline enrollment events. Reference: [https://duo.com/docs/adminapi\#offline-enrollment-logs](https://duo.com/docs/adminapi#offline-enrollment-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`action`** | `string` | The offline enrollment operation. One of "o2fa\_user\_provisioned", "o2fa\_user\_deprovisioned", or "o2fa\_user\_reenrolled". |
| `description` | `string` | Information about the Duo Windows Logon client system as reported by the application. |
| **`isotimestamp`** | `timestamp` | ISO8601 timestamp of the event. |
| **`object`** | `string` | The Duo Windows Logon integration's name. |
| `timestamp` | `timestamp` | Unix timestamp of the event. |
| **`username`** | `string` | The Duo username. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Duo.Telephony

Duo telephony log events. Reference: [https://duo.com/docs/adminapi\#telephony-logs](https://duo.com/docs/adminapi#telephony-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| `context` | `string` | How this telephony event was initiated. One of: "administrator login", "authentication", "enrollment", or "verify". |
| `credits` | `int` | How many telephony credits this event cost. |
| **`isotimestamp`** | `timestamp` | ISO8601 timestamp of the event. |
| **`phone`** | `string` | The phone number that initiated this event. |
| `timestamp` | `timestamp` | Unix timestamp of the event. |
| **`type`** | `string` | The event type. Either "sms" or "phone". |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |

