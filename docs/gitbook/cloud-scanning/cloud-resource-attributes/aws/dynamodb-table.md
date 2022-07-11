# DynamoDB Table

## Resource Type

`AWS.DynamoDB.Table`

## Resource ID Format

For DynamoDB Tables, the resource ID is the ARN.

`arn:aws:dynamodb:us-east-2:123456789012:table/example-table`

## Background

This resource represents a snapshot of an AWS DynamoDB Table.

## Fields

| Field                     | Type      | Description                                                                                                                                    |
| ------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `AttributeDefinitions`    | `List`    | Attributes that describe the key schema for the table and indexes.                                                                             |
| `BillingModeSummary`      | `Map`     | The details for the read/write capacity mode.                                                                                                  |
| `GlobalSecondaryIndexes`  | `List`    | The global secondary indexes.                                                                                                                  |
| `ItemCount`               | `Integer` | The number of items in the specified table.                                                                                                    |
| `KeySchema`               | `List`    | The primary key structure for the table.                                                                                                       |
| `LatestStreamArn`         | `String`  | The ARN that uniquely identifies the latest stream for this table.                                                                             |
| `LatestStreamLabel`       | `String`  | A timestamp, in ISO 8601 format, for this stream.                                                                                              |
| `LocalSecondaryIndexes`   | `List`    | One or more local secondary indexes on the table.                                                                                              |
| `ProvisionedThroughput`   | `Map`     | The provisioned throughput settings for the table, consisting of read and write capacity units, along with data about increases and decreases. |
| `RestoreSummary`          | `Map`     | Details for the restore.                                                                                                                       |
| `SSEDescription`          | `Map`     | The description of the server-side encryption status on the specified table.                                                                   |
| `StreamSpecification`     | `Map`     | The current DynamoDB Streams configuration for the table.                                                                                      |
| `TableSizeBytes`          | `Integer` | The total size of the specified table, in bytes.                                                                                               |
| `TableStatus`             | `String`  | The current state of the table                                                                                                                 |
| `AutoScalingDescriptions` | `List`    | Details on how dynamically adjusting provisioned throughput capacity in response to traffic patterns is configured                             |
| `TimeToLiveDescription`   | `Map`     | The description of the Time to Live (TTL) status on the specified table.                                                                       |

## Example

```javascript
{
    "AccountId": "123456789012",
    "Arn": "arn:aws:dynamodb:us-east-2:123456789012:table/example-table",
    "AttributeDefinitions": [
        {
            "AttributeName": "Attribute1",
            "AttributeType": "S"
        }
    ],
    "AutoScalingDescriptions": [
        {
            "CreationTime": "2019-01-01T00:00:00Z",
            "MaxCapacity": 40000,
            "MinCapacity": 5,
            "ResourceId": "table/example-table",
            "RoleARN": "arn:aws:iam::123456789012:role/aws-service-role/dynamodb.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_DynamoDBTable",
            "ScalableDimension": "dynamodb:table:ReadCapacityUnits",
            "ServiceNamespace": "dynamodb",
            "SuspendedState": {
                "DynamicScalingInSuspended": false,
                "DynamicScalingOutSuspended": false,
                "ScheduledScalingSuspended": false
            }
        },
        {
            "CreationTime": "2019-01-01T00:00:00Z",
            "MaxCapacity": 40000,
            "MinCapacity": 5,
            "ResourceId": "table/example-table",
            "RoleARN": "arn:aws:iam::123456789012:role/aws-service-role/dynamodb.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_DynamoDBTable",
            "ScalableDimension": "dynamodb:table:WriteCapacityUnits",
            "ServiceNamespace": "dynamodb",
            "SuspendedState": {
                "DynamicScalingInSuspended": false,
                "DynamicScalingOutSuspended": false,
                "ScheduledScalingSuspended": false
            }
        }
    ],
    "BillingModeSummary": null,
    "GlobalSecondaryIndexes": null,
    "Id": "1111-2222",
    "ItemCount": 0,
    "KeySchema": [
        {
            "AttributeName": "Attribute1",
            "KeyType": "HASH"
        }
    ],
    "LatestStreamArn": "arn:aws:dynamodb:us-east-2:123456789012:table/example-table/stream/2019-01-01T00:00:00.000",
    "LatestStreamLabel": "2019-01-01T00:00:00.000",
    "LocalSecondaryIndexes": null,
    "Name": "example-table",
    "ProvisionedThroughput": {
        "LastDecreaseDateTime": null,
        "LastIncreaseDateTime": null,
        "NumberOfDecreasesToday": 0,
        "ReadCapacityUnits": 5,
        "WriteCapacityUnits": 5
    },
    "Region": "us-east-2",
    "ResourceId": "arn:aws:dynamodb:us-east-2:123456789012:table/example-table",
    "ResourceType": "AWS.DynamoDB.Table",
    "RestoreSummary": null,
    "SSEDescription": null,
    "StreamSpecification": {
        "StreamEnabled": true,
        "StreamViewType": "NEW_AND_OLD_IMAGES"
    },
    "TableSizeBytes": 0,
    "TableStatus": "ACTIVE",
    "Tags": null,
    "TimeCreated": "2019-01-01T00:00:00.000Z",
    "TimeToLiveDescription": {
        "AttributeName": null,
        "TimeToLiveStatus": "DISABLED"
    }
}
```
