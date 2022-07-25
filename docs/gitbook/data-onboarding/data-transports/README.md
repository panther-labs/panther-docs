---
description: Panther integrates with various common data transport log ingestion sources
---

# Data Transports

## Overview

A Data Transport is a type of log source that is used to send log types that are not natively supported by Panther. Data Transports aim to successfully process custom data types through Panther’s log processing pipeline, map existing detections to custom data types, and map data models to custom data types. In addition to using a Data Transport to onboard your custom logs, you'll need to create a [custom schema](../custom-log-types/) to normalize and classify the data.

## Panther Supported Data Transports

Panther currently supports the following Data Transport methods:

* [AWS S3](s3.md)
* [AWS CloudWatch](cwl-source.md)&#x20;
* [AWS SQS](sqs/)
* [AWS SNS](sqs/sns.md)
* [Google Cloud Storage (GCS)](https://docs.runpanther.io/data-onboarding/data-transports/gcs)
