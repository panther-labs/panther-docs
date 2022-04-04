# Basic vs. Advanced

### Data Included with GreyNoise Basic Package <a href="#data-included-with-greynoise-basic-package" id="data-included-with-greynoise-basic-package"></a>

#### Noise Dataset <a href="#noise-dataset" id="noise-dataset"></a>

The following fields are included from the Noise dataset at no extra cost with **GreyNoise Basic**:

| Field Name       | Field Type | Example | Description                                                                  |
| ---------------- | ---------- | ------- | ---------------------------------------------------------------------------- |
| `ip`             | string     | 1.2.3.4 | IP address that information is about.                                        |
| `actor`          | string     | unknown | The confirmed owner/operator of this IP address.                             |
| `classification` | string     | unknown | <p>IP Classification - possible options: benign, unknown, malicious.<br></p> |

#### RIOT Dataset <a href="#riot-dataset" id="riot-dataset"></a>

The following fields are included from the RIOT dataset at no extra cost with **GreyNoise Basic**:

| Field Name | Field Type | Example           | Description                              |
| ---------- | ---------- | ----------------- | ---------------------------------------- |
| `ip`       | string     | 8.8.8.8           | IP address that information is about.    |
| `name`     | string     | Google Public DNS | The name of the provider and/or service. |

### Data Included with GreyNoise Advanced Package <a href="#data-included-with-greynoise-advanced-package" id="data-included-with-greynoise-advanced-package"></a>

#### Noise Dataset <a href="#noise-dataset-1" id="noise-dataset-1"></a>

The following fields are included from the Noise dataset with **GreyNoise Advanced**:

| Field Name                   | Field Type  | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Description                                                                                                                      |
| ---------------------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `actor`                      | string      | unknown                                                                                                                                                                                                                                                                                                                                                                                                                                                    | The confirmed owner/operator of this IP address.                                                                                 |
| `bot`                        | boolean     | false                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Data Enrichment - IP is associated with known bot activity.                                                                      |
| `classification`             | string      | unknown                                                                                                                                                                                                                                                                                                                                                                                                                                                    | IP Classification - possible options: benign, unknown, malicious.                                                                |
| `cve`                        | string list | <p>[<br>"CVE-2021-38645",<br>"CVE-2021-38647"<br>]</p>                                                                                                                                                                                                                                                                                                                                                                                                     | List of CVEs the IP has been observed scanning for or exploiting                                                                 |
| `first_seen`                 | date        | 2021-11-23                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Date of first observed behavior on the GreyNoise Sensor network (format: YYYY-MM-DD).                                            |
| `ip`                         | string      | 1.2.3.4                                                                                                                                                                                                                                                                                                                                                                                                                                                    | IP address that information is about                                                                                             |
| `last_seen`                  | date        | 2021-12-31                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Date of last observed behavior on the GreyNoise Sensor network (format: YYYY-MM-DD).                                             |
| `metadata`                   | object      | <p>{</p><p>"asn": "AS37963",</p><p>"category": "hosting",</p><p>"city": "Hangzhou",</p><p>"country": "China",</p><p>"country_code": "CN",</p><p>"organization": "Hangzhou Alibaba Advertising Co.,Ltd.",</p><p>"os": "Linux 3.11+",</p><p>"rdns": "",</p><p>"region": "Zhejiang", "tor": false</p><p>}</p>                                                                                                                                                 | Data Enrichment - Additional IP metadata.                                                                                        |
| `metadata.asn`               | string      | AS37963                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Data Enrichment - IPs attached ASN.                                                                                              |
| `metadata.category`          | string      | hosting                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Data Enrichment - IPs attached category.                                                                                         |
| `metadata.city`              | string      | Miami                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Data Enrichment - IPs attached city.                                                                                             |
| `metadata.country`           | string      | United States                                                                                                                                                                                                                                                                                                                                                                                                                                              | Data Enrichment - IPs attached country.                                                                                          |
| `metadata.country_code`      | string      | US                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Data Enrichment - IPs attached country code.                                                                                     |
| `metadata.organization`      | string      | FranTech Solutions                                                                                                                                                                                                                                                                                                                                                                                                                                         | Data Enrichment - IPs attached organization.                                                                                     |
| `metadata.os`                | string      | Linux 2.2-3.x                                                                                                                                                                                                                                                                                                                                                                                                                                              | Data Enrichment - IPs attached operating system.                                                                                 |
| `metadata.rdns`              | string      | miamitor4.us                                                                                                                                                                                                                                                                                                                                                                                                                                               | Data Enrichment - rDNS lookup for IP.                                                                                            |
| `metadata.region`            | string      | Florida                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Data Enrichment - IPs attached region.                                                                                           |
| `metadata.tor`               | boolean     | true                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Data Enrichment - IP is a known tor exit node.                                                                                   |
| `raw_data`                   | object      | <p>{<br>"hassh": [<br>{<br>"fingerprint": "a7a87fbe86774c2e40cc4a7ea2ab1b3c",<br>"port": 22<br>}<br>],<br>"ja3": [<br>{<br>"fingerprint": "19e29534fd49dd27d09234e639c4057e",<br>"port": 8443<br>}<br>],<br>"scan": [<br>{<br>"port": 22,<br>"protocol": "TCP"<br>}<br>],<br>"web": {<br>"paths": [<br>"/favicon.ico"<br>],<br>"useragents": [<br>"Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)"<br>]<br>}<br>}</p> | Observed Activity captured by the GreyNoise sensor network.                                                                      |
| `raw_data.hassh`             | object list | <p>[<br>{<br>"fingerprint": "a7a87fbe86774c2e40cc4a7ea2ab1b3c",<br>"port": 22<br>}<br>]</p>                                                                                                                                                                                                                                                                                                                                                                | Observed HAASH activity.                                                                                                         |
| `raw_data.hassh.fingerprint` | string      | a7a87fbe86774c2e40cc4a7ea2ab1b3c                                                                                                                                                                                                                                                                                                                                                                                                                           | HASSH Fingerprint captured.                                                                                                      |
| `raw_data.hassh.port`        | int         | 22                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Port observed activity occurred on                                                                                               |
| `raw_data.ja3`               | object list | <p>[<br>{<br>"fingerprint": "19e29534fd49dd27d09234e639c4057e",<br>"port": 8443<br>}<br>]</p>                                                                                                                                                                                                                                                                                                                                                              | Observed JA3 activity.                                                                                                           |
| `raw_data.ja3.fingerprint`   | string      | 19e29534fd49dd27d09234e639c4057e                                                                                                                                                                                                                                                                                                                                                                                                                           | JA3 Fingerprint captured                                                                                                         |
| `raw_data.ja3.port`          | int         | 8443                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Port observed activity occurred on.                                                                                              |
| `raw_data.scan`              | object list | \[ { "port": 22, "protocol": "TCP" } ]                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                                                  |
| `raw_data.scan.port`         | int         | 22                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Port observed activity occurred on.                                                                                              |
| `raw_data.scan.protocol`     | string      | TCP                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Protocol observed activity occurred on.                                                                                          |
| `raw_data.web`               | object      | <p>{<br>"paths": [<br>"/favicon.ico"<br>],<br>"useragents": [<br>"Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)"<br>]<br>}</p>                                                                                                                                                                                                                                                                                       | Observed scanning activity occurred with these web objects.                                                                      |
| `raw_data.web.paths`         | string list | <p>[ </p><p>"/favicon.ico"</p><p>]</p>                                                                                                                                                                                                                                                                                                                                                                                                                     | Observed scanning activity traversed this web path.                                                                              |
| `raw_data.web.useragents`    | string list | <p>[<br>"Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)"<br>]</p>                                                                                                                                                                                                                                                                                                                                                     | Observed scanning activity used these user agents.                                                                               |
| `spoofable`                  | boolean     | false                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Did this IP complete a three-way handshake with the GreyNoise sensor network? If false, indicates that traffic _may_ be spoofed. |
| `tags`                       | string list | <p>[<br>"Carries HTTP Referer",<br>"Cobalt Strike SSH Client",<br>"Follows HTTP Redirects"<br>]</p>                                                                                                                                                                                                                                                                                                                                                        | List of GreyNoise tags associated with the observed scanning behavior performed by this IP.                                      |
| `vpn`                        | boolean     | false                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Data Enrichment - IP is a known VPN service IP.                                                                                  |
| `vpn_service`                | string      | PIA\_VPN                                                                                                                                                                                                                                                                                                                                                                                                                                                   | If IP is a known VPN, the name of the associated VPN Service.                                                                    |

#### RIOT Dataset <a href="#riot-dataset-1" id="riot-dataset-1"></a>

The following fields are included from the RIOT dataset with **GreyNoise Advanced**:

| Field Name     | Field Type | Example                                                                                                                                                                                         | Description                                                                                                                                                                                                                 |
| -------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ip`           | string     | 8.8.8.8                                                                                                                                                                                         | IP address that information is about.                                                                                                                                                                                       |
| `name`         | string     | Google Public DNS                                                                                                                                                                               | The name of the provider and/or service.                                                                                                                                                                                    |
| `category`     | string     | public\_dns                                                                                                                                                                                     | The RIOT category the provider belongs to identifying the type of service provided.                                                                                                                                         |
| `description`  | string     | Google's global domain name system (DNS) resolution service.                                                                                                                                    | A description of the provider and what they do.                                                                                                                                                                             |
| `explanation`  | string     | Public DNS services are used as alternatives to ISP's name servers. You may see devices on your network communicating with Google Public DNS over port 53/TCP or 53/UDP to resolve DNS lookups. | An explanation of the category type and what may be expected from this provider and category.                                                                                                                               |
| `last_updated` | datetime   | 2021-11-24T11:42:37Z                                                                                                                                                                            | Date and time when this record was last updated from its source (format: YYYY-MM-DDTHH:MM:SSZ).                                                                                                                             |
| `logo_url`     | string     | <p>https[:]//upload.wikimedia.org/wikipedia/<br>commons/2/2f/Google_2015_logo.svg</p>                                                                                                           | URL to a logo for the provider (unused in most cases and generally can be ignored/excluded).                                                                                                                                |
| `reference`    | url        | <p>https[:]//developers.google.com/speed/<br>public-dns/docs/isp#alternative</p>                                                                                                                | Reference URL for information about this provider and/or service.                                                                                                                                                           |
| `trust_level`  | string     | 1                                                                                                                                                                                               | <p>GreyNoise defines the trust level assigned to this IP/provider. Additional information on trust levels can be found <a href="https://docs.greynoise.io/docs/understanding-greynoise-riot-trust-levels">here</a>.<br></p> |
