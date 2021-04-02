# Suricata

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Suricata.Anomaly

Suricata parser for the Anomaly event type in the EVE JSON output. Reference: [https://suricata.readthedocs.io/en/suricata-5.0.2/output/eve/eve-json-output.html\#anomaly](https://suricata.readthedocs.io/en/suricata-5.0.2/output/eve/eve-json-output.html#anomaly)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`anomaly`** | `{   "code":bigint,   "event":string,   "layer":string,   "type":string }` | Suricata Anomaly Anomaly |
| `app_proto` | `string` | Suricata Anomaly AppProto |
| `community_id` | `string` | Suricata Anomaly CommunityID |
| `dest_ip` | `string` | Suricata Anomaly DestIP |
| `dest_port` | `int` | Suricata Anomaly DestPort |
| **`event_type`** | `string` | Suricata Anomaly EventType |
| `flow_id` | `bigint` | Suricata Anomaly FlowID |
| `icmp_code` | `bigint` | Suricata Anomaly IcmpCode |
| `icmp_type` | `bigint` | Suricata Anomaly IcmpType |
| `metadata` | `{   "flowbits":[string],   "flowints":{     "applayer_anomaly_count":bigint,     "http_anomaly_count":bigint,     "tcp_retransmission_count":bigint,     "tls_anomaly_count":bigint } }` | Suricata Anomaly Metadata |
| `packet` | `string` | Suricata Anomaly Packet |
| `packet_info` | `{   "linktype":bigint }` | Suricata Anomaly PacketInfo |
| `pcap_cnt` | `bigint` | Suricata Anomaly PcapCnt |
| `pcap_filename` | `string` | Suricata Anomaly PcapFilename |
| `proto` | `bigint` | Suricata Anomaly Proto |
| `src_ip` | `string` | Suricata Anomaly SrcIP |
| `src_port` | `int` | Suricata Anomaly SrcPort |
| **`timestamp`** | `timestamp` | Suricata Anomaly Timestamp |
| `tx_id` | `bigint` | Suricata Anomaly TxID |
| `vlan` | `[bigint]` | Suricata Anomaly Vlan |
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

## Suricata.DNS

Suricata parser for the DNS event type in the EVE JSON output. Reference: [https://suricata.readthedocs.io/en/suricata-5.0.2/output/eve/eve-json-output.html\#dns](https://suricata.readthedocs.io/en/suricata-5.0.2/output/eve/eve-json-output.html#dns)

| Column | Type | Description |
| :--- | :--- | :--- |
| `community_id` | `string` | Suricata DNS CommunityID |
| **`dns`** | `{   "aa":boolean,   "answers":[{     "rdata":string,     "rrname":string,     "rrtype":string,     "ttl":bigint }],   "authorities":[{     "rrname":string,     "rrtype":string,     "ttl":bigint }],   "flags":string,   "grouped":{     "A":[string],     "AAAA":[string],     "CNAME":[string],     "MX":[string],     "PTR":[string],     "TXT":[string] },   "id":bigint,   "qr":boolean,   "ra":boolean,   "rcode":string,   "rd":boolean,   "rrname":string,   "rdata":string,   "rrtype":string,   "ttl":bigint,   "tx_id":bigint,   "type":string,   "version":bigint }` | Suricata DNS DNS |
| **`dest_ip`** | `string` | Suricata DNS DestIP |
| `dest_port` | `int` | Suricata DNS DestPort |
| **`event_type`** | `string` | Suricata DNS EventType |
| `flow_id` | `bigint` | Suricata DNS FlowID |
| `pcap_cnt` | `bigint` | Suricata DNS PcapCnt |
| `pcap_filename` | `string` | Suricata DNS PcapFilename |
| **`proto`** | `bigint` | Suricata DNS Proto |
| **`src_ip`** | `string` | Suricata DNS SrcIP |
| `src_port` | `int` | Suricata DNS SrcPort |
| **`timestamp`** | `timestamp` | Suricata DNS Timestamp |
| `vlan` | `[bigint]` | Suricata DNS Vlan |
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

