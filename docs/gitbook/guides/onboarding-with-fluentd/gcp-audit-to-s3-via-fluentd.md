# GCP Audit to S3 via Fluentd

## Objectives <a href="#objectives" id="objectives"></a>

The objective of this recipe is to stream Google Cloud Project Audit Logs into Panther. Many businesses use several cloud providers, and these steps will allow teams to gather API calls that happen with a GCP account into Panther.

\
We’ll implement this using a combination of primitives across GCP and AWS.

## Solution Brief <a href="#solution-brief" id="solution-brief"></a>

At a high level, we’ll be implementing the following flow:

1. Audit logs are generated in GCP and routed to PubSub
2. Fluentd polls PubSub and forwards to an S3 Bucket
3. The S3 bucket is onboarded into Panther for normalization, detection, and long-term storage

## Steps <a href="#steps" id="steps"></a>

### Step 1: Create a New Pub/Sub <a href="#step-1-create-a-new-pub-sub" id="step-1-create-a-new-pub-sub"></a>

![Pub/Sub console](../../.gitbook/assets/pub-sub\_console.png)

* In GCP, open the Pub/Sub console
* Create a new Topic, call it Panther-Audit
  * Uncheck ‘Add a default subscription’&#x20;
  * Select CREATE TOPIC
* Click Subscriptions > Create subscription
  * Input Panther-Audit as the Subscription ID
  * Select the Panther-Audit topic
  * Leave all other options or tune the expiration/retention as needed (per your intended spending/budgeting)
  * Click CREATE

\
Write down the Topic name (projects/\<project-name>/topics/Panther-Audit) and Topic subscription (Panther-Audit); we’ll use it later!

### Step 2: Create a Logs Router <a href="#step-2-create-a-logs-router" id="step-2-create-a-logs-router"></a>

![Logging Console](../../.gitbook/assets/logging\_console.png)

* Open the Logging console
* Click Logs Router
* Click CREATE SINK
* Set the name to Panther-Audit
* Set the Sink Destination
  * Cloud Pub/Sub topic
  * Select the Panther-Audit Topic
* Click CREATE SINK

You can validate this pipeline is working by going to Pub/Sub, clicking the Topic ID of Panther-Audit, and viewing the ACTIVITY to see Audit events.

![Activity](../../.gitbook/assets/pub-sub\_activity.png)

### Step 3: Create a Service Account <a href="#hardbreak-step-3-create-a-service-account" id="hardbreak-step-3-create-a-service-account"></a>

* Open IAM & Admin
* Click Service Accounts
* Click CREATE SERVICE ACCOUNT
  * Set the Service account name to Panther-Audit. Add a description if you like.
  * Click Create and Continue
  * Under **Grant this service account access to project**, set the service account access role to _Pub/Sub Viewer_ and _Pub/Sub Subscriber_
  * Click Continue
  * Click Done
* Under Service accounts -> Actions, click Manage keys, ADD KEY, Create new key, select JSON, and hit CREATE to download your credentials.
* Keep this credential file safe! We’ll use it soon.

![Service Account Actions](../../.gitbook/assets/service\_account\_actions.png)

### Step 4: Configure AWS Infrastructure  <a href="#step-4-configure-aws-infrastructure" id="step-4-configure-aws-infrastructure"></a>

On the [Getting Started with Fluentd](resource-guide.md) page, review and deploy the **Firehose & S3** stack

### Step 5: Launch Your Instance in AWS <a href="#step-5-launch-your-instance-in-aws" id="step-5-launch-your-instance-in-aws"></a>

* Open the AWS EC2 Console (in the same region where you launched the stack above) and launch an Ubuntu Instance.
  * Click Launch Instance
  * Select **Ubuntu Server 20.04 LTS**
  * Select **t2.medium** (or a more powerful instance type, if you’d like)
  * In the IAM Role section, select the value of the **InstanceProfileName** copied in Step 4, with the format “\<stack-name>-FirehoseInstanceProfile-\<random-string>”
  * Click Add Storage, and add a 64GiB capacity drive
  * Set your Security Group, Key Pair, and other preferences as you’d like
  * Click Launch

### Step 6: Install and Configure Fluentd <a href="#step-6-install-and-configure-fluentd" id="step-6-install-and-configure-fluentd"></a>

* Add your keypair to your ssh agent
  * `ssh-add <path-to-keypair>`
* SCP your GCP credentials downloaded in Step 3 to the instance
  * `scp <path-to-gcp-cred-file> ubuntu@<public-ip>:/home/ubuntu/`&#x20;
* SSH to your newly launched EC2 instance
  * `ssh ubuntu@<public-ip>`
* [Install Fluentd for Ubuntu](https://docs.fluentd.org/installation/install-by-deb)
  * Follow the instructions for Ubuntu Focal
* Install the [official](https://github.com/awslabs/aws-fluent-plugin-kinesis) AWS Kinesis plugin
  * `sudo td-agent-gem install fluent-plugin-kinesis`
* Install the GCP plugin
  * `sudo td-agent-gem install fluent-plugin-gcloud-pubsub-custom`

Overwrite the default fluentd config in `/etc/td-agent/td-agent.conf`:

```
<system>
  log_level debug
</system>

<source>
  @type gcloud_pubsub
  tag gcp.audit
  project <YOUR-GCP-PROJECT-ID>
  key <PATH-TO-YOUR-KEYPAIR>
  topic <PANTHER-AUDIT-TOPIC-ID>
  subscription <PANTHER-AUDIT-SUBSCRIPTION-ID>
  max_messages 1000
  return_immediately true
  pull_interval 1
  pull_threads 2
  parse_error_action exception
  <parse>
    @type json
  </parse>
</source>

<match gcp.**>
  @type kinesis_firehose
  region <YOUR-FIREHOSE-REGION>
  delivery_stream_name <YOUR-FIREHOSE-NAME>

  <assume_role_credentials>
    duration_seconds 3600
    role_arn <YOUR-FIREHOSE-ROLE-ARN>
    role_session_name "#{Socket.gethostname}-panther-audit"
  </assume_role_credentials>
</match>
```

* Restart `td-agent`
  * `sudo systemctl restart td-agent`

### Step 7: Onboard data into Panther <a href="#step-7-onboard-data-into-panther" id="step-7-onboard-data-into-panther"></a>

Since [GCP audit logs](https://docs.runpanther.io/data-onboarding/supported-logs/gcp) are supported out of the box, you may configure the S3 bucket as a [data transport](https://docs.runpanther.io/data-onboarding/data-transports/s3) to start ingesting the logs through Panther.

## **Troubleshooting**

* Note: logs may take \~5 minutes to show up in the S3 bucket because of the `IntervalInSeconds`and `SizeInMBs` parameters within the CloudFormation template.
* Monitor the td-agent logs for any errors
  * `sudo tail -f /var/log/td-agent/td-agent.log`
* If you need more verbose logging, run:
  * `td-agent -vv`
