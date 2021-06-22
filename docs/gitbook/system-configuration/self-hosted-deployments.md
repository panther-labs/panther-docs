# Self Hosted Deployments

In most cases, Panther strongly recommends using the SaaS deployment model. In some rare cases though self-hosted makes the most sense. If you're preparing a self-hosted deployment, please follow the steps on this page to make sure the deployment goes as smoothly as possible.

### Before you deploy

#### Have Panther approve your account

Before you can deploy Panther, you will need to provide Panther with the account ID of the account from which you intend to deploy Panther.

#### Check IAM Permissions

Deploying Panther into your own account requires creating a number of AWS resources, including the provisioning of IAM resources. To ensure you have adequate permissions to perform those actions, make sure you're using a privileged user or role when creating the panther root stack.

We provide a `PantherDeploymentRole` template [here](https://github.com/panther-labs/panther/blob/master/deployments/auxiliary/cloudformation/panther-deployment-role.yml) that creates an IAM role with relatively least privilege access configured for deploying Panther. Note that this role has the ability to create arbitrary IAM entities, so privilege escalation is trivial. Panther needs this permission to create the least-privilege roles used by the Panther application itself, but the `PantherDeploymentRole` should be treated as a sensitive administrator role.

#### Check organization SCPs

Panther creates and tags a number of AWS resources when deploying Panther. Organizations with restrictive organization Service Control Policies \(SCPs\) may find their Panther deployment fails partway through unexpectedly with an access denied error. Common causes for this are SCPs that restrict the creation of VPCs and other network resources, SCPs that restrict tagging resources, SCPs that restrict specific S3 API calls like `s3:SetBucketEncryption`, and SCPs relating to the KMS service.

Before deploying Panther, do a review of your organization SCPs for anything that might block the deployment of Panther. Key AWS service restrictions to look out for are S3, EC2/VPC, KMS, Lambda, DynamoDB, KMS, ECS, SQS, and SNS. If you're not sure about a particular SCP,  feel free to reach out to your Panther rep and ask.

#### Remove old Panther infrastructure

Often a self-hosted deployment starts after a self-hosted POC. Be sure that any Panther infrastructure \(including auxiliary infra like the `PantherAuditRole`\) has been removed from the account before deploying Panther, as namespace conflicts may cause deployments to fail.

### While you deploy

#### Naming the root stack

When deploying Panther, you will be provided with a template URL to a root panther stack to deploy. If you're using the `PantherDeploymentRole` to deploy Panther, be sure to name the root stack something with a `panther-` prefix. The name of the root stack will be pre-pended to any resources created by the stack, and the `PantherDeploymentRole` limits its access in part by restricting its permissions to only affect resources that start with the name `panther-`.

#### Configure deployment parameters

The root stack has a number of deployment parameters you can configure to control how Panther is deployed. We recommend changing at least these parameters:

* `FirstUserEmail`:  the email address provided here will be used to send the invite to the first user of Panther. Be sure its an email you have access to.
* `OnboardSelf`: this parameter defaults to `true`, but for most self hosted customers we recommend setting this parameter to `false` for at least the first deployment.

#### Minimize initial configurations

Panther has a number of other configuration options besides the ones listed above. We recommend not setting any of these parameters on the first deployment of Panther. If any step of the initial deployment fails, the entire deployment will fail and rollback deleting all infrastructure. After you complete the initial deployment of Panther, you can update the stack with different root parameters. Then if any of these settings cause a deployment failure, Panther will simply roll back to the previous settings without needing an entire fresh deployment. This includes parameters like the snowflake and custom domain configuration parameters.

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

```text
https://panther-public-cloudformation-templates.s3.amazonaws.com/panther-deployment-role/{version}/template.yml
```

#### While you upgrade

In order to perform the upgrade, simply find the root Panther stack in the CloudFormation console, click the `Update` button, select `Replace template URL`, and insert the `TemplateURL` for the desired version of Panther you wish to deploy. The  template URL should be in this format:

```
https://panther-enterprise-{region}.s3.amazonaws.com/{version}/panther.yml
```

You will be prompted to click through a few pages verifying your CloudFormation parameters are correct and that CloudFormation can create IAM resources and nested CloudFormation resources on your behalf.

#### Available versions of Panther

We recommend not skipping minor versions of Panther while upgrading, but upgrading to the most recent patch version instead. Here are the most recent patch versions of Panther that we recommend upgrading to:

* `v1.18.4`
* `v1.17.4`
* `v1.16.4`
* `v1.15.3`
* `v1.14.3`
* `v1.13.0`
* `v1.12.1`
* `v1.11.0`
* `v1.10.1`

If you plan to upgrade from a version older than `v1.10.X`,  please contact  Panther support first for guidance.

### Migration Notes

In addition to upgrading the `PantherDeploymentRole`, some upgrades may require modifications to the CloudFormation parameters.

#### Migraging from 1.17.x -&gt; 1.18.x:

Change the `PipLibraries` parameter to remove the following libraries as they are now included by default \(you may keep any 3rd party libraries\):

* `jsonpath-ng`
* `policyuniverse`
* `requests`



#### 





