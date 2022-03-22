# Self Hosted Deployments

In most cases, Panther strongly recommends using the SaaS deployment model. In some rare cases though self-hosted makes the most sense. If you're preparing a self-hosted deployment, please follow the steps on this page to make sure the deployment goes as smoothly as possible.

### Before you deploy

#### Have Panther approve your account

Before you can deploy Panther, you will need to provide Panther with the **account ID** and **region** of the account from which you intend to deploy Panther. We maintain [a list of supported regions](https://docs.runpanther.io/help/operations/architecture#supported-aws-regions) in our documentation.

#### Check IAM Permissions

Deploying Panther into your own account requires creating a number of AWS resources, including the provisioning of IAM resources. To ensure you have adequate permissions to perform those actions, make sure you're using a privileged user or role when creating the panther root stack.

We provide a `PantherDeploymentRole` template that creates an IAM role with relatively least privilege access configured for deploying Panther. Note that this role has the ability to create arbitrary IAM entities, so privilege escalation is trivial. Panther needs this permission to create the least-privilege roles used by the Panther application itself, but the `PantherDeploymentRole` should be treated as a sensitive administrator role.

#### Check organization SCPs

Panther creates and tags a number of AWS resources when deploying Panther. Organizations with restrictive organization Service Control Policies (SCPs) may find their Panther deployment fails partway through unexpectedly with an access denied error. Common causes for this are SCPs that restrict the creation of VPCs and other network resources, SCPs that restrict tagging resources, SCPs that restrict specific S3 API calls like `s3:SetBucketEncryption`, and SCPs relating to the KMS service.

Before deploying Panther, do a review of your organization SCPs for anything that might block the deployment of Panther. Key AWS service restrictions to look out for are S3, EC2/VPC, KMS, Lambda, DynamoDB, KMS, ECS, SQS, and SNS. If you're not sure about a particular SCP,  feel free to reach out to your Panther rep and ask.

#### Remove old Panther infrastructure

Often a self-hosted deployment starts after a self-hosted POC. Be sure that any Panther infrastructure (including auxiliary infra like the `PantherAuditRole`) has been removed from the account before deploying Panther, as namespace conflicts may cause deployments to fail.

### While you deploy

#### Naming the root stack

When deploying Panther, you will be provided with a template URL to a root panther stack to deploy. If you're using the `PantherDeploymentRole` to deploy Panther, be sure to name the root stack something with a `panther-` prefix. The name of the root stack will be pre-pended to any resources created by the stack, and the `PantherDeploymentRole` limits its access in part by restricting its permissions to only affect resources that start with the name `panther-`.

#### Configure deployment parameters

The Panther CloudFormation stack has a number of configurable deployment parameters. Pay special attention to the following options:

* `FirstUserEmail` _**(required)**_: a Panther admin invite will be sent to this email address. Updates to this value are ignored after the first successful deploy.
* `PulumiSecretArn` and `PulumiSecretKeyArn` **(**_**required**_**):** these values will be provided by our team - you will have a dedicated [Pulumi](https://www.pulumi.com) access token in our organization.
* `OnboardSelf`: whether you want Panther to onboard its own AWS account for monitoring.
* `SentryEnvironment`: by default, application errors are sent to [Sentry](https://sentry.io) for us to triage. We strongly recommend keeping this enabled with the default value (`prod`), but if that's not an option for you, you can disable the Sentry integration by setting this to a blank string.
* `SupportRoleIdentityAccountId`: by default, a read-only SupportRole is deployed with Panther which our on-call engineers can assume to triage application errors. This role does **not** have access to your data and we’d encourage you to keep it enabled so we can deliver a better support experience. However, if you prefer, this role can be disabled by setting the `SupportRoleIdentityAccountId` to a blank string.
* `OpsRoleIdentityAccountId`: a non-empty value will deploy an OperationsRole with service-level admin permissions for migrations, data recoveries, and other operational emergencies. We recommend keeping this role disabled until necessary (it's off by default).
* `DataLakeForwarderMemory`: Memory to use for Cloud Security DataLake Forwarder lambdas. The default setting is 256, with a maximum value of 2048 and a minimum value of 256.
* `LogProcessorGzipLevel`: Gzip compression level to write logs. The default setting is 6, with a maximum value of 9 and a minimum value of 1.
* `MaxLookupTableCompressedSizeMB`: The maximum size (in MB) of the Gzip-compressed data backing a Lookup Table. The default setting is 200, with a  maximum value of 400.

#### Minimize initial configurations

Panther has a number of other configuration options besides the ones listed above. We recommend not setting any of these parameters on the first deployment of Panther. If any step of the initial deployment fails, the entire deployment will fail and rollback deleting all infrastructure. After you complete the initial deployment of Panther, you can update the stack with different root parameters. Then if any of these settings cause a deployment failure, Panther will simply roll back to the previous settings without needing an entire fresh deployment. This includes parameters like the snowflake and custom domain configuration parameters.

#### Trigger Pulumi CodeBuild

We are slowly migrating to Pulumi for infrastructure management - after the main Panther stack is deployed, you'll need to start a build for the `panther-pulumi` CodeBuild project (in v1.22+). For example: `aws codebuild start-build --project-name panther-pulumi`

### If a deployment fails

#### Find the failed stack

When the initial deployment fails, all resources will be deleted besides the root stack. To start troubleshooting the cause of the deployment failure, go to the root stack and find which nested resource failed first. That will tell you which stack caused the failure. You can then find this stack by searching for it in the CloudFormation console. Since the stack has been deleted, you will need to filter for deleted stacks when searching.

#### Find the stack failure reason

Once you've navigated to the stack that caused the deployment failure, go to the events tab in the CloudFormation console and scroll down to the first error you can find. This will show you why the deployment failed. You can either begin troubleshooting yourself or share this info with your Panther rep and we can assist the troubleshooting process.

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

**SAMPLE**

****[https://panther-enterprise-us-east-2.s3.amazonaws.com/v1.25.1/panther.yml](https://panther-enterprise-{region}.s3.amazonaws.com/%7Bversion%7D/panther.yml)

```
https://panther-enterprise-{region}.s3.amazonaws.com/{version}/panther.yml
```

You will be prompted to click through a few pages verifying your CloudFormation parameters are correct and that CloudFormation can create IAM resources and nested CloudFormation resources on your behalf.

#### Available versions of Panther

We recommend not skipping minor versions of Panther while upgrading, but upgrading to the most recent patch version instead. Here are the most recent patch versions of Panther that we recommend upgrading to:

* `v1.31.1`
* `1.30.5`
* `v1.29.3`
* `v1.28.7`
* `v1.27.8`
* `v1.26.9`
* `v1.25.18`
* `v1.24.12`
* `v1.23.5`
* `v1.22.7`
* `v1.21.5`
* `v1.20.3`
* `v1.19.5`
* `v1.18.8`
* `v1.17.4`

### Migration Notes

In addition to upgrading the `PantherDeploymentRole`, some upgrades may require modifications to the CloudFormation parameters.

#### Migrating from 1.17.x -> 1.18.x:

Change the `PipLibraries` parameter to remove the following libraries as they are now included by default (you may keep any 3rd party libraries):

* `jsonpath-ng`
* `policyuniverse`
* `requests`

**Migrating to 1.19.5, from 1.19.x -> 1.20.x or from 1.20.x -> 1.21x:**

From 1.19.5 and on, we have attached a custom domain to our GraphQL API and require an additional certificate for it. We've also updated our deployment role in order to add the necessary permissions to support an edge-optimized custom domain on API Gateway. The changes can be summed up to:

1. Pull our latest Cloudformation template and update your Panther Deployment Role (if you're using an Administrative role to deploy Panther, then this step can be omitted).
2. Create a certificate for the the _**api.**_** ** subdomain of your existing panther domain in **us-east-1** (e.g. if your panther domain is [panther.mydomain.com](http://api.mydomain.com), then you should create a certificate for [api.panther.mydomain.com](http://api.panther.mydomain.com)), add the necessary CNAME in order to validate it and wait until validation is complete.
3. Use its ARN value in the "ApiCertificateArn" CloudFormation parameter of Panther's template and update your Panther deployment.
4.  Create an "A Alias Record"  for the api subdomain you've created above. You can find the Alias Target and the Alias Hosted zone by inspecting the outputs of the the "Bootstrap" CloudFormation stack that is part of your Panther deployment. The values you'll be needing are:

    ```
    PantherApiDistributionDomainName
    PantherApiDistributionHostedZoneId
    ```

    To add this "A Alias Record ", you'll need to use Cloudformation. The AWS Console is not an option since this Cloudfront Distribution is internal to AWS and is not going to be exposed in the Console's Distributions dropdown. The stack you'll need to deploy is the following:

    ```
    AWSTemplateFormatVersion: 2010-09-09
    Parameters:
      PantherDomainName:
        Type: String
        Description: The domain name which Panther runs in (i.e. `panther.mydomain.com`).
      PantherHostedZoneName:
        Type: String
        Description: The name of the hosted zone of your Panther domain in Route53 (i.e. `mydomain.com`).
      PantherApiDistributionHostedZoneId:
        Type: String
        Description: The hosted zone ID of the cloudfront distribution of the custom API GW domain
      PantherApiDistributionDomainName:
        Type: String
        Description: The domain name of the cloudfront distribution of the custom API GW domain

    Resources:
      PantherApiSubdomain:
        Type: AWS::Route53::RecordSet
        Properties:
          Comment: Alias used for customer panther api
          HostedZoneName: !Sub "${PantherHostedZoneName}."
          Name: !Sub "api.${PantherDomainName}."
          Type: A
          AliasTarget:
            HostedZoneId: !Ref PantherApiDistributionHostedZoneId
            DNSName: !Ref PantherApiDistributionDomainName
            EvaluateTargetHealth: false
    ```
