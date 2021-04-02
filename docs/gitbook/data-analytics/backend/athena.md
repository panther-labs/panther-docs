# Athena

## Using an Athena Backend

If a Snowflake cluster connection is not set up, Panther will default to using an Athena back-end. Panther's data processing engine handles both data compaction into Parquet and ensures that the metadata is current.

This means that historical queries using the partition filters will effectively prune the data being read from storage to , while recent data is instantly available for queries as well.

## Accessing Data with AWS Glue

All log data is stored in AWS [Glue](https://aws.amazon.com/glue/) tables. This makes the data available in many tools such as Athena, Redshift, Glue Spark Jobs and SageMaker.

