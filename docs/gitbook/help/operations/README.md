# Operations

## Monitoring

### Visibility

Panther has 5 CloudWatch dashboards to provide visibility into the operation of the system:

* **PantherOverview** An overview of all errors and performance of all Panther components.
* **PantherCloudSecurity**: Details of the components monitoring infrastructure for CloudSecurity.
* **PantherAlertProcessing**: Details of the components that relay alerts for CloudSecurity and Log Processing.
* **PantherLogAnalysis**: Details of the components processing logs and running rules.
* **PantherRemediation**: Details of the components that remediate infrastructure issues.

### Alarms

Panther uses CloudWatch Alarms to monitor the health of each component. Edit the `deployments/panther_config.yml` file to associate an SNS topic you have created with the Panther CloudWatch alarms to receive notifications. If this value is blank then Panther will associate alarms with the default Panther SNS topic called `panther-alarms`:

```yaml
MonitoringParameterValues:
  # This is the arn for the SNS topic you want associated with Panther system alarms.
  # If this is not set alarms will be associated with the SNS topic `panther-alarms`.
  AlarmSNSTopicARN: 'arn:aws:sns:us-east-1:05060362XXX:MyAlarmSNSTopic'
```

To configure alarms to send to your team, follow the guides below:

* [SNS Email and SMS Integration](https://docs.aws.amazon.com/sns/latest/dg/sns-user-notifications.html)
*   [PagerDuty Integration](https://support.pagerduty.com/docs/aws-cloudwatch-integration-guide)

    NOTE: As of this writing (August 2020) Pager Duty cannot [handle composite CloudWatch alarms](https://community.pagerduty.com/forum/t/composite-alarm-in-cloudwatch-not-triggering-pd-integration/1798) which Panther uses to avoid duplicate pages to oncall staff. The work around is to use a `Custom Event Transformer`.

    Follow the [instructions ](https://www.pagerduty.com/docs/guides/custom-event-transformer/) using the below code:

    ```javascript
       var details = JSON.parse(PD.inputRequest.rawBody);

       var description = "unknown event";
       if ("AlarmDescription" in details) {  // looks like a CloudWatch event ...
         var descLines = details.AlarmDescription.split("\n");
         description = (descLines.length > 1)? descLines[0] + " " + descLines[1] : descLines[0];
       }

       var normalized_event = {
         event_type: PD.Trigger,
         description: description,
         incident_key: description,
         details: details
       };

       PD.emitGenericEvents([normalized_event]);
    ```

    Configure the SNS topic to use `RawMessageDelivery: true` when creating the Pager Duty subscription.

### Assessing Data Ingest Volume

The Panther log analysis CloudWatch dashboard provides deep insight into operationally relevant aspects of log processing. In particular, understanding the ingest volume is critically important to forecast the cost of running Panther. One of the panes in the dashboard will show ingest volume by log type. This can be used, in combination with your AWS bill, to forecast costs as you scale your data. We suggest you use a month of data to estimate your costs.

The steps to view the dashboard:

* Login to the AWS Console
* Select `CloudWatch` from the Services menu
* Select `Dashboards` from the left pane of the CloudWatch console

![](<../../../../.gitbook/assets/cloudwatch-dashboards (6) (6) (8) (9) (1) (1) (3) (1) (1) (1) (2) (4) (7).png>)

* Select the dashboard beginning with `PantherLogAnalysis`

![](<../../../../.gitbook/assets/cloudwatch-dashboards-log-analysis (6) (6) (4) (1) (1) (3) (1) (1) (1) (2) (4) (7).png>)

* Select the vertical `...` of the pane entitled `Input MBytes (Uncompressed) by Log Type` and select from the menu `View in CloudWatch Insights`

![](<../../../../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-select (6) (6) (8) (5) (1) (1) (3) (1) (1) (1) (2) (4) (7).png>)

* Set the time period for 4 weeks and click `Apply`

![](<../../../../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-select-time (6) (6) (8) (9) (1) (1) (3) (1) (1) (1) (2) (4) (7).png>)

* Click `Run Query`

![](<../../../../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-show (6) (6) (8) (6) (1) (1) (3) (1) (1) (1) (2) (4) (7).png>)

## Tools

Panther comes with some operational tools useful for managing the Panther infrastructure. The tools are statically compiled executables for linux, mac (AKA darwin) and windows.

These tools require that [AWS credentials](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html) be set in the environment with sufficient privileges. We recommend a tool to manage these securely such as [AWS Vault](https://github.com/99designs/aws-vault).

{% hint style="warning" %}
Do not run any of these tools unless specifically advised by a Panther team member
{% endhint %}

### Panther v1.27+

Each tool can be downloaded individually from an S3 URL with the following format:\
\
`https://panther-community-us-east-1.s3.amazonaws.com/{version}/tools/{os}-{arch}-{tool}.zip`, where:

* `{version}` is the version of Panther you have deployed, e.g. `v1.27.0`
* `{os}` is one of: `darwin`, `linux` , or `windows`
* `{arch}` is either `amd64` or `arm64`
* `{tool}` is the name of the tool you need (see next section)

&#x20;An example of a complete tool link would be: [https://panther-community-us-east-1.s3.amazonaws.com/v1.27.0/tools/darwin-amd64-snowconfig.zip](https://panther-community-us-east-1.s3.amazonaws.com/v1.23.3/tools/darwin-amd64.zip)

#### Tool Names

Running these commands with the `-h` flag will explain usage.

* **analytics-backfiller:** backfill backend analytics via an EventBridge bus
* **checker:** compares detection entities to every [panther-analysis](https://github.com/panther-labs/panther-analysis) release
* **compact**: backfill JSON-to-Parquet conversion of log data
* **cost**: generates cost reports using the costexplorer api
* **filegen:** writes synthetic log files to s3 for use in benchmarking
* **flushrsc**: flush "delete pending" entries from the panther-resource table
* **gluerecover**: scans S3 for missing AWS Glue partitions and recovers them
* **gluesync**: update glue table and partition schemas
* **historicalmigrate:** migrate historical data from Athena to Snowflake
* **logprocessor:** run log processor locally for profiling purposes using pprof
* **migrate**: utility to do a data migration for the gsuite\_reports table (log & rule table)
* **opslambda:** invokes the `panther-ops-tools` Lambda function to handle some common ops tasks. Over time, more opstool functionality will move into this function
  * **invite:** invites a Panther admin user
  * **requeue:** copy messages from one SQS queue to another, typically to replay DLQ messages
* **pantherlog:** parse logs using built-in or custom schemas
* **s3list:** list all objects in all sources and outputs the S3 objects to a file
* **s3queue**: list files under an S3 path and send them to the log processor input queue for processing (useful for back fill of data)
* **s3sns**: lists S3 objects and posts S3 notifications to the Panther log processor SNS topic
* **s3undelete:** removes S3 delete markers from a versioned bucket
* **snowconfig**: uses an account-admin enabled SF user to configure the databases and roles for the Panther users
* **snowcopy:** uses the shares created by `snowshare` to copy data into a new account
* **snowcreate**: uses the Panther Snowflake ORG admin account and credentials to create new Snowflake accounts
* **snowmelt:** uses an account-admin enabled SF user to destroy the databases and roles for the Panther users
* **snowrepair**: generates a ddl file to configure Snowflake to ingest Panther data
* **snowrotate**: uses an account-admin enabled SF user to rotate the credentials for the two Panther users
* **snowshare:** creates Snowflake data shares of Panther databases between a source and a target account
* **sources**: lists all log sources, optionally validates each log processing role can be assumed and data accessed
* **updater:** takes the CSV output of the `checker` tool and auto-applies the recommended actions

### Panther v1.26.X and Older

In these versions of Panther, all tools were bundled together in a single zipfile: `https://panther-community-us-east-1.s3.amazonaws.com/{version}/tools/{architecture}.zip`

`{version}` is the version of Panther you have deployed, e.g. `v1.23.3`

`{architecture}` is one of the following:

* `darwin-amd64`
* `linux-amd64`
* `linux-arm`
* `windows-amd64`
* `windows-arm`

Each zip archive will contain the entire set of tools (see above for a list of tool names).

An example of a full link to the set of tools would be: [https://panther-community-us-east-1.s3.amazonaws.com/v1.23.3/tools/darwin-amd64.zip](https://panther-community-us-east-1.s3.amazonaws.com/v1.23.3/tools/darwin-amd64.zip)
