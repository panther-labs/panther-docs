# Okta

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Okta.SystemLog

The Okta System Log records system events related to your organization in order to provide an audit trail that can be used to understand platform activity and to diagnose problems.

Panther Enterprise Only

Reference: [https://developer.okta.com/docs/reference/api/system-log/](https://developer.okta.com/docs/reference/api/system-log/)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`uuid`** | `string` | Unique identifier for an individual event |
| **`published`** | `timestamp` | Timestamp when event was published |
| **`eventType`** | `string` | Type of event that was published |
| **`version`** | `string` | Versioning indicator |
| **`severity`** | `string` | Indicates how severe the event is: DEBUG, INFO, WARN, ERROR |
| `legacyEventType` | `string` | Associated Events API Action objectType attribute value |
| `displayMessage` | `string` | The display message for an event |
| `actor` | `{   "id":string,   "type":string,   "alternateId":string,   "displayName":string,   "details":string }` | Describes the entity that performed an action |
| `client` | `{   "id":string,   "userAgent":{     "browser":string,     "os":string,     "rawUserAgent":string },   "geographicalContext":{     "geolocation":{       "lat":double,       "lon":double },     "city":string,     "state":string,     "country":string,     "postalCode":string },   "zone":string,   "ipAddress":string,   "device":string }` | The client that requested an action |
| `request` | `{   "ipChain":[{     "ip":string,     "geographicalContext":{       "geolocation":{         "lat":double,         "lon":double },       "city":string,       "state":string,       "country":string,       "postalCode":string },     "version":string,     "source":string }] }` | The request that initiated an action |
| `outcome` | `{   "result":string,   "reason":string }` | The outcome of an action |
| `target` | `[{   "id":string,   "type":string,   "alternateId":string,   "displayName":string,   "details":string }]` | Zero or more targets of an action |
| `transaction` | `{   "id":string,   "type":string,   "detail":string }` | The transaction details of an action |
| `debugContext` | `{   "debugData":string }` | The debug request data of an action |
| `authenticationContext` | `{   "authenticatorProvider":string,   "authenticationStep":int,   "credentialProvider":string,   "credentialType":string,   "issuer":{     "id":string,     "type":string },   "externalSessionId":string,   "interface":string }` | The authentication data of an action |
| `securityContext` | `{   "asNumber":bigint,   "asOrg":string,   "isp":string,   "domain":string,   "isProxy":boolean }` | The security data of an action |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| **`p_event_time`** | `timestamp` | Panther added standardize event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardize log parse time \(UTC\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_sha1_hashes` | `[string]` | Panther added field with collection of SHA1 hashes associated with the row |
| `p_any_md5_hashes` | `[string]` | Panther added field with collection of MD5 hashes associated with the row |
| `p_any_sha256_hashes` | `[string]` | Panther added field with collection of SHA256 hashes of any algorithm associated with the row |

