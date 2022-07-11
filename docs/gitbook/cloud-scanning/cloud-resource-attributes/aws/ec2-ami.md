---
description: Amazon Machine Image
---

# EC2 AMI

## Resource Type

`AWS.EC2.AMI`

## Resource ID Format

For EC2 AMIs, the resource ID is the ARN.

`arn:aws:ec2:us-west-2:123456789012:image/ami-1`

## Background

EC2 AMIs contain information required to launch an instance.

## Fields

| Field                 | Type      | Description                                                                                                                           |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `Architecture`        | `String`  | The architecture of the image.                                                                                                        |
| `BlockDeviceMappings` | `List`    | Any block device mapping entries.                                                                                                     |
| `Description`         | `String`  | The description of the AMI that was provided during image creation.                                                                   |
| `EnaSupport`          | `Boolean` |                                                                                                                                       |
| `Hypervisor`          | `String`  | The hypervisor type of the image.                                                                                                     |
| `ImageLocation`       | `String`  | The location of the AMI.                                                                                                              |
| `ImageOwnerAlias`     | `String`  | The AWS account alias or the AWS account ID of the AMI owner                                                                          |
| `ImageType`           | `String`  | The type of image.                                                                                                                    |
| `KernelId`            | `String`  | The kernel associated with the image,                                                                                                 |
| `OwnerId`             | `String`  | The AWS account ID of the image owner.                                                                                                |
| `Platform`            | `String`  | The instance operating system.                                                                                                        |
| `ProductCodes`        | `List`    | Any product codes associated with the AMI.                                                                                            |
| `Public`              | `Boolean` | Indicates whether the image has public launch permissions                                                                             |
| `RamdiskId`           | `String`  | The RAM disk associated with the image.                                                                                               |
| `RootDeviceName`      | `String`  | The device name of the root device volume (for example, /dev/sda1).                                                                   |
| `RootDeviceType`      | `String`  | The type of root device used by the AMI.                                                                                              |
| `SriovNetSupport`     | `String`  | Whether enhanced networking with the Intel 82599 Virtual Function interface is enabled.                                               |
| `State`               | `String`  | The current state of the AMI. If the state is `available`, the image is successfully registered and can be used to launch an instance |
| `StateReason`         | `Map`     | The reason for the state change.                                                                                                      |
| `VirtualizationType`  | `String`  | The type of virtualization of the AMI.                                                                                                |

```javascript
{
    "AccountId": "123456789012",
    "Architecture": "x86_64",
    "Arn": "arn:aws:ec2:us-west-2:123456789012:image/ami-1",
    "BlockDeviceMappings": [
        {
            "DeviceName": "/dev/xvda",
            "Ebs": {
                "DeleteOnTermination": true,
                "Encrypted": false,
                "Iops": null,
                "KmsKeyId": null,
                "SnapshotId": "snap-1",
                "VolumeSize": 8,
                "VolumeType": "gp2"
            },
            "NoDevice": null,
            "VirtualName": null
        }
    ],
    "Description": "Image Description",
    "EnaSupport": true,
    "Hypervisor": "xen",
    "Id": "ami-1",
    "ImageLocation": "amazon/amzn-ami.1",
    "ImageOwnerAlias": "amazon",
    "ImageType": "machine",
    "KernelId": null,
    "Name": "amzn-ami.1",
    "OwnerId": "123456789012",
    "Platform": null,
    "ProductCodes": null,
    "Public": true,
    "RamdiskId": null,
    "Region": "us-west-2",
    "ResourceId": "arn:aws:ec2:us-west-2:123456789012:image/ami-1",
    "ResourceType": "AWS.EC2.AMI",
    "RootDeviceName": "/dev/xvda",
    "RootDeviceType": "ebs",
    "SriovNetSupport": "simple",
    "State": "available",
    "StateReason": null,
    "Tags": null,
    "TimeCreated": "2019-01-01T00:00:00.000Z",
    "VirtualizationType": "hvm"
}
```
