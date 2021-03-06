---
description: Use Packs to group detections and enable updates via the Panther Console
---

# Detection Packs

## Overview

Packs are used to logically group detections as well as enable detection updates via the Panther Console. Panther-provided packs are defined in this open source repository: [`panther-labs/panther-analysis`](https://github.com/panther-labs/panther-analysis).

A single pack can group any number of Detections, Queries, Global Helpers, Data Models, and Lookup Tables. For example, one of the provided Detections Packs, `Panther Universal Detections`, groups all the rules that rely on Data Models and all of their dependencies.

Updates to detections in these Packs are tracked automatically by the Panther backend. When a new update for a Detection Pack is available in the `panther-analysis` repository, the Packs page in your Panther Console under **Detections > Packs** will display an _Update available_ flag next to the relevant items.

Detections that are part of an enabled Detection Pack will be labeled as `MANAGED`, and detections that are not part of a Detection Pack will be labeled as `UNMANAGED`.

{% hint style="warning" %}
Note: Managing Detections via the Panther Console is **not** recommended if you are already using a Git-Based workflow to manage and upload detections with Panther Analysis Tool. Managing detections via both methods simultaneously may result in unexpected behavior**.**&#x20;
{% endhint %}

### Panther Built-In Detection Packs

Panther provides several Detection Packs by default. There are packs that group all of the Panther-provided detections related to a particular log source, as well as Detection Packs that are grouped on a particular focus (e.g., generic rules that leverage unified data models or a core set of detections for AWS.) Some popular examples include:

| **Display Name**      | **Description**                                                                       |
| --------------------- | ------------------------------------------------------------------------------------- |
| Universal Detections  | This pack groups the standard rules that leverage unified data models                 |
| Panther Core AWS Pack | Group of the most critical and high value detections pertinent to the AWS environment |
| Panther Okta Pack     | Group of all Panther created detections for Okta                                      |

## How to use Detection Packs

### Viewing Detection Packs

You can view a list of Panther-provided Detection Packs in your Panther Console under **Detections > Packs**.

![List Packs](../.gitbook/assets/pack-list.png)

Click on a Pack to view its details, including a description, the enabled status, the currently enabled version, and which detections are in the pack.&#x20;

![Pack Details](../.gitbook/assets/pack-details.png)



### Enabling and disabling Detection Pack

Packs can be disabled or enabled in your Panther Console. If you enable a pack, all the detections in the pack will be enabled. If you would like to disable a single or multiple detections within it, you can do this on a one-by-one basis without having to disable the entire pack. When you update a pack that has disabled detections, the detections will be updated but they will stay disabled.

To enable or disable a Pack:

1. Log in to your Panther Console.
2. Navigate to **Detections > Packs**.
3. Toggle the **Enabled** slider on or off.

![Enable Pack From List](../.gitbook/assets/pack-list-enable.png)

The **Enabled** slider also appears on Pack detail pages:

![Enable Pack From Details](../.gitbook/assets/pack-details-enable.png)

### Update or Rollback Detection Pack

New updates to Detection Packs are periodically released to the `panther-analysis` repository. These updates are automatically detected by Panther, and the pack overview page will show an _Update Available_ flag next to relevant packs.

To update pack detections:

1. Log in to your Panther Console.
2. Navigate to **Detections > Packs**.
3. Select the version in the "Version" dropdown menu then click **Update Pack**.

![Update Pack](../.gitbook/assets/pack-update.png)

To revert to a previous Pack version:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**.
3. Select the version in the "Version" dropdown menu then click **Revert Pack**.

![Revert Pack](../.gitbook/assets/pack-revert.png)

## Managing Detections with Packs

### Editing Managed Detections

After a Pack has been enabled, there are a subset of fields that you can manually edit in the Panther Console:

* Enabled / Disabled
* Severity
* Events Threshold
* Deduplication Period
* Destination Override&#x20;

Any changes you make to these fields in the Panther Console will be preserved when the pack is updated or reverted to a previous version. All other fields will be greyed out in the Panther Console, and the "Functions and Tests" editor will be read-only. &#x20;

{% hint style="info" %}
Note: For enabled Packs, the above fields can **only** be edited manually in the Panther Console. Editing these fields in the Detection .yml files will not override these values.
{% endhint %}



You can make changes to the editable fields in the Panther Console:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**.
3. Click on the Pack that contains the detection you want to edit, then click on the detection.
4. In the page that opens, click **Edit Rule** in the upper right side.\
   ![](../.gitbook/assets/edit-rule.png)
   * You will be presented with all the fields that you can edit.
5. Click **Update** to save your changes.&#x20;

![](../.gitbook/assets/UpdateButton.jpg)

### Cloning and editing a managed detection

If a rule or policy included in a Detection Pack does not fit your needs, you can clone it and then customize the cloned copy of the Detection Pack:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**.
3. Click on the Pack that contains the detection you want to edit, then click on the detection.
4. In the page that opens, click **Clone & edit** in the upper right:\
   ![](../.gitbook/assets/clone-rule.png)
   * You will be redirected to the standard rule creation interface where you can make changes to the new copy of the rule.
5. Click **Update** in the upper right.

Note that the display name of a cloned Detection will have `_COPY` appended to it.

Also note that cloning and editing a rule does not disable it. If you wish to have your customized copy of the Detection _replace_ the Panther managed rule, make sure to go back and [disable it](https://docs.runpanther.io/writing-detections/detection-packs#enable-and-disable-detection-pack).

The Cloned rule will not be managed by Panther or receive automatic updates when the original version of the Pack is updated. The original version that is still contained within the Pack will continue to receive updates as normal, whether it is enabled or disabled.

## Pack Sources

Pack Sources provide a way to configure custom Github sources for Detection Packs. Once a Pack Source is configured, Panther will check the source repository for new tagged releases every 24 hours. In order for Panther to find your custom Pack(s) from your Pack Source, you must:

* Ensure that your release is finalized, and not in a draft state
* Ensure that your release is named according to [SemVer format](https://semver.org/), and the tag of the release must be the same as the name of the release
* Ensure that the artifact of the release is named `panther-analysis-all.zip` (and a corresponding `panther-analysis-all.sig` if you are signing your release)&#x20;
* Ensure that your `panther-analysis-all.zip` contains at least one Pack Manifest file ([see section on Pack Manifests below](detection-packs.md#pack-manifests) for more information)

![](<../.gitbook/assets/Screen Shot 2022-04-05 at 4.15.00 PM.png>)

You can use the `panther_analysis_tool` (PAT) to generate the required release assets, as well as publish a draft release (see [Creating a Github Release - Panther Analysis Tool](detection-packs.md#creating-a-github-release-panther-analysis-tool) for additional details.) You can manage custom packs using the same functionality as Panther-provided packs.

Pack source fields are described in the following table.

| **Field Name** | **Required** | Description                                                      | Expected Value |
| -------------- | ------------ | ---------------------------------------------------------------- | -------------- |
| `Owner`        | Yes          | The owner/organization of the target repository                  | String         |
| `Repository`   | Yes          | The name of the repository                                       | String         |
| `kmsKey`       | No           | The ARN for a sign/verify kms key to validate release signatures | String         |
| `AccessToken`  | No           | Personal Access Token used to access a private repository        | String         |

### Pack Manifests

Packs are defined by creating Pack Manifest yaml files, which contain metadata about your Pack (such as its name, description, and the detections/files that are included in your Pack).&#x20;

Your `panther-analysis-all.zip` release artifact can contain many different Pack Manifests along with other files from your repository such as detections, global helpers, data models, etc. If you add your GitHub repository as a Pack Source in the Panther Console, then each of these Pack Manifest files will show up as a Pack in the Panther Console that can be separately enabled/disabled.

The following table is a reference of the different keys that are valid in your Pack Manifest. You can find additional examples of the Pack Manifests that Panther uses in our [Panther-provided Packs on Github](https://github.com/panther-labs/panther-analysis/tree/32f815266776ef7b3e7f609a0213032ff42c72f0/packs).

| Field Name     | Required | Description                                                                                                                                                        | Expected Value     |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------ |
| AnalysisType   | true     | Indicates the analysis type this file is                                                                                                                           | `pack`             |
| PackID         | true     | The unique ID of this pack                                                                                                                                         | String             |
| Description    | false    | Extra information about this Pack that will be displayed in the Panther Console                                                                                    | String             |
| PackDefinition | true     | A mapping with a single field called `IDs` which is a list of strings. Each string in the `IDs` list should be a unique ID of a file that is included in this Pack | { IDs: \[string] } |
| DisplayName    | true     | The user-friendly title that will be displayed for this Pack in the Panther Console                                                                                | String             |

### Accessing Private Repositories

In order for Panther to have access to poll a private repository, you must configure the Pack Source with a personal access token. See the [Github documentation](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) for further details on creating a token.

A personal access token will grant access to all the repositories where the account owner has access. We recommend creating a ["machine user"](https://docs.github.com/en/developers/overview/managing-deploy-keys#machine-users) that you can add as an outside collaborator to the repository containing the detection packs. This way, the access token can be scoped for a particular use and repository.

### Release Signatures

The Panther-provided packs are signed using an asymmetric AWS KMS key. Prior to importing any detections from the Panther pack source, it will validate the signature using the release asset `panther-analysis.sig`. This ensures that any detections being imported have not been tampered or modified. If you would like to use similar functionality, [create a sign/verify KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) and modify the policy to allow Panther to run `kms:Verify` using that key.



In this example entry to add to the key policy, the account ID should be replaced with the account ID where Panther is running:

```javascript
{
    "Sid": "Enable KMS Verify",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::{accountid}:root"
    },
    "Action": "kms:Verify",
    "Resource": "*"
}
```

### Managing Pack Sources

**To Add a pack source**:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**, then click the Detection Pack Sources tab.
3. Click **Create New** in the upper right.&#x20;
   * Enter the field names for each input field.
4. Click **Save**.

![](<../.gitbook/assets/Screen Shot 2022-03-29 at 10.16.36 AM.png>)

**To modify the `kmsKey` or `AccessToken` fields for a pack source**:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**, then click the Detection Pack Sources tab.
3. Click `...` next to a Pack Source then click **Edit**. Click on a Pack Source.&#x20;
   * Edit the fields on this page.
4. Click **Save**.



**To Delete a pack source**:

1. Log in to your Panther Console.
2. Navigate to **Analysis > Packs**, then click the Detection Pack Sources tab.
3. Click `...` next to a Pack Source then click **Delete**.

![](<../.gitbook/assets/Screen Shot 2022-03-29 at 10.18.35 AM.png>)

{% hint style="info" %}
Deleting a Pack Source will delete the packs originating from it, along with all of the detections in it.
{% endhint %}

### Creating a Github Release - Panther Analysis Tool

The `panther_analysis_tool` (PAT) can streamline the process of creating an appropriate Github release, with or without an associated signature file.

To generate the release assets, use the `release` command.

```bash
% panther_analysis_tool release --help
usage: panther_analysis_tool release [-h] [--aws-profile AWS_PROFILE]
                                     [--filter KEY=VALUE [KEY=VALUE ...]]
                                     [--kms-key KMS_KEY]
                                     [--minimum-tests MINIMUM_TESTS]
                                     [--out OUT] [--path PATH] [--skip-tests]

optional arguments:
  -h, --help            show this help message and exit
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --filter KEY=VALUE [KEY=VALUE ...]
  --kms-key KMS_KEY     The key id to use to sign the release asset.
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --out OUT             The path to store output files.
  --path PATH           The relative path to Panther policies and rules.
  --skip-tests
```

To automatically create a draft release in your Github repository, first set the `GITHUB_TOKEN` environment variable to a personal access token with appropriate permissions to access the target repository. Then, use the `publish` command.

{% hint style="warning" %}
Note: Using the `panther_analysis_tool publish` command creates a draft release. Before Panther is able to pull in this release artifact, you must go to your Github repository and manually finalize the draft into a release.
{% endhint %}

```bash
% panther_analysis_tool publish --help
usage: panther_analysis_tool publish [-h] [--body BODY]
                                     [--github-branch GITHUB_BRANCH]
                                     [--github-owner GITHUB_OWNER]
                                     [--github-repository GITHUB_REPOSITORY]
                                     --github-tag GITHUB_TAG
                                     [--aws-profile AWS_PROFILE]
                                     [--filter KEY=VALUE [KEY=VALUE ...]]
                                     [--kms-key KMS_KEY]
                                     [--minimum-tests MINIMUM_TESTS]
                                     [--out OUT] [--skip-tests]

optional arguments:
  -h, --help            show this help message and exit
  --body BODY           The text body for the release
  --github-branch GITHUB_BRANCH
                        The branch to base the release on
  --github-owner GITHUB_OWNER
                        The github owner of the repsitory
  --github-repository GITHUB_REPOSITORY
                        The github repsitory name
  --github-tag GITHUB_TAG
                        The tag name for this release
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --filter KEY=VALUE [KEY=VALUE ...]
  --kms-key KMS_KEY     The key id to use to sign the release asset.
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --out OUT             The path to store output files.
  --skip-tests
```

{% hint style="info" %}
The `kms-key` argument is an optional argument that you can use to generate a signature file. If you want to use this argument, be sure to run panther\_analysis\_tool using the appropriate aws credentials to call `kms:Sign` on the specified key.
{% endhint %}
