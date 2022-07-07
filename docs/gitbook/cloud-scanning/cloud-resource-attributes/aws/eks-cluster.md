---
description: Amazon Elastic Kubernetes Service Cluster
---

# EKS Cluster

## Resource Type

`AWS.EKS.Cluster`

## Resource ID Format

The resource ID is the cluster ARN.

`arn:aws:eks:us-west-2:123456789012:cluster/prod-cluster`

## Background

This resource represents a snapshot of an AWS EKS Cluster.

## Fields

| Field                  | Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `CertificateAuthority` | `Map`    | The certificate-authority-data for your cluster.                                                                                                                                                                                                                                                                                                                                                                         |
| `EncryptionConfig`     | `List`   | The encryption configuration for the cluster.                                                                                                                                                                                                                                                                                                                                                                            |
| `Endpoint`             | `String` | The endpoint for your Kubernetes API server.                                                                                                                                                                                                                                                                                                                                                                             |
| `Identity`             | `Map`    | The identity provider information for the cluster.                                                                                                                                                                                                                                                                                                                                                                       |
| `Logging`              | `Map`    | The logging configuration for your cluster.                                                                                                                                                                                                                                                                                                                                                                              |
| `PlatformVersion`      | `String` | The platform version of your Amazon EKS cluster. For more information, see Platform Versions (https://docs.aws.amazon.com/eks/latest/userguide/platform-versions.html) in the Amazon EKS User Guide .                                                                                                                                                                                                                    |
| `ResourcesVpcConfig`   | `Map`    | The VPC configuration used by the cluster control plane. Amazon EKS VPC resources have specific requirements to work properly with Kubernetes. For more information, see Cluster VPC Considerations (https://docs.aws.amazon.com/eks/latest/userguide/network\_reqs.html) and Cluster Security Group Considerations (https://docs.aws.amazon.com/eks/latest/userguide/sec-group-reqs.html) in the Amazon EKS User Guide. |
| `RoleArn`              | `String` | The Amazon Resource Name (ARN) of the IAM role that provides permissions for the Kubernetes control plane to make calls to Amazon Web Services API operations on your behalf.                                                                                                                                                                                                                                            |
| `Status`               | `String` | The current status of the cluster.                                                                                                                                                                                                                                                                                                                                                                                       |
| `Version`              | `String` | The Kubernetes server version for the cluster.                                                                                                                                                                                                                                                                                                                                                                           |
| `NodeGroup`            | `List`   | The Amazon EKS managed node group.                                                                                                                                                                                                                                                                                                                                                                                       |
| `FargateProfile`       | `List`   | The Amazon EKS fargate profile                                                                                                                                                                                                                                                                                                                                                                                           |

## Example

```javascript
{
  "AccountId": "123456789012",
  "Arn": "arn:aws:eks:us-east-1:123456789012:cluster/prod-cluster",
  "CertificateAuthority": {
    "Data": "BASE64=="
  },
  "Endpoint": "https://ABCD.gr7.us-east-1.eks.amazonaws.com",
  "Identity": {
    "Oidc": {
      "Issuer": "https://oidc.eks.us-east-1.amazonaws.com/id/ABCD"
    }
  },
  "Logging": {
    "ClusterLogging": [
      {
        "Enabled": true,
        "Types": ["api", "audit", "authenticator", "controllerManager", "scheduler"]
      }
    ]
  },
  "Name": "prod-cluster",
  "NodeGroup": [
    {
      "AccountId": null,
      "AmiType": "AL2_x86_64",
      "Arn": "arn:aws:eks:us-east-1:123456789012:nodegroup/prod-cluster/panther-nodegroup/a8c014e7-1745-153s-8629-fdd2310dd454",
      "DiskSize": 20,
      "Health": {
        "Issues": null
      },
      "InstanceTypes": ["t3.medium"],
      "LaunchTemplate": null,
      "NodeRole": "arn:aws:iam::123456789012:role/pantherAmazonEKSNodeRole",
      "NodegroupArn": "arn:aws:eks:us-east-1:123456789012:nodegroup/prod-cluster/panther-nodegroup/a8c014e7-1745-153s-8629-fdd2310dd454",
      "NodegroupName": "panther-nodegroup",
      "Region": null,
      "ReleaseVersion": "1.21.5-20220406",
      "RemoteAccess": null,
      "Resources": {
        "AutoScalingGroups": [
          {
            "Name": "eks-panther-nodegroup-a8c014e7-1745-153s-8629-fdd2310dd454"
          }
        ],
        "RemoteAccessSecurityGroup": null
      },
      "ScalingConfig": {
        "DesiredSize": 2,
        "MaxSize": 2,
        "MinSize": 2
      },
      "Subnets": [
        "subnet-04f8c113e077c380d",
        "subnet-0f4f3b663720faa55",
        "subnet-0667b8049fd745gae",
        "subnet-04c1f3d997e75c82f"
      ],
      "Tags": null,
      "TimeCreated": "2022-04-14T20:40:28.917Z",
      "Version": "1.21"
    }
  ],
  "PlatformVersion": "eks.2",
  "Region": "us-east-1",
  "ResourceId": "arn:aws:eks:us-east-1:123456789012:cluster/prod-cluster",
  "ResourceType": "AWS.EKS.Cluster",
  "ResourcesVpcConfig": {
    "ClusterSecurityGroupId": "sg-03avb0a6b09a782df",
    "EndpointPrivateAccess": false,
    "EndpointPublicAccess": true,
    "PublicAccessCidrs": ["0.0.0.0/0"],
    "SecurityGroupIds": null,
    "SubnetIds": [
      "subnet-04f8c113e077c380d",
      "subnet-0f4f3b663720faa55",
      "subnet-0667b8049fd745gae",
      "subnet-04c1f3d997e75c82f"
    ],
    "VpcId": "vpc-006d2efcd9b2df2f2"
  },
  "RoleArn": "arn:aws:iam::123456789012:role/pantherEKSClusterRole",
  "Status": "ACTIVE",
  "TimeCreated": "2022-04-14T19:45:31.286Z",
  "Version": "1.22"
}
```
