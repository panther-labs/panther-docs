# Box

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Box.Event

Contains events for the entire enterprise Reference: [https://developer.box.com/reference/get-events](https://developer.box.com/reference/get-events)

| Column | Type | Description |
| :--- | :--- | :--- |
| `additional_details` | `string` | This object provides additional information about the event if available. |
| `created_at` | `timestamp` | The timestamp of the event |
| `created_by` | `{   "id":string,   "type":string,   "login":string,   "name":string }` | The user that performed the action represented by the event. |
| **`event_id`** | `string` | The ID of the event object. You can use this to detect duplicate events |
| **`event_type`** | `string` | The event type that triggered this event |
| **`type`** | `string` | The object type \(always 'event'\) |
| **`source`** | `{   "id":string,   "type":string,   "login":string,   "name":string,   "item_id":string,   "item_name":string,   "item_type":string,   "owned_by":{     "id":string,     "type":string,     "login":string,     "name":string },   "parent":{     "etag":string,     "id":string,     "type":string,     "name":string,     "sequence_id":string } }` | The item that triggered this event |
| `session_id` | `string` | The event type that triggered this event |
| `ip_address` | `string` | The IP address the request was made from. |
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

