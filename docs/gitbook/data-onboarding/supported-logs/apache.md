# Apache

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Apache.AccessCombined

Apache HTTP server access logs using the 'combined' format Reference: [https://httpd.apache.org/docs/current/logs.html\#combined](https://httpd.apache.org/docs/current/logs.html#combined)

| Column | Type | Description |
| :--- | :--- | :--- |
| `remote_host_ip_address` | `string` | This is the IP address of the client \(remote host\) which made the request to the server. If HostnameLookups is set to On, then the server will try to determine the hostname and log it in place of the IP address. |
| `client_identity_rfc_1413` | `string` | The RFC 1413 identity of the client determined by identd on the clients machine. |
| `request_user` | `string` | The userid of the person requesting the document as determined by HTTP authentication. |
| `request_time` | `timestamp` | The time that the request was received. |
| `request_method` | `string` | The HTTP request method |
| `request_uri` | `string` | The HTTP request URI |
| `request_protocol` | `string` | The HTTP request protocol |
| `response_status` | `smallint` | The HTTP status of the response |
| `response_size` | `bigint` | The size of the HTTP response in bytes |
| `user_agent` | `string` | The User-Agent HTTP header |
| `referer` | `string` | The Referer HTTP header |
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

## Apache.AccessCommon

Apache HTTP server access logs using the 'common' format Reference: [https://httpd.apache.org/docs/current/logs.html\#common](https://httpd.apache.org/docs/current/logs.html#common)

| Column | Type | Description |
| :--- | :--- | :--- |
| `remote_host_ip_address` | `string` | This is the IP address of the client \(remote host\) which made the request to the server. If HostnameLookups is set to On, then the server will try to determine the hostname and log it in place of the IP address. |
| `client_identity_rfc_1413` | `string` | The RFC 1413 identity of the client determined by identd on the clients machine. |
| `request_user` | `string` | The userid of the person requesting the document as determined by HTTP authentication. |
| `request_time` | `timestamp` | The time that the request was received. |
| `request_method` | `string` | The HTTP request method |
| `request_uri` | `string` | The HTTP request URI |
| `request_protocol` | `string` | The HTTP request protocol |
| `response_status` | `smallint` | The HTTP status of the response |
| `response_size` | `bigint` | The size of the HTTP response in bytes |
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

