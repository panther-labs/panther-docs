---
description: Manage legacy self-hosted deployments in Panther
---

# Self-Hosted Deployments (Legacy)

{% hint style="danger" %}
Panther does not support self-hosted deployments for new accounts. Only SaaS deployments are supported for new customers.
{% endhint %}

The following is legacy documentation.

### Upgrades

When Panther publishes a new release, we will notify our self-hosted customers so that they can coordinate upgrades on their schedule. Upgrades should generally be straightforward, but there are a few steps to follow before and during upgrades to make sure everything goes smoothly.

#### Before you upgrade

Before you begin an upgrade, make sure you know what version of Panther to upgrade to. We use semantic versioning, and highly recommend not skipping minor releases. So if for example, you're on version `1.10.X`  and want to upgrade to version `1.13.X`, we recommend first upgrading to the highest patch version of `1.11.X`, then `1.12.X`, and then finally `1.13.X`. This ensures there are no migration issues.

Additionally, if you are using our `PantherDeploymentRole` to deploy Panther, make sure you update the `PantherDeploymentRole` to the correct version for the version of Panther you are deploying. If you are on version `1.13.X` and wish to upgrade to version  `1.14.X`,  make sure the `PantherDeploymentRole`  is also on version `1.14.X` before upgrading. Here is the `PantherDeploymentRole` template URL:

```
https://panther-public-cloudformation-templates.s3.amazonaws.com/panther-deployment-role/{version}/template.yml
```

#### While you upgrade

In order to perform the upgrade, simply find the root Panther stack in the CloudFormation console, click the `Update` button, select `Replace template URL`, and insert the `TemplateURL` for the desired version of Panther you wish to deploy. The template URL should be in this format:

**Example**

****[https://panther-enterprise-us-east-2.s3.amazonaws.com/v1.25.1/panther.yml](https://panther-enterprise-{region}.s3.amazonaws.com/%7Bversion%7D/panther.yml)

```
https://panther-enterprise-{region}.s3.amazonaws.com/{version}/panther.yml
```

You will be prompted to click through a few pages verifying your CloudFormation parameters are correct and that CloudFormation can create IAM resources and nested CloudFormation resources on your behalf.

#### Trigger Pulumi CodeBuild

We are migrating to Pulumi for infrastructure management - after the main Panther stack is deployed, you'll need to start a build for the `panther-pulumi` CodeBuild project. \
\
For example: `aws codebuild start-build --project-name panther-pulumi`

#### Available versions of Panther

We recommend not skipping minor versions of Panther while upgrading, but upgrading to the most recent patch version instead. Here are the most recent patch versions of Panther that we recommend upgrading to:

* `v1.39.7`
* `v1.38.5`
* `v1.37.11`
* `v1.36.0`
* `v1.35.7`
* `v1.34.18`
* `v1.33.5`
* `v1.32.7`
* `v1.31.1`
* `v1.30.5`
* `v1.29.3`
* `v1.28.7`
* `v1.27.8`
* `v1.26.9`
* `v1.25.18`
* `v1.24.12`
* `v1.23.5`

### Reference

#### Naming the root stack

When deploying Panther, you will be provided with a template URL to a root panther stack to deploy. If you're using the `PantherDeploymentRole` to deploy Panther, be sure to name the root stack something with a `panther-` prefix. The name of the root stack will be pre-pended to any resources created by the stack, and the `PantherDeploymentRole` limits its access in part by restricting its permissions to only affect resources that start with the name `panther-`.

#### Configuring deployment parameters

The Panther CloudFormation stack has a number of configurable deployment parameters. Pay special attention to the following options:

* `FirstUserEmail` _**(required)**_: a Panther admin invite will be sent to this email address. Updates to this value are ignored after the first successful deploy.
* `PulumiSecretArn` and `PulumiSecretKeyArn` **(**_**required**_**):** these values will be provided by our team - you will have a dedicated [Pulumi](https://www.pulumi.com/) access token in our organization.
* `OnboardSelf`: whether you want Panther to onboard its own AWS account for monitoring.
* `SentryEnvironment`: by default, application errors are sent to [Sentry](https://sentry.io/) for us to triage. We strongly recommend keeping this enabled with the default value (`prod`), but if that's not an option for you, you can disable the Sentry integration by setting this to a blank string.
* `SupportRoleIdentityAccountId`: by default, a read-only SupportRole is deployed with Panther which our on-call engineers can assume to triage application errors. This role does **not** have access to your data and we’d encourage you to keep it enabled so we can deliver a better support experience. However, if you prefer, this role can be disabled by setting the `SupportRoleIdentityAccountId` to a blank string.
* `OpsRoleIdentityAccountId`: a non-empty value will deploy an OperationsRole with service-level admin permissions for migrations, data recoveries, and other operational emergencies. We recommend keeping this role disabled until necessary (it's off by default).
* `DataLakeForwarderMemory`: Memory to use for Cloud Security DataLake Forwarder lambdas. The default setting is 256, with a maximum value of 2048 and a minimum value of 256.
* `MaxLookupTableCompressedSizeMB`: The maximum size (in MB) of the Gzip-compressed data backing a Lookup Table. The default setting is 200, with a maximum value of 400.
* `CloudSecurityScanSegments`: Segments to use in table scans. The default setting is 5, with a minimum value of 5.
* `ReplayAPIReservedConcurrency`: Reserved concurrency for `panther-replay-log-pusher` Lambda function. The default setting is 40, with a minimum value of 0.
* `EnablePantherAuditLogIngestion`: Enable ingestion of Audit Logs from this instance of Panther, within this instance of Panther. The default setting is `false`, with allowed values of `true` or `false`.
* `PantherAuditLogsExpirationDays`: The expiration in days for Panther Audit Logs - applies to an S3 lifecycle policy. The default setting is 1825, with a minimum value of 30.
* `SnapshotScanWindowMinutes`: If non-zero, deduplicate scan requests in minutes. The default setting is 0, with a minimum value of 0.
* `SnowflakeRBACSecretARN`: ARN pointing at the AWS secret with configuration and credentials for the PANTHER\_RBAC Snowflake user. The allowed pattern is `'^(arn:(aws|aws-cn|aws-us-gov):secretsmanager:[a-z]{2}-[a-z]{4,9}-[1-9]:[0-9]{12}:secret:\S+)?$'`
* `MessageForwarderReservedConcurrency`: Reserved concurrency for panther-message-forwarder Lambda function (has no effect if EnableLambdaReservedConcurrency=false). The default setting is 50, with a minimum value of 0.&#x20;
* `EnableReplays`: Enables or disables the ability to run replays. The default value is `true`, with allowed values of `true` or `false`.
* `PythonRuntime`: The python runtime for AWS Lambda functions. The default value is `python3.7`, with allowed values of `python3.7` and `python3.9`.
* `ReplayProcessorReservedConcurrency`: Reserved concurrency for panther-replay-results-processor Lambda function. The default value is 40, with a minimum value of 0.
* `SnapshotPollerLambdaMemorySize`: Snapshot Poller (Cloud Security) Lambda memory size in MB. The default value is 1024, with a minimum value of 1024 and a maximum value of 10240.
* `DatadogAPIKey`: API key for sending observability data to Datadog.
* `DatadogAPIKeySecretArn`: Secrets Manager arn for API key for sending observability data to Datadog. The default value is `"`, with an allowed pattern of `'^(arn:(aws|aws-cn|aws-us-gov):secretsmanager:[a-z]{2}-[a-z]{4,9}-[1-9]:[0-9]{12}:secret:\S+)?$'`.
* `DatadogAppKeySecretArn`: Secrets Manager arn for App key for setting up AWS Integration for Datadog. The default value is `"`, with an allowed pattern of `'^(arn:(aws|aws-cn|aws-us-gov):secretsmanager:[a-z]{2}-[a-z]{4,9}-[1-9]:[0-9]{12}:secret:\S+)$'.`
* `DatadogExtensionVersion`: The Datadog lambda extension version. The default value is 22, with allowed values of 21 or 22.
* `DatadogEnabled`: Enables or disables sending telemetry data to Datadog. The default value is `false`, with allowed values of `true` or `false`.
* `EnableReports`: Enable viewing reports in the Console. The default value is `false`, with allowed values of `true` or `false`.
* `AirgapSubnetOneIPRange`: A valid & available IP range in the existing VPC you plan to deploy Panther into. Only takes affect if VpcID is specified. Used by the VPC lambdas. The default value is 172.31.254.0/25 with an allowed pattern of `'^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5]).){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])(/([0-9]|[1-2][0-9]|3[0-2]))$'`.
* `AirgapSubnetTwoIPRange`: A second valid & available IP range in the existing VPC you plan to deploy Panther into, for multiple AZ redundancy. Only takes affect if VpcID is specified. Used by the VPC lambdas. The default value is '172.31.254.128/25' with an allowed pattern of `'^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5]).){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])(/([0-9]|[1-2][0-9]|3[0-2]))$'.`
* `FeatureSandboxedExecFlows`: Tells the detections engine which functions should use the Python executor. The default value is `'test-rule'`.
* `EnableIntelligentTiering`: Enable INTELLIGENT\_TIERING on Panther manager S3 buckets. The default value is `false`, with allowed values of `true` or `false`.
* `SnowflakeDDLUpdateConcurrency`: The concurrency used when updating table/view/pipes. The default value is 1, with a minimum value of 1 and a maximum value of 100.
* `EnableAlertsGSIThree`: Feature flag to enable third month partition GSI. The default value is `false`, with allowed values of `true` or `false`.
* `EnableFips`: Use FIPS endpoints for AWS services in US regions. If enabled, may degrade performance. Ignored for Snowflake backends. The default value is `false`, with allowed values of `true` or `false`.
* `SnowflakeRBACSecretARN`: ARN pointing at the AWS secret with config and creds for the PANTHER\_RBAC Snowflake user. The default value is `''` with an allowed pattern of `'^(arn:(aws|aws-cn|aws-us-gov):secretsmanager:[a-z]{2}-[a-z]{4,9}-[1-9]:[0-9]{12}:secret:\S+)?$'`.
* `EnableNewNavigation`: Enable the new navigation page grouping in the UI. The default value is `false`, with allowed values of `true` or `false`.
* `DynamoDBCloudtrailEnabled`: Enables/disables data access logging for specific DynamoDB tables. The default value is `false`, with allowed values of `true` or `false`.
* `SegmentEnvironment`: Segment environment type - leave blank to disable the Segment integration. The default value is `prod`, with allowed values of `''`, `dev`, `staging`, or `prod`. Note: Ensure the write key is defined for the env you choose.
* `SlowRuleMaxUtilization`: The maximum amount of time allowed for a rule to run before we trigger an alarm. The default value is 50, with a minvalue of 1.
* `EnableAlertAssignees`: Enables/disables the ability to assign alerts to users. The default value is `false`, with allowed values of `true` or `false`.
* `EnableMicrosoftGraphPuller`: Enables/disables the Microsoft Graph Puller feature. The default value is `false`, with allowed values of `true` or `false`.
* `Segment`: This write key can only send data to Segment. It cannot read, pull, or edit any data. The dev is `6hqUUfPShNwNGgBM7CaDCybyP4Zq0QoI` and the staging and prod values are both `"`.

Panther has a number of other configuration options besides the ones listed above. We recommend not setting any of these parameters on the first deployment of Panther. If any step of the initial deployment fails, the entire deployment will fail and rollback deleting all infrastructure. After you complete the initial deployment of Panther, you can update the stack with different root parameters. Then if any of these settings cause a deployment failure, Panther will simply roll back to the previous settings without needing an entire fresh deployment. This includes parameters like the snowflake and custom domain configuration parameters.
