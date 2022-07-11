# CloudTrail

## Resource Type

`AWS.CloudTrail`

## Resource ID Format

For CloudTrail Trails, the resource ID is the ARN.

`arn:aws:cloudtrail:us-west-2:123456789012:trail/example-trail`

## Background

The [CloudTrail](https://aws.amazon.com/cloudtrail/) resource represents the system within AWS responsible for tracking account activity.

## Fields

| Field                        | Type      | Description                                                                                                                 |
| ---------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------- |
| `CloudWatchLogsLogGroupArn`  | `String`  | An Amazon Resource Name that represents the log group to which CloudTrail logs will be delivered.                           |
| `CloudWatchLogsRoleArn`      | `String`  | The role for the CloudWatch Logs endpoint to assume to write to a user's log group.                                         |
| `HasCustomEventSelectors`    | `Boolean` | Specifies if the trail has custom event selectors.                                                                          |
| `HomeRegion`                 | `String`  | The region in which the trail was created.                                                                                  |
| `IncludeGlobalServiceEvents` | `Boolean` | Boolean to include Amazon Web Services API calls from Amazon Global Services.                                               |
| `IsMultiRegionTrail`         | `Boolean` | Whether the trail exists only in one region or exists in all regions.                                                       |
| `IsOrganizationTrail`        | `Boolean` | Whether the trail is an organization trail.                                                                                 |
| `KmsKeyId`                   | `String`  | The KMS key ID that encrypts the logs delivered by CloudTrail.                                                              |
| `LogFileValidationEnabled`   | `Boolean` | Whether log file validation is enabled.                                                                                     |
| `S3BucketName`               | `String`  | The name of the Amazon S3 bucket into which CloudTrail delivers the trail files.                                            |
| `S3KeyPrefix`                | `String`  | The Amazon S3 key prefix that comes after the name of the S3 bucket.                                                        |
| `SnsTopicARN`                | `String`  | The ARN of the Amazon SNS topic that CloudTrail uses to send notifications when log files are delivered.                    |
| `EventSelectors`             | `List`    | The collection of management and data event settings across each CloudTrail in each region                                  |
| `Status`                     | `Map`     | CloudTrail [status](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API\_GetTrailStatus.html) of last events. |

## Example

```javascript
{
    "AccountId": "123456789012",
    "Arn": "arn:aws:cloudtrail:us-west-2:123456789012:trail/example-trail",
    "CloudWatchLogsLogGroupArn": null,
    "CloudWatchLogsRoleArn": null,
    "EventSelectors": [
        {
            "DataResources": [
                {
                    "Type": "AWS::S3::Object",
                    "Values": null
                }
            ],
            "IncludeManagementEvents": true,
            "ReadWriteType": "All"
        }
    ],
    "HasCustomEventSelectors": true,
    "HomeRegion": "us-west-2",
    "IncludeGlobalServiceEvents": true,
    "IsMultiRegionTrail": true,
    "IsOrganizationTrail": true,
    "KmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/1111",
    "LogFileValidationEnabled": true,
    "Name": "example-trail",
    "Region": "us-west-2",
    "ResourceId": "arn:aws:cloudtrail:us-west-2:123456789012:trail/example-trail",
    "ResourceType": "AWS.CloudTrail",
    "S3BucketName": "example-bucket",
    "S3KeyPrefix": null,
    "SnsTopicARN": "arn:aws:sns:us-west-2:123456789012:example-topic",
    "SnsTopicName": "arn:aws:sns:us-west-2:123456789012:example-topic",
    "Status": {
        "IsLogging": true,
        "LatestCloudWatchLogsDeliveryError": null,
        "LatestCloudWatchLogsDeliveryTime": null,
        "LatestDeliveryAttemptSucceeded": "2019-01-01T00:00:00Z",
        "LatestDeliveryAttemptTime": "2019-01-01T00:00:00Z",
        "LatestDeliveryError": null,
        "LatestDeliveryTime": "2019-01-01T00:00:00Z",
        "LatestDigestDeliveryError": null,
        "LatestDigestDeliveryTime": "2019-01-01T00:00:00Z",
        "LatestNotificationAttemptSucceeded": "2019-01-01T00:00:00Z",
        "LatestNotificationAttemptTime": "2019-01-01T00:00:00Z",
        "LatestNotificationError": null,
        "LatestNotificationTime": "2019-01-01T00:00:00Z",
        "StartLoggingTime": "2019-01-01T00:00:00Z",
        "StopLoggingTime": null,
        "TimeLoggingStarted": "2019-01-01T00:00:00Z",
        "TimeLoggingStopped": null
    },
    "Tags": null,
    "TimeCreated": null
}
```
