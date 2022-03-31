---
description: Identity and Access Management (IAM) Policy
---

# IAM Policy

## Resource Type

`AWS.IAM.Policy`

## Resource ID Format

For IAM Policies, the resource ID is the ARN.

`arn:aws:iam::123456789012:policy/example-policy`

## Background

This resource represents an IAM Policy, which is an entity that, when attached to an identity or resource, defines their permissions.  
Entities that can have an IAM Policy attached are linked below:

{% page-ref page="iam-user.md" %}

{% page-ref page="iam-group.md" %}

{% page-ref page="iam-role.md" %}

### AWS Managed Policies

Default in AWS are IAM Policies managed by AWS, which are distinct from a customer managed policy and inline policy. Generally, these policies are overly-permissive in nature and for that same reason, should generally be avoided.   
Panther will only store an AWS managed policy if it is attached to an IAM entity.  
Since IAM Policies exist within a globally-unique namespace, Panther preserves the stored ARN but modifies the Resource ID as follows:

```go
// Original AWS Managed Policy ARN
arn:aws:iam::aws:policy/example-policy

// Stored Resource ID
arn:aws:iam::{ACCOUNT-ID}:policy/example-policy
```

## Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `Entities` | `Map` | This has three keys, `PolicyGroups`, `PolicyRoles`, and `PolicyUsers`. Each key maps to a list of IAM groups, roles, or users respectively that have the policy attached. |
| `AttachmentCount` | `Int` | The number of entities \(users, groups, and roles\) that the policy is attached to. |
| `DefaultVersionId` | `String` | The identifier for the version of the policy that is set as the default version. |
| `Description` | `String` | A friendly description of the policy. |
| `IsAttachable` | `Bool` | Specifies whether the policy can be attached to an IAM user, group, or role. |
| `Path` | `String` | The path to the policy. |
| `PolicyDocument` | `String` | A JSON policy document describing what permissions this policy grants. |
| `UpdateDate` | `String` | The date and time, in ISO 8601 date-time format, when the policy was last updated. |

## Example

```javascript
{
    "AccountId": "123456789012",
    "Arn": "arn:aws:iam::123456789012:policy/example-policy",
    "AttachmentCount": 1,
    "DefaultVersionId": "v1",
    "Description": null,
    "Entities": {
        "PolicyGroups": null,
        "PolicyRoles": [
            {
                "RoleId": "AAAA",
                "RoleName": "example-role"
            }
        ],
        "PolicyUsers": null
    },
    "Id": "1111",
    "IsAttachable": true,
    "Name": "example-policy",
    "Path": "/",
    "PermissionsBoundaryUsageCount": 0,
    "PolicyDocument": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Action\":[\"kms:Encrypt\",\"kms:Decrypt\",\"kms:GenerateDataKey\"],\"Resource\":\"arn:aws:kms:us-west-2:123456789012:key/1\",\"Effect\":\"Allow\",\"Sid\":\"DecryptSecrets\"},{\"Action\":[\"sqs:SendMessage\",\"sqs:SendMessageBatch\"],\"Resource\":[\"arn:aws:sqs:us-west-2:123456789012:example-queue\"],\"Effect\":\"Allow\",\"Sid\":\"SendSQSMessages\"}]}",
    "Region": "global",
    "ResourceId": "arn:aws:iam::123456789012:policy/example-policy",
    "ResourceType": "AWS.IAM.Policy",
    "Tags": null,
    "TimeCreated": "2019-01-01T00:00:00.000Z",
    "UpdateDate": "2019-01-01T00:00:00Z"
}
```

