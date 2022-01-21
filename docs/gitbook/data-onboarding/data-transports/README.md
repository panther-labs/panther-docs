# Data Transports

### What is a Data Transport?

A data transport is a type of log source that can be used to send log types that aren't natively supported in Panther. In addition to using a Data Transport to onboard a custom log type, you'll have to create a custom schema to normalize and classify the data. Learn more about custom schemas [here](../custom-log-types/). Panther currently supports the following Data Transports:

* [AWS S3](s3.md)
* [AWS CloudWatch](cwl-source.md)&#x20;
* [AWS SQS](sqs.md)
* [AWS SNS](sns.md)
