# Zeek

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Zeek.DNS

Zeek DNS activity Reference: [https://docs.zeek.org/en/current/scripts/base/protocols/dns/main.zeek.html\#type-DNS::Info](https://docs.zeek.org/en/current/scripts/base/protocols/dns/main.zeek.html#type-DNS::Info)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`ts`** | `timestamp` | The earliest time at which a DNS protocol message over the associated connection is observed. |
| **`uid`** | `string` | A unique identifier of the connection over which DNS messages are being transferred. |
| **`id_orig_h`** | `string` | The originator’s IP address. |
| **`id_orig_p`** | `int` | The originator’s port number. |
| **`id_resp_h`** | `string` | The responder’s IP address. |
| **`id_resp_p`** | `int` | The responder’s port number. |
| **`proto`** | `string` | The transport layer protocol of the connection. |
| `trans_id` | `int` | A 16-bit identifier assigned by the program that generated the DNS query. Also used in responses to match up replies to outstanding queries. |
| `query` | `string` | The domain name that is the subject of the DNS query. |
| `qclass` | `bigint` | The QCLASS value specifying the class of the query. |
| `qclass_name` | `string` | A descriptive name for the class of the query. |
| `qtype` | `bigint` | A QTYPE value specifying the type of the query. |
| `qtype_name` | `string` | A descriptive name for the type of the query. |
| `rcode` | `bigint` | The response code value in DNS response messages. |
| `rcode_name` | `string` | A descriptive name for the response code value. |
| `AA` | `boolean` | The Authoritative Answer bit for response messages specifies that the responding name server is an authority for the domain name in the question section. |
| `TC` | `boolean` | The Truncation bit specifies that the message was truncated. |
| `RD` | `boolean` | The Recursion Desired bit in a request message indicates that the client wants recursive service for this query. |
| `RA` | `boolean` | The Recursion Available bit in a response message indicates that the name server supports recursive queries. |
| `Z` | `bigint` | A reserved field that is usually zero in queries and responses. |
| `answers` | `[string]` | The set of resource descriptions in the query answer. |
| `TTLs` | `[double]` | The caching intervals \(measured in seconds\) of the associated RRs described by the answers field. |
| `rejected` | `boolean` | The DNS query was rejected by the server. |
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

