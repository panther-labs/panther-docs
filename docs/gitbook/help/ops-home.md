# Operations

## Monitoring

### Visibility

Panther has 5 CloudWatch dashboards to provide visibility into the operation of the system:

* **PantherOverview** An overview all errors and performance of all Panther components.
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
* [PagerDuty Integration](https://support.pagerduty.com/docs/aws-cloudwatch-integration-guide)

  NOTE: As of this writing \(August 2020\) Pager Duty cannot [handle composite CloudWatch alarms](https://community.pagerduty.com/forum/t/composite-alarm-in-cloudwatch-not-triggering-pd-integration/1798) which Panther uses to avoid duplicate pages to oncall staff. The work around is to use a `Custom Event Transformer`.

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

![](../.gitbook/assets/cloudwatch-dashboards%20%286%29%20%286%29%20%288%29%20%286%29.png)

* Select the dashboard beginning with `PantherLogAnalysis`

![](../.gitbook/assets/cloudwatch-dashboards-log-analysis%20%286%29%20%286%29%20%284%29.png)

* Select the vertical `...` of the pane entitled `Input MBytes (Uncompressed) by Log Type` and select from the menu `View in CloudWatch Insights`

![](../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-select%20%286%29%20%286%29%20%288%29%20%281%29.png)

* Set the time period for 4 weeks and click `Apply`

![](../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-select-time%20%286%29%20%286%29%20%288%29%20%283%29.png)

* Click `Run Query`

![](../.gitbook/assets/cloudwatch-dashboards-log-analysis-input-show%20%286%29%20%286%29%20%288%29%20%284%29.png)

## Tools

Panther comes with some operational tools useful for managing the Panther infrastructure. These are statically compiled executables for linux, mac \(AKA darwin\) and windows. They can be copied/installed onto operational support hosts.

* **compact**: backfill JSON to Parquet conversion of log data \(used when upgrading to Panther Enterprise\)
* **flushrsc**: flush delete pending entries from the panther-resource table.
* **gluesync**: update glue table and partition schemas
* **invite**: invite the initial user to Panther \(if you did not get an invitation during deployment or need to resend it for an existing user\)
* **requeue**: copy messages from a dead letter queue back to the originating queue for reprocessing
* **s3queue**: list files under an S3 path and send to the log processor input queue for processing \(useful for back fill of data\)
* **snowpipe**: generate a ddl file to configure Snowflake to ingest Panther data
* **pantherlog**: Parse logs using built-in or custom schemas

These tools require that the [AWS credentials](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html) be set in the environment. We recommend a tool to manage these securely such as [AWS Vault](https://github.com/99designs/aws-vault).

Running these commands with a `-h` flag will explain usage.

Opstools are found at `https://panther-community-us-east-1.s3.amazonaws.com/{version}/tools/ops/{tool-name}-{architecture}`

`{version}` is latest Panther version, e.g. v1.17.0

`{tool-name}` is one of:

* compact
* cost
* flushrsc
* gluerecover
* gluesync
* invite
* migrate
* releasecopy
* requeue
* s3queue
* s3sns
* snowconfig
* snowpipe
* pantherlog

`{architecture}` is one of:

* `darwin-amd64`
* `linux-amd64`
* `linux-arm`
* `windows-amd64.exe`
* `windows-arm.exe`

Devtools are found at `https://panther-community-us-east-1.s3.amazonaws.com/{version}/tools/dev/{tool-name}-{architecture}`

`{version}` and `{architecture}` are shared with opstools.

`{tool-name}` is one of:

* filegen
* logprocessor
* pantherlog

An example of a full link to a devtool would be [https://panther-community-us-east-1.s3.amazonaws.com/v1.17.0/tools/dev/pantherlog-darwin-amd64](https://panther-community-us-east-1.s3.amazonaws.com/v1.17.0/tools/dev/pantherlog-darwin-amd64)

