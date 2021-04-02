# Cisco Umbrella

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## CiscoUmbrella.CloudFirewall

Cloud Firewall logs show traffic that has been handled by network tunnels. Reference: [https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning\#section-cloud-firewall-logs](https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning#section-cloud-firewall-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`timestamp`** | `timestamp` | The timestamp of the request transaction in UTC \(2015-01-16 17:48:41\). |
| `originId` | `string` | The unique identity of the network tunnel. |
| `identity` | `string` | The name of the network tunnel. |
| `identityType` | `string` | The type of identity that made the request. Should always be 'CDFW Tunnel Device'. |
| `direction` | `string` | The direction of the packet. It is destined either towards the internet or to the customer's network. |
| `ipProtocol` | `int` | The actual IP protocol of the traffic. It could be TCP, UDP, ICMP. |
| `packetSize` | `int` | The size of the packet that Umbrella CDFW received. |
| `sourceIp` | `string` | The internal IP address of the user-generated traffic towards the CDFW. If the traffic goes through NAT before it comes to CDFW, it will be the NAT IP address. |
| `sourcePort` | `int` | The internal port number of the user-generated traffic towards the CDFW. |
| `destinationIp` | `string` | The destination IP address of the user-generated traffic towards the CDFW. |
| `destinationPort` | `int` | The destination port number of the user-generated traffic towards the CDFW. |
| `dataCenter` | `string` | The name of the Umbrella Data Center that processed the user-generated traffic. |
| `ruleId` | `string` | The ID of the rule that processed the user traffic. |
| `verdict` | `string` | The final verdict whether to allow or block the traffic based on the rule. |
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

## CiscoUmbrella.DNS

DNS logs show traffic that has reached our DNS resolvers. Reference: [https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning\#section-dns-logs](https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning#section-dns-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`timestamp`** | `timestamp` | When this request was made in UTC. This is different than the Umbrella dashboard, which converts the time to your specified time zone. |
| `policyIdentity` | `string` | The first identity that matched the request. |
| `identities` | `[string]` | All identities associated with this request. |
| `internalIp` | `string` | The internal IP address that made the request. |
| `externalIp` | `string` | The external IP address that made the request. |
| `action` | `string` | Whether the request was allowed or blocked. |
| `queryType` | `string` | The type of DNS request that was made. For more information, see Common DNS Request Types. |
| `responseCode` | `string` | The DNS return code for this request. For more information, see Common DNS return codes for any DNS service \(and Umbrella\). |
| `domain` | `string` | The domain that was requested. |
| `categories` | `[string]` | The security or content categories that the destination matches. |
| `policyIdentityType` | `string` | The first identity type matched with this request. Available in version 3 and above. |
| `identityTypes` | `[string]` | The type of identity that made the request. For example, Roaming Computer, Network, and so on. Available in version 3 and above. |
| `blockedCategories` | `[string]` | The categories that resulted in the destination being blocked. Available in version 4 and above. |
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

## CiscoUmbrella.IP

IP logs show traffic that has been handled by the IP Layer Enforcement feature. Reference: [https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning\#section-ip-logs](https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning#section-ip-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`timestamp`** | `timestamp` | The timestamp of the request transaction in UTC \(2015-01-16 17:48:41\). |
| `identity` | `string` | The first identity that matched the request. |
| `sourceIp` | `string` | The IP of the computer making the request. |
| `sourcePort` | `int` | The port the request was made on. |
| `destinationIp` | `string` | The destination IP requested. |
| `destinationPort` | `int` | The destination port the request was made on. |
| `categories` | `[string]` | Which security categories, if any, matched against the destination IP address/port requested. |
| `identityTypes` | `[string]` | The type of identity that made the request. For example, Roaming Computer, Network, and so on. Available in version 3 and above. |
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

## CiscoUmbrella.Proxy

Proxy logs show traffic that has passed through the Umbrella Secure Web Gateway or the Selective Proxy. Reference: [https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning\#section-proxy-logs](https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning#section-proxy-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| `timestamp` | `timestamp` | The timestamp of the request transaction in UTC \(2015-01-16 17:48:41\). |
| `identity` | `string` | The first identity that matched the request. |
| `identities` | `[string]` | Which identities, in order of granularity, made the request through the intelligent proxy. |
| `internalIp` | `string` | The internal IP address of the computer making the request. |
| `externalIp` | `string` | The egress IP address of the network where the request originated. |
| `destinationIp` | `string` | The destination IP address of the request. |
| `contentType` | `string` | The type of web content, typically text/html. |
| `verdict` | `string` | Whether the destination was blocked or allowed. |
| `url` | `string` | The URL requested. |
| `referrer` | `string` | The referring domain or URL. |
| `userAgent` | `string` | The browser agent that made the request. |
| `statusCode` | `int` | The HTTP status code; should always be 200 or 201. |
| `requestSize` | `bigint` | Request size in bytes. |
| `responseSize` | `bigint` | Response size in bytes. |
| `responseBodySize` | `bigint` | Response body size in bytes. |
| `sha` | `string` | SHA256 hex digest of the response content. |
| `categories` | `[string]` | The security categories for this request, such as Malware. |
| `avDetections` | `[string]` | The detection name according to the antivirus engine used in file inspection. |
| `puas` | `[string]` | A list of all potentially unwanted application \(PUA\) results for the proxied file as returned by the antivirus scanner. |
| `ampDisposition` | `string` | The status of the files proxied and scanned by Cisco Advanced Malware Protection \(AMP\) as part of the Umbrella File Inspection feature; can be Clean, Malicious or Unknown. |
| `ampMalwareName` | `string` | If Malicious, the name of the malware according to AMP. |
| `ampScore` | `string` | The score of the malware from AMP. This field is not currently used and will be blank. |
| `identityType` | `string` | The type of identity that made the request. For example, Roaming Computer, Network, and so on. |
| `blockedCategories` | `[string]` | The categories that resulted in the destination being blocked. Available in version 4 and above. |
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

