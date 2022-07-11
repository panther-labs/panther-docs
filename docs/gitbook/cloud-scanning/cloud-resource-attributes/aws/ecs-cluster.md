---
description: Amazon Elastic Container Service Cluster
---

# ECS Cluster

## Resource Type

`AWS.ECS.Cluster`

## Resource ID Format

The resource ID is the cluster ARN.

`arn:aws:ecs:us-east-1:123456789012:cluster/panther-web-cluster`

## Background

An Amazon ECS cluster is a logical grouping of tasks or services.

## Fields

| Field                               | Type      | Description                                                                   |
| ----------------------------------- | --------- | ----------------------------------------------------------------------------- |
| `ActiveServicesCount`               | `Integer` | The number of services that are running on the cluster in an ACTIVE state.    |
| `Attachments`                       | `String`  | The resources attached to a cluster.                                          |
| `AttachmentsStatus`                 | `String`  | The status of the capacity providers associated with the cluster.             |
| `CapacityProviders`                 | `List`    | The capacity providers associated with the cluster.                           |
| `DefaultCapacityProviderStrategy`   | `List`    | The default capacity provider strategy for the cluster.                       |
| `PendingTasksCount`                 | `Integer` | The number of tasks in the cluster that are in the PENDING state.             |
| `RegisteredContainerInstancesCount` | `Integer` | The number of container instances registered into the cluster.                |
| `RunningTasksCount`                 | `Integer` | The number of tasks in the cluster that are in the RUNNING state.             |
| `Settings`                          | `List`    | The settings for the cluster.                                                 |
| `Statistics`                        | `List`    | Additional information about your clusters that are separated by launch type. |
| `Status`                            | `String`  | The status of the cluster.                                                    |
| `Services`                          | `List`    | The services that the cluster uses.                                           |
| `Tasks`                             | `List`    | The ECS tasks running in the cluster owner.                                   |

## Example

```javascript
{
	"AccountId": "123456789012",
	"ActiveServicesCount": 1,
	"Arn": "arn:aws:ecs:us-west-2:123456789012:cluster/panther-web-cluster",
	"Name": "panther-web-cluster",
	"PendingTasksCount": 0,
	"Region": "us-west-2",
	"RegisteredContainerInstancesCount": 0,
	"ResourceId": "arn:aws:ecs:us-west-2:123456789012:cluster/panther-web-cluster",
	"ResourceType": "AWS.ECS.Cluster",
	"RunningTasksCount": 1,
	"Services": [
		"panther-web"
	],
	"Status": "ACTIVE",
	"Tags": {
		"Application": "Panther",
		"PantherVersion": "1.38",
		"Stack": "panther-web",
		"aws:cloudformation:logical-id": "WebApplicationCluster",
		"aws:cloudformation:stack-id": "arn:aws:cloudformation:us-west-2:123456789012:stack/panther-Web-Stack",
		"aws:cloudformation:stack-name": "panther-Web"
	},
	"Tasks": [
		"arn:aws:ecs:us-west-2:123456789012:task/panther-web-cluster"
	]
}
```
