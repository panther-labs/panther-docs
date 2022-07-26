---
description: >-
  Onboarding Google Cloud Storage (GCS) as a Data Transport log source in the
  Panther Console
---

# Google Cloud Storage (GCS) Source

## Overview

With Google Cloud Storage (GCS) as a log source, Panther can pull log data directly from GCS buckets, write rules, and run queries on this processed data.

### Prerequisites

Panther requires certain configurations within Google Cloud Platform (GCP) to authenticate and pull logs. A bucket and a subscription for a topic set up with notifications are required. Panther ingests new files through [Pub/Sub notifications](https://cloud.google.com/pubsub).&#x20;

## How to connect GCS as a Data Transport log source in Panther

1. In the Panther Console, click **Integrations > Log Sources**.
2. Click **Create New**.
3. On the left side of the page, click **Custom Onboarding**.&#x20;
4. On the Google Cloud Storage tile, click **Select**.\
   ![](<../../.gitbook/assets/gcs no beta.png>)
5. On the "Configure your source" page, fill in the fields:
   * **Name**: Enter a memorable name for the GCS log source.
   * **Log Types**: Select the Log Types Panther should use to parse your GCS logs. **Note:** At least one Log Type must be selected from the dropdown menu.\
     ![](<../../.gitbook/assets/Screen Shot 2022-01-26 at 11.50.45 AM.png>)
6. Click **Continue Setup.**
7. On the "Infrastructure & Credentials" page, follow Steps 1 and 2 to create the infrastructure component with a Terraform template. **Note:** You can also [follow our alternative documentation](gcs.md#configuring-the-integration-in-google-cloud-platform-gcp) to complete the infrastructure components process manually.
   * Step 1: Download and complete the Terraform template
     * Download the **Terraform Template.**
     * Fill out the fields in the `production.tfvars` file with your configuration.
     * Initialize a working directory containing Terraform configuration files by running the Terraform Command schema provided.
     * Copy the corresponding **Terraform of gcloud command schema** provided and run it in your CLI.
     * Generate a JSON keyfile by replacing the value for your service account email\
       in the gcloud command code listed.
       * You can find the key file in the output of the Terraform run.
   * Step 2: Provide pulling configuration & JSON Keyfile
     * Drag and drop or upload the JSON key into the correct field in Step 2.
     * Paste in your **GCS Bucket Nam**e and **Pub/Sub Subscription ID,** found in the **Subscriptions** section of your Google Cloud account.\
       \
       ![](<../../.gitbook/assets/Infrastructure and credentials page.png>)\

8. Click **Continue Setup**.&#x20;
9. Click **Finish Setup**.

#### Configuring your integration manually in Google Cloud Platform (GCP)&#x20;

If you choose to create the infrastructure components manually rather than using a Terraform template during the GCS setup above, follow the instructions below.

1. Log in to your Google Cloud console.
2. Determine which bucket Panther will pull logs from.
   * If you have not created a bucket yet, please see [Google's documentation on creating a bucket](https://cloud.google.com/storage/docs/creating-buckets).
3. [Create a topic](https://cloud.google.com/pubsub/docs/admin#creating\_a\_topic) for the notifications.
   * You can create a topic using the `gcloud` CLI tool with the following command format: \
     `gcloud pubsub topics create $TOPIC_ID`
4. [Configure the bucket to send notifications](https://cloud.google.com/storage/docs/reporting-changes) for new files to the topic you created.&#x20;
   * You can create a notification using the `gcloud` CLI tool with the following command format:\
     `gsutil notification create -t $TOPIC_NAME -e OBJECT_FINALIZE -f json gs://$BUCKET_NAME`
   * **Note:** Panther only requires the `OBJECT_FINALIZE` type.
5. [Create a subscription](https://cloud.google.com/pubsub/docs/admin#pubsub\_create\_pull\_subscription-gcloud) to be used with the topic you created. **Note:** This subscription should not be used by any service other than Panther.
   * You can create a subscription using the `gcloud` CLI tool with the following command format:\
     `gcloud pubsub subscriptions create $SUBSCRIPTION_ID --topic $TOPIC_ID --topic-project $PROJECT_ID`
6. [Create a new Google Cloud service account](https://cloud.google.com/iam/docs/creating-managing-service-accounts) and take note of the account email address. Panther will use this to be able to access the infrastructure created for this GCS integration.&#x20;
   *   The following permissions are required for the project where the Pub/Sub subscription and topic lives:\


       |                             **Permissions required**                            |       **Role**      |      **Scope**      |
       | :-----------------------------------------------------------------------------: | :-----------------: | :-----------------: |
       | <p><code>storage.objects.get</code></p><p><code>storage.objects.list</code></p> |   `storage/viewer`  |    _bucket-name_    |
       |                          `pubsub.subscriptions.consume`                         | `pubsub/subscriber` | _subscription-name_ |
       |                            `pubsub.subscriptions.get`                           |   `pubsub/viewer`   | _subscription-name_ |
       |                           `monitoring.timeSeries.list`                          | `monitoring/viewer` |       project       |
   * **Note:** You can set conditions or IAM policies on permissions for specific resources. This can be done either in the IAM page of the service account (as seen in the example screenshot below) or in the specific resource's page.\
     ![](../../.gitbook/assets/gcp-grant-access.png)
7. [Generate a JSON key file](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) for the service account, which will be used in Panther to authenticate to the GCP infrastructure.&#x20;
   * You can create a JSON key file using the gcloud CLI tool with the following command format: \
     `gcloud iam service-accounts keys create $KEYFILE_PATH --iam-account=$SERVICE_ACCOUNT_EMAIL`

## View collected logs

After GCS log sources are fully configured, you can search your data in Data Explorer. For more information and for example queries, please see the documentation on [Data Explorer](../../data-analytics/data-explorer.md).

