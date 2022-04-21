# Lookup Tables (BETA)

{% hint style="info" %}
This feature is available in version 1.27 and newer.

Lookup Tables are currently in public beta. Please share any bug reports and feature requests with your account team.
{% endhint %}

### Overview

Lookup Tables allow you to add important context to your detections and alerts for improved investigation workflows. Use Lookup Tables to enhance alerts with identity/asset information, vulnerability context, network maps, and more. You can associate one or more log types with your Lookup Table, and then all logs of those log types will contain enrichment data from your Lookup Table.

Consider using [Global helpers](https://docs.panther.com/writing-detections/globals) instead when extra information is only needed for a few specific detections and will not be frequently updated.

### Configuring a Lookup Table

You can populate your Lookup Table data using the following methods:

#### Import via File Upload

This option is best for data that is relatively static, such as information about AWS accounts or corporate subnets.&#x20;

For instructions, please see the documentation: [Import via File Upload](file-upload.md).

#### Sync Data from an S3 Source

This option is best for a larger amount of data that updates more frequently from an S3 bucket. Any changes in the S3 bucket will sync to Panther.

For instructions, please see the documentation: [Sync data from an S3 Source](s3-source.md).

### Query via Data Explorer

`p_enrichment` is not stored in the Data Lake, but you can join against the Lookup Table directly to any table in the Data Explorer with a query similar to the following:&#x20;

```
with logs as 
(select * from my_logs), 
lookup as (select * from my_lookup_table) 
select logs.fieldA, lookup.fieldB 
from logs join lookup on logs.selector_field = lookup.key_field
```

### Examples

#### Translating 1Password UUIDs into human readable names

Please see our guide about using Lookup Tables to translate 1Password's Universally Unique Identifier (UUID) values into human readable names: [Using Lookup Tables: 1Password UUIDs](https://docs.runpanther.io/guides/using-lookup-tables-1password-uuids).

#### Lookup Table using CIDR matching

You can write detections that consider the traffic logs from company IP space (e.g. VPNs and hosted systems) differently from others logs originating from public IP space: [Lookup Table example using CIDR matching](file-upload.md#example-using-cidr-matching).
