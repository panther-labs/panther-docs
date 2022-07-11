# EC2 Instance

## Resource Type

`AWS.EC2.Instance`

## Resource ID Format

For EC2 Instances, the resource ID is the ARN.

`arn:aws:ec2:us-west-2:123456789012:instance/i-1`

## Background

This resource represents a snapshot of an AWS EC2 Instance.

## Fields

| Field                                   | Type      | Description                                                                             |
| --------------------------------------- | --------- | --------------------------------------------------------------------------------------- |
| AmiLaunchIndex                          | `Integer` | The AMI launch index.                                                                   |
| Architecture                            | `String`  | The architecture of the image.                                                          |
| BlockDeviceMappings                     | `List`    | Any block device mapping entries for the instance.                                      |
| CapacityReservationId                   | `String`  | The ID of the Capacity Reservation.                                                     |
| CapacityReservationSpecification        | `Map`     | Information about the Capacity Reservation targeting option.                            |
| ClientToken                             | `String`  | The provided token when the instance launched.                                          |
| CpuOptions                              | `Map`     | The CPU options for the instance.                                                       |
| EbsOptimized                            | `Boolean` | Whether the instance is optimized for Amazon EBS I/O.                                   |
| ElasticGpuAssociations                  | `List`    | The Elastic GPU associated with the instance.                                           |
| ElasticInferenceAcceleratorAssociations | `List`    | The elastic inference accelerator associated with the instance.                         |
| EnaSupport                              | `Boolean` | Whether enhanced networking with ENA is enabled.                                        |
| HibernationOptions                      | `Map`     | Whether the instance is enabled for Amazon Web Services Nitro Enclaves.                 |
| Hypervisor                              | `String`  | The hypervisor type of the instance.                                                    |
| IamInstanceProfile                      | `Map`     | The IAM instance profile associated with the instance.                                  |
| ImageId                                 | `String`  | The ID of the AMI used to launch the instance.                                          |
| InstanceLifecycle                       | `String`  | Whether this is a Spot Instance or a Scheduled Instance.                                |
| InstanceType                            | `String`  | The instance type.                                                                      |
| KernelId                                | `String`  | The kernel associated with this instance.                                               |
| KeyName                                 | `String`  | The name of the key pair, if this instance was launched with an associated key pair.    |
| Licenses                                | `String`  | The license configurations for the instance.                                            |
| MetadataOptions                         | `List`    | The metadata options for the instance.                                                  |
| Monitoring                              | `Map`     | The monitoring for the instance.                                                        |
| NetworkInterfaces                       | `List`    |                                                                                         |
| Placement                               | `Map`     | The location where the instance launched, if applicable.                                |
| Platform                                | `String`  | The instance operating system.                                                          |
| PrivateDnsName                          | `String`  | The private DNS hostname name assigned to the instance.                                 |
| PrivateIpAddress                        | `String`  | The private IPv4 address assigned to the instance.                                      |
| ProductCodes                            | `List`    | The product codes attached to this instance.                                            |
| PublicDnsName                           | `String`  | The public DNS name assigned to the instance.                                           |
| PublicIpAddress                         | `String`  | The public IP address.                                                                  |
| RamdiskId                               | `String`  | The RAM disk associated with this instance.                                             |
| RootDeviceName                          | `String`  | The device name of the root device volume.                                              |
| RootDeviceType                          | `String`  | The root device type used by the AMI.                                                   |
| SecurityGroups                          | `List`    | The security groups for the instance.                                                   |
| SourceDestCheck                         | `Boolean` | Whether source/destination checking is enabled.                                         |
| SpotInstanceRequestId                   | `String`  | The ID of the Spot Instance request.                                                    |
| SriovNetSupport                         | `String`  | Whether enhanced networking with the Intel 82599 Virtual Function interface is enabled. |
| State                                   | `Map`     | The current state of the instance.                                                      |
| StateReason                             | `Map`     | The reason for the most recent state transition.                                        |
| StateTransitionReason                   | `String`  | The reason for the most recent state transition.                                        |
| SubnetId                                | `String`  | The ID of the subnet in which the instance is running.                                  |
| VirtualizationType                      | `String`  | The virtualization type of the instance.                                                |
| VpcId                                   | `String`  | The ID of the VPC in which the instance is running.                                     |

## Example

```javascript
{
    "AccountId": "123456789012",
    "AmiLaunchIndex": 0,
    "Architecture": "x86_64",
    "Arn": "arn:aws:ec2:us-west-2:123456789012:instance/i-1",
    "BlockDeviceMappings": [
        {
            "DeviceName": "/dev/xvda",
            "Ebs": {
                "AttachTime": "2019-01-01T00:00:00Z",
                "DeleteOnTermination": true,
                "Status": "attached",
                "VolumeId": "vol-1"
            }
        }
    ],
    "CapacityReservationId": null,
    "CapacityReservationSpecification": {
        "CapacityReservationPreference": "open",
        "CapacityReservationTarget": null
    },
    "ClientToken": null,
    "CpuOptions": {
        "CoreCount": 1,
        "ThreadsPerCore": 1
    },
    "EbsOptimized": false,
    "ElasticGpuAssociations": null,
    "ElasticInferenceAcceleratorAssociations": null,
    "EnaSupport": true,
    "HibernationOptions": {
        "Configured": false
    },
    "Hypervisor": "xen",
    "IamInstanceProfile": null,
    "Id": "i-1",
    "ImageId": "ami-1",
    "InstanceLifecycle": null,
    "InstanceType": "t2.micro",
    "KernelId": null,
    "KeyName": "ec2-instance-key-pair",
    "Licenses": null,
    "Monitoring": {
        "State": "disabled"
    },
    "NetworkInterfaces": [
        {
            "Association": {
                "IpOwnerId": "amazon",
                "PublicDnsName": "ec2-111-111-111-111.us-west-2.compute.amazonaws.com",
                "PublicIp": "111.111.111.111"
            },
            "Attachment": {
                "AttachTime": "2019-01-01T00:00:00Z",
                "AttachmentId": "eni-attach-1",
                "DeleteOnTermination": true,
                "DeviceIndex": 0,
                "Status": "attached"
            },
            "Description": null,
            "Groups": [
                {
                    "GroupId": "sg-1",
                    "GroupName": "launch-wizard-1"
                }
            ],
            "InterfaceType": "interface",
            "Ipv6Addresses": null,
            "MacAddress": "00:00:de:ad:be:ef",
            "NetworkInterfaceId": "eni-1",
            "OwnerId": "123456789012",
            "PrivateDnsName": "ip-10-10-10-10.us-west-2.compute.internal",
            "PrivateIpAddress": "10.10.10.10",
            "PrivateIpAddresses": [
                {
                    "Association": {
                        "IpOwnerId": "amazon",
                        "PublicDnsName": "ec2-111-111-111-111.us-west-2.compute.amazonaws.com",
                        "PublicIp": "111.111.111.111"
                    },
                    "Primary": true,
                    "PrivateDnsName": "ip-10-10-10-10.us-west-2.compute.internal",
                    "PrivateIpAddress": "10.10.10.10"
                }
            ],
            "SourceDestCheck": true,
            "Status": "in-use",
            "SubnetId": "subnet-1",
            "VpcId": "vpc-1"
        }
    ],
    "Placement": {
        "Affinity": null,
        "AvailabilityZone": "us-west-2b",
        "GroupName": null,
        "HostId": null,
        "PartitionNumber": null,
        "SpreadDomain": null,
        "Tenancy": "default"
    },
    "Platform": null,
    "PrivateDnsName": "ip-10-10-10-10.us-west-2.compute.internal",
    "PrivateIpAddress": "10.10.10.10",
    "ProductCodes": null,
    "PublicDnsName": "ec2-111-111-111-111.us-west-2.compute.amazonaws.com",
    "PublicIpAddress": "111.111.111.111",
    "RamdiskId": null,
    "Region": "us-west-2",
    "ResourceId": "arn:aws:ec2:us-west-2:123456789012:instance/i-1",
    "ResourceType": "AWS.EC2.Instance",
    "RootDeviceName": "/dev/xvda",
    "RootDeviceType": "ebs",
    "SecurityGroups": [
        {
            "GroupId": "sg-1",
            "GroupName": "launch-wizard-1"
        }
    ],
    "SourceDestCheck": true,
    "SpotInstanceRequestId": null,
    "SriovNetSupport": null,
    "State": {
        "Code": 16,
        "Name": "running"
    },
    "StateReason": null,
    "StateTransitionReason": null,
    "SubnetId": "subnet-1",
    "Tags": {
        "Key1": "Value1"
    },
    "TimeCreated": "2019-01-01T00:00:00.000Z",
    "VirtualizationType": "hvm",
    "VpcId": "vpc-1"
}
```
