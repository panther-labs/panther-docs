# Getting Started with Fluentd

## Overview

This guide is aimed to help you quickly set up the necessary AWS resources that can be used to onboard data from various utilities and sources like Fluentd, Syslog, Windows Events, GCP Audit logs, and more.

Fluentd supports Firehose and S3 destination plugins. We have provided sample CloudFormation templates below that can be customized to fit your environment.

Once the template has been deployed and the resources have been created, return to the log source guide to continue configuring the log source.

### Firehose to S3 Template \(Recommended\) <a id="Firehose-to-S3-Template-(Recommended)"></a>

{% hint style="info" %}
The Fluentd Firehose plugin is generally more performant than the Fluentd S3 plugin
{% endhint %}

![](../../.gitbook/assets/image%20%2824%29.png)

#### **Resources**:

This template creates a Kinesis Firehose resource and an S3 bucket, and configures permissions to write to the Firehose stream, the Firehose stream to send its logs to S3, and permissions for Firehose to write to the S3 bucket.

#### **Pipeline:**

After deploying the template, save the outputs for use in the Fluentd configurations.

The outputs of this template are:

* InstanceProfileName - The profile that can be used to assume role with correct permissions
* S3Bucket - The S3bucket that firehose will send events to
* FirehoseSendDataRoleArn - Arn of the role to write to Firehose
* FirehoseName - The firehose stream name

The template can be found here: [CloudFormation Template](https://github.com/panther-labs/panther-auxiliary/blob/main/cloudformation/panther-fluentd-firehose.yml).

### S3 Template <a id="S3-Template"></a>

{% hint style="warning" %}
As mentioned above, this template is less performant than the Firehose template and is not recommended unless necessary
{% endhint %}

![](../../.gitbook/assets/image%20%2825%29.png)

### **Resources**:

This template creates an S3 bucket and permissions to write to the created bucket.

### **Pipeline:**

After deploying the template, you must create an access token in IAM for the user that was created.

The outputs of this template are:

* FluentdUser - IAM user to be used within the Fluentd configuration
* S3Bucket - The bucket that was created to use within the Fluentd configuration

The template can be found here: [CloudFormation Template](https://github.com/panther-labs/panther-auxiliary/blob/main/cloudformation/panther-fluentd-s3.yml).

