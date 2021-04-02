# Osquery

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Osquery.Batch

Batch contains all the data included in OsQuery batch logs Reference: [https://osquery.readthedocs.io/en/stable/deployment/logging/](https://osquery.readthedocs.io/en/stable/deployment/logging/)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`calendarTime`** | `timestamp` | The time of the event \(UTC\). |
| **`counter`** | `bigint` | Counter |
| `decorations` | `{   string:string }` | Decorations |
| **`diffResults`** | `{   "added":[{     string:string }],   "removed":[{     string:string }] }` | Computed differences. |
| **`epoch`** | `bigint` | Epoch |
| **`hostname`** | `string` | Hostname |
| **`name`** | `string` | Name |
| **`unixTime`** | `bigint` | Unix epoch |
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

## Osquery.Differential

Differential contains all the data included in OsQuery differential logs Reference: [https://osquery.readthedocs.io/en/stable/deployment/logging/](https://osquery.readthedocs.io/en/stable/deployment/logging/)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`action`** | `string` | Action |
| **`calendarTime`** | `timestamp` | The time of the event \(UTC\). |
| **`columns`** | `{   string:string }` | Columns |
| `counter` | `bigint` | Counter |
| `decorations` | `{   string:string }` | Decorations |
| **`epoch`** | `bigint` | Epoch |
| **`hostIdentifier`** | `string` | HostIdentifier |
| `logType` | `string` | LogType |
| `log_type` | `string` | LogUnderscoreType |
| **`name`** | `string` | Name |
| **`unixTime`** | `bigint` | UnixTime |
| `logNumericsAsNumbers` | `boolean` | LogNumericsAsNumbers |
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

## Osquery.Snapshot

Snapshot contains all the data included in OsQuery differential logs Reference: [https://osquery.readthedocs.io/en/stable/deployment/logging/](https://osquery.readthedocs.io/en/stable/deployment/logging/)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`action`** | `string` | Action |
| **`calendarTime`** | `timestamp` | The time of the event \(UTC\). |
| **`counter`** | `bigint` | Counter |
| `decorations` | `{   string:string }` | Decorations |
| **`epoch`** | `bigint` | Epoch |
| **`hostIdentifier`** | `string` | HostIdentifier |
| **`name`** | `string` | Name |
| **`snapshot`** | `[{   string:string }]` | Snapshot |
| **`unixTime`** | `bigint` | UnixTime |
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

## Osquery.Status

Status is a diagnostic osquery log about the daemon. Reference: [https://osquery.readthedocs.io/en/stable/deployment/logging/](https://osquery.readthedocs.io/en/stable/deployment/logging/)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`calendarTime`** | `timestamp` | The time of the event \(UTC\). |
| `decorations` | `{   string:string }` | Decorations |
| **`filename`** | `string` | Filename |
| **`hostIdentifier`** | `string` | HostIdentifier |
| **`line`** | `bigint` | Line |
| `logType` | `string` | LogType |
| `log_type` | `string` | LogUnderScoreType |
| `message` | `string` | Message |
| **`severity`** | `bigint` | Severity |
| **`unixTime`** | `bigint` | UnixTime |
| **`version`** | `string` | Version |
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

