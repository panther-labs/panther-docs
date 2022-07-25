---
description: >-
  Workflows outside of the Panther Console that you can use to interact with
  your Panther account
---

# Panther Developer Workflows: Detections

## Overview

Panther Developer Workflows are workflows that you can use outside of the Panther Console to interact with your Panther account, such as the Panther API, Panther Analysis Tool, and CI/CD workflows.&#x20;

This page describes how to use the Panther Analysis Tool (PAT) to test and upload locally managed Detections, and how to optionally integrate with a CI/CD setup.

For information on using the Panther API, please see the [Panther API documentation](../api-beta/).

## Using the Panther Analysis Tool

The `panther_analysis_tool` (PAT) is an [open source](https://github.com/panther-labs/panther\_analysis\_tool) utility for testing, packaging, and deploying Panther detections from source code. It's designed for developer-centric workflows such as managing your Panther analysis packs programmatically or within [CI/CD pipelines](https://docs.panther.com/guides/ci-cd-onboarding-guide).

### Installing PAT

#### Installing with pip

First, ensure you have [Python 3](https://docs.python-guide.org/starting/install3/osx/) installed.

To install PAT, run this command:

```bash
pip3 install panther_analysis_tool
```

#### Building from source

If you'd prefer instead to run from source for development reasons, first setup your environment:

```
$ make install
$ pipenv run -- pip3 install -e .
```

#### Updating the version

You can use the following utility script to update the version number in relevant files if a new release is being created:

```
cd bin/
./version_bump.py 0.10.9  #replace with the new version you are releasing
```

If you would rather use the `panther_analysis_tool` outside of the virtual environment, install it directly:

```
$ make deps
$ pip3 install -e .
```

### PAT configuration file

PAT will read options from a configuration file called `.panther_settings.yml` located in your working directory. An example configuration file is included in this repo: [example\_panther\_config.yml](https://github.com/panther-labs/panther\_analysis\_tool/blob/master/example\_panther\_config.yml). It contains example syntax for supported options.

Note that options in the configuration file override options passed on the command line. For example if you set `minimum_tests: 2` in the configuration file and `--minimum-tests 1` on the command line, the minimum number of tests will be 2.

### PAT commands and usage <a href="#uploading-to-panther" id="uploading-to-panther"></a>

#### Uploading packages to Panther directly

{% hint style="info" %}
**Panther SaaS customers**: Please file a support ticket to gain upload access to your Panther environment.
{% endhint %}

Make sure to configure your environment with valid AWS credentials prior to running the command below. This command will upload based on the exported value of `AWS_REGION`.

To upload your analysis packs to your Panther Console, run the following command:

```bash
panther_analysis_tool upload --path <path-to-your-rules> --out tmp
```

Analysis with the same ID are overwritten. Additionally, locally deleted rules/policies will not automatically be deleted in the database and must be removed manually. We recommend setting the Enabled property to false instead of deleting policies or rules for CLI driven workflows.

#### Creating a package to upload to the Panther Console

To create a package for uploading manually to the Panther Console, run the following command:

```
$ panther_analysis_tool zip --path tests/fixtures/valid_policies/ --out tmp
[INFO]: Testing analysis packs in tests/fixtures/valid_policies/

AWS.IAM.MFAEnabled
	[PASS] Root MFA not enabled fails compliance
	[PASS] User MFA not enabled fails compliance

[INFO]: Zipping analysis packs in tests/fixtures/valid_policies/ to tmp
[INFO]: <current working directory>/tmp/panther-analysis-2020-03-23T12-48-18.zip
```

####

#### Deleting Rules, Policies, or Saved Queries with PAT

While `panther_analysis_tool upload --path <directory>` will upload everything from `<directory>`, it will not delete anything in your Panther instance if you simply remove a local file from `<directory>`. Instead, you can use the `panther_analysis_tool delete` command to explicitly delete detections from your Panther instance.\
\
To delete a specific detection, you can run the following command:

```
panther_analysis_tool delete --analysis-id MyRuleId
```

This will interactively ask you for a confirmation before it deletes the detection. If you would like to delete without confirming, you can use the following command:

```
panther_analysis_tool delete --analysis-id MyRuleId --no-confirm
```



#### Running Tests with PAT

Use the Panther Analysis Tool to load the defined specification files and evaluate unit tests locally:

```bash
panther_analysis_tool test --path <folder-name>
```

To filter rules or policies based on certain attributes:

```bash
panther_analysis_tool test --path <folder-name> --filter RuleID=Category.Behavior.MoreInfo
```



#### Available commands

```
$ panther_analysis_tool -h


usage: panther_analysis_tool [-h] [--version] [--debug] {release,test,upload,delete,test-lookup-table,zip} ...

Panther Analysis Tool: A command line tool for managing Panther policies and rules.

positional arguments:
  {release,test,upload,delete,test-lookup-table,zip}
    release             Create release assets for repository containing panther detections. Generates a file called panther-analysis-all.zip and optionally generates panther-analysis-all.sig
    test                Validate analysis specifications and run policy and rule tests.
    upload              Upload specified policies and rules to a Panther deployment.
    delete              Delete policies, rules, or saved queries from a Panther deployment
    test-lookup-table   Validate a Lookup Table spec file.
    zip                 Create an archive of local policies and rules for uploading to Panther.

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  --debug
```

#### Available sub commands

{% tabs %}
{% tab title="release" %}
```
$ panther_analysis_tool release -h
usage: panther_analysis_tool release [-h] [--aws-profile AWS_PROFILE]
                                     [--filter KEY=VALUE [KEY=VALUE ...]]
                                     [--ignore-files IGNORE_FILES [IGNORE_FILES ...]]
                                     [--kms-key KMS_KEY]
                                     [--minimum-tests MINIMUM_TESTS]
                                     [--out OUT] [--path PATH] [--skip-tests]
                                     [--skip-disabled-tests]
                                     [--available-destination AVAILABLE_DESTINATION]

optional arguments:
  -h, --help            show this help message and exit
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --filter KEY=VALUE [KEY=VALUE ...]
  --ignore-files IGNORE_FILES [IGNORE_FILES ...]
                        Relative path to files in this project to be ignored
                        by panther-analysis tool, space separated. Example
                        ./foo.yaml ./bar/baz.yaml
  --kms-key KMS_KEY     The key id to use to sign the release asset.
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --out OUT             The path to store output files.
  --path PATH           The relative path to Panther policies and rules.
  --skip-tests
  --skip-disabled-tests
  --available-destination AVAILABLE_DESTINATION
                        A destination name that may be returned by the
                        destinations function. Repeat the argument to define
                        more than one name.

```
{% endtab %}

{% tab title="test" %}
```
$ panther_analysis_tool test -h   
usage: panther_analysis_tool test [-h] [--filter KEY=VALUE [KEY=VALUE ...]]
                                  [--minimum-tests MINIMUM_TESTS]
                                  [--path PATH]
                                  [--ignore-extra-keys IGNORE_EXTRA_KEYS]
                                  [--ignore-files IGNORE_FILES [IGNORE_FILES ...]]
                                  [--skip-disabled-tests]
                                  [--available-destination AVAILABLE_DESTINATION]

optional arguments:
  -h, --help            show this help message and exit
  --filter KEY=VALUE [KEY=VALUE ...]
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --path PATH           The relative path to Panther policies and rules.
  --ignore-extra-keys IGNORE_EXTRA_KEYS
                        Meant for advanced users; allows skipping of extra
                        keys from schema validation.
  --ignore-files IGNORE_FILES [IGNORE_FILES ...]
                        Relative path to files in this project to be ignored
                        by panther-analysis tool, space separated. Example
                        ./foo.yaml ./bar/baz.yaml
  --skip-disabled-tests
  --available-destination AVAILABLE_DESTINATION
                        A destination name that may be returned by the
                        destinations function. Repeat the argument to define
                        more than one name.

```
{% endtab %}

{% tab title="upload" %}
```
$ panther_analysis_tool upload -h
usage: panther_analysis_tool upload [-h] [--max-retries MAX_RETRIES]
                                    [--aws-profile AWS_PROFILE]
                                    [--filter KEY=VALUE [KEY=VALUE ...]]
                                    [--minimum-tests MINIMUM_TESTS]
                                    [--out OUT] [--path PATH] [--skip-tests]
                                    [--skip-disabled-tests]
                                    [--ignore-extra-keys IGNORE_EXTRA_KEYS]
                                    [--ignore-files IGNORE_FILES [IGNORE_FILES ...]]
                                    [--available-destination AVAILABLE_DESTINATION]

optional arguments:
  -h, --help            show this help message and exit
  --max-retries MAX_RETRIES
                        Retry to upload on a failure for a maximum number of
                        times
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --filter KEY=VALUE [KEY=VALUE ...]
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --out OUT             The path to store output files.
  --path PATH           The relative path to Panther policies and rules.
  --skip-tests
  --skip-disabled-tests
  --ignore-extra-keys IGNORE_EXTRA_KEYS
                        Meant for advanced users; allows skipping of extra
                        keys from schema validation.
  --ignore-files IGNORE_FILES [IGNORE_FILES ...]
                        Relative path to files in this project to be ignored
                        by panther-analysis tool, space separated. Example
                        ./foo.yaml ./bar/baz.yaml
  --available-destination AVAILABLE_DESTINATION
                        A destination name that may be returned by the
                        destinations function. Repeat the argument to define
                        more than one name.
```
{% endtab %}

{% tab title="delete" %}
```
$ panther_analysis_tool delete -h
usage: panther_analysis_tool delete [-h] [--no-confirm] [--athena-datalake]
                                    [--aws-profile AWS_PROFILE]
                                    [--analysis-id ANALYSIS_ID [ANALYSIS_ID ...]]
                                    [--query-id QUERY_ID [QUERY_ID ...]]

optional arguments:
  -h, --help            show this help message and exit
  --no-confirm          Skip manual confirmation of deletion
  --athena-datalake     Instance DataLake is backed by Athena
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --analysis-id ANALYSIS_ID [ANALYSIS_ID ...]
                        Space separated list of Rule or Policy IDs
  --query-id QUERY_ID [QUERY_ID ...]
                        Space separated list of Saved Queries

```
{% endtab %}

{% tab title="test-lookup-table" %}
```
panther_analysis_tool test-lookup-table -h
usage: panther_analysis_tool test-lookup-table [-h]
                                               [--aws-profile AWS_PROFILE]
                                               --path PATH

optional arguments:
  -h, --help            show this help message and exit
  --aws-profile AWS_PROFILE
                        The AWS profile to use when updating the AWS Panther
                        deployment.
  --path PATH           The relative path to a lookup table input file.

```
{% endtab %}

{% tab title="zip" %}
```
$ panther_analysis_tool zip -h
usage: panther_analysis_tool zip [-h] [--filter KEY=VALUE [KEY=VALUE ...]]
                                 [--ignore-files IGNORE_FILES [IGNORE_FILES ...]]
                                 [--minimum-tests MINIMUM_TESTS] [--out OUT]
                                 [--path PATH] [--skip-tests]
                                 [--skip-disabled-tests]
                                 [--available-destination AVAILABLE_DESTINATION]

optional arguments:
  -h, --help            show this help message and exit
  --filter KEY=VALUE [KEY=VALUE ...]
  --ignore-files IGNORE_FILES [IGNORE_FILES ...]
                        Relative path to files in this project to be ignored
                        by panther-analysis tool, space separated. Example
                        ./foo.yaml ./bar/baz.yaml
  --minimum-tests MINIMUM_TESTS
                        The minimum number of tests in order for a detection
                        to be considered passing. If a number greater than 1
                        is specified, at least one True and one False test is
                        required.
  --out OUT             The path to store output files.
  --path PATH           The relative path to Panther policies and rules.
  --skip-tests
  --skip-disabled-tests
  --available-destination AVAILABLE_DESTINATION
                        A destination name that may be returned by the
                        destinations function. Repeat the argument to define
                        more than one name.

```
{% endtab %}
{% endtabs %}

#### Filtering PAT commands

The `test`, `zip`, and `upload` commands all support filtering. Filtering works by passing the `--filter` argument with a list of filters specified in the format `KEY=VALUE1,VALUE2`. The keys can be any valid field in a policy or rule. When using a filter, only anaylsis that matches each filter specified will be considered.&#x20;

For example, the following command will test only items with the AnalysisType of policy AND the severity of High:

```
panther_analysis_tool test --path tests/fixtures/valid_policies --filter AnalysisType=policy Severity=High
[INFO]: Testing analysis packs in tests/fixtures/valid_policies

AWS.IAM.BetaTest
	[PASS] Root MFA not enabled fails compliance
	[PASS] User MFA not enabled fails compliance
```

The following command will test items with the AnalysisType policy OR rule, AND the severity High:

```
panther_analysis_tool test --path tests/fixtures/valid_policies --filter AnalysisType=policy,rule Severity=High
[INFO]: Testing analysis packs in tests/fixtures/valid_policies

AWS.IAM.BetaTest
	[PASS] Root MFA not enabled fails compliance
	[PASS] User MFA not enabled fails compliance

AWS.CloudTrail.MFAEnabled
	[PASS] Root MFA not enabled fails compliance
	[PASS] User MFA not enabled fails compliance
```

When writing policies or rules that refer to the `global` analysis types, be sure to include them in your filter. You can include an empty string as a value in a filter, and it will mean the filter is only applied if the field exists.&#x20;

The following command will return an error, because the policy in question imports a global but the global does not have a severity so it is excluded by the filter:

```
panther_analysis_tool test --path tests/fixtures/valid_policies --filter AnalysisType=policy,global Severity=Critical
[INFO]: Testing analysis packs in tests/fixtures/valid_policies

AWS.IAM.MFAEnabled
	[ERROR] Error loading module, skipping

Invalid: tests/fixtures/valid_policies/example_policy.yml
	No module named 'panther'

[ERROR]: [('tests/fixtures/valid_policies/example_policy.yml', ModuleNotFoundError("No module named 'panther'"))]
```

For this query to work as expected, you need to allow for the severity field to be absent:

```
panther_analysis_tool test --path tests/fixtures/valid_policies --filter AnalysisType=policy,global Severity=Critical,""
[INFO]: Testing analysis packs in tests/fixtures/valid_policies

AWS.IAM.MFAEnabled
	[PASS] Root MFA not enabled fails compliance
	[PASS] User MFA not enabled fails compliance
```

Filters work for the `zip` and `upload` commands in the exact same way they work for the `test` command.

In addition to filtering, you can set a minimum number of unit tests with the `--minimum-tests` flag. Detections that don't have the minimum number of tests will be considered failing, and if `--minimum-tests` is set to 2 or greater it will also enforce that at least one test must return True and one must return False.

In the example below, even though the rules passed all their tests, they're still considered failing because they do not have the correct test coverage:

```
panther_analysis_tool test --path tests/fixtures/valid_policies --minimum-tests 2
% panther_analysis_tool test --path okta_rules --minimum-tests 2
[INFO]: Testing analysis packs in okta_rules

Okta.AdminRoleAssigned
	[PASS] Admin Access Assigned

Okta.BruteForceLogins
	[PASS] Failed login

Okta.GeographicallyImprobableAccess
	[PASS] Non Login
	[PASS] Failed Login

--------------------------
Panther CLI Test Summary
	Path: okta_rules
	Passed: 0
	Failed: 3
	Invalid: 0

--------------------------
Failed Tests Summary
	Okta.AdminRoleAssigned
		['Insufficient test coverage, 2 tests required but only 1 found.', 'Insufficient test coverage: expected at least one passing and one failing test.']

	Okta.BruteForceLogins
		['Insufficient test coverage, 2 tests required but only 1 found.', 'Insufficient test coverage: expected at least one passing and one failing test.']

	Okta.GeographicallyImprobableAccess
		['Insufficient test coverage: expected at least one passing and one failing test.']
```

## Writing Detections locally

Writing Detections locally means creating Python and metadata files that define a Panther Detection on your own machine. After writing Detections locally, you upload the files to your Panther instance (typically via the [Panther Analysis Tool](panther-analysis-tool.md#using-the-panther-analysis-tool)) to control your Detection content.&#x20;

In Panther, there are three core Detection types:&#x20;

* **Real-Time Rules** that analyze data as soon as it's sent to Panther
* **Scheduled Rules** that run after a Scheduled Query has been executed
* **Policies** that detect insecure cloud resources

### File setup

Each detection consists of:

* A Python file (a file with a `.py` extension) containing your detection/audit logic&#x20;
* A YAML or JSON specification file (a file with a `.yml` or `.json` extension) containing metadata attributes of the detection.&#x20;
  * By convention, we give this file the same name as the Python file.

We recommend creating folders based on log/resource type to group your detections, such as `suricata_rules` or `aws_s3_policies`. You can use the open source [Panther Analysis](https://github.com/panther-labs/panther-analysis) repo as a reference.

We also recommend managing these files in a version control system (VCS). Most commonly we see GitHub or GitLab used, which are managed git providers.

{% hint style="info" %}
It's best practice to create a fork of Panther's [open-source analysis repository](https://github.com/panther-labs/panther-analysis), but you can also create your own repo from scratch.
{% endhint %}

### Writing real-time and scheduled rules locally

Rules are Python functions to detect suspicious behaviors. Returning a value of `True` indicates suspicious activity, which triggers an alert.

1.  [Write your Rule](rules.md) and save it (in your folder of choice) as `my_new_rule.py`:\
    def rule(event):

    ```
    def rule(event):  
      return 'prod' in event.get('hostName')
    ```
2.  Create a metadata file using the template below:

    ```
    AnalysisType: rule
    DedupPeriodMinutes: 60 # 1 hour
    DisplayName: Example Rule to Check the Format of the Spec
    Enabled: true
    Filename: my_new_rule.py
    RuleID: Type.Behavior.MoreContext
    Severity: High
    LogTypes:
      - LogType.GoesHere
    Reports:
      ReportName (like CIS, MITRE ATT&CK):
        - The specific report section relevant to this rule
    Tags:
      - Tags
      - Go
      - Here
    Description: >
      This rule exists to validate the CLI workflows of the Panther CLI
    Runbook: >
      First, find out who wrote this the spec format, then notify them with feedback.
    Reference: https://www.a-clickable-link-to-more-info.com
    ```

When this rule is uploaded, each of the fields you would normally populate in the UI will be auto-filled. See [Rule Specification Reference](panther-analysis-tool.md#rule-specification-reference) below for a complete list of required and optional fields.

### Rule Tests

Tests help validate that your rule will behave as intended and detect the early signs of a breach. In your spec file, add the `Tests` key with sample cases:

```
Tests:
  -
    Name: Name to describe our first test
    LogType: LogType.GoesHere
    ExpectedResult: true or false
    Log:
      {
        "hostName": "test-01.prod.acme.io",
        "user": "martin_smith",
        "eventTime": "June 22 5:50:52 PM"
      }
```

We recommend running as many test cases as possible, including both true and false positives.

### Writing Policies locally

Policies are Python functions to detect misconfigured cloud infrastructure. Returning a value of `True` indicates this resource is valid and properly configured. Returning `False` indicates a policy failure, which triggers an alert.

1.  [Write your policy](policies.md) and save it (in your folder of choice) as `my_new_policy.py`:\
    def polcy(resource):

    ```
    def polcy(resource):  
      return resource['Region'] != 'us-east-1'
    ```
2.  Create a specification file using the template below:

    ```
    AnalysisType: policy
    Enabled: true
    Filename: my_new_policy.py
    PolicyID: Category.Type.MoreInfo
    ResourceType:
      - Resource.Type.Here
    Severity: Info|Low|Medium|High|Critical
    DisplayName: Example Policy to Check the Format of the Spec
    Tags:
      - Tags
      - Go
      - Here
    Runbook: Find out who changed the spec format.
    Reference: https://www.link-to-info.io
    ```

See the [Policy Specification Reference](panther-analysis-tool.md#policy-specification-reference) below for a complete list of required and optional fields.

### Policy Tests

In the spec file, add the following `Tests` key:

```
Tests:
  -
    Name: Name to describe our first test.
    ResourceType: AWS.S3.Bucket
    ExpectedResult: true
    Resource:
      {
        "PublicAccessBlockConfiguration": null,
        "Region": "us-east-1",
        "Policy": null,
        "AccountId": "123456789012",
        "LoggingPolicy": {
          "TargetBucket": "access-logs-us-east-1-100",
          "TargetGrants": null,
          "TargetPrefix": "acmecorp-fiancial-data/"
        },
        "EncryptionRules": [
          {
            "ApplyServerSideEncryptionByDefault": {
              "SSEAlgorithm": "AES256",
              "KMSMasterKeyID": null
            }
          }
        ],
        "Arn": "arn:aws:s3:::acmecorp-fiancial-data",
        "Name": "acmecorp-fiancial-data",
        "LifecycleRules": null,
        "ResourceType": "AWS.S3.Bucket",
        "Grants": [
          {
            "Permission": "FULL_CONTROL",
            "Grantee": {
              "URI": null,
              "EmailAddress": null,
              "DisplayName": "admins",
              "Type": "CanonicalUser",
              "ID": "013ae1034i130431431"
            }
          }
        ],
        "Versioning": "Enabled",
        "ResourceId": "arn:aws:s3:::acmecorp-fiancial-data",
        "Tags": {
          "aws:cloudformation:logical-id": "FinancialDataBucket"
        },
        "Owner": {
          "ID": "013ae1034i130431431",
          "DisplayName": "admins"
        },
        "TimeCreated": "2020-06-13T17:16:36.000Z",
        "ObjectLockConfiguration": null,
        "MFADelete": null
      }
```

The value of Resource can be a JSON object copied directly from the Policies > Resources explorer.

### Policy and rule unit test mocking

Both policy and rule tests support unit test mocking. In order to configure mocks for a particular test case, add the `Mocks` key to your test case. The `Mocks` key is used to define a list of functions you want to mock, and the value that should be returned when that function is called. Multiple functions can be mocked in a single test. For example, if we have a rule test and want to mock the function `get_counter` to always return a `1` and the function `geoinfo_from_ip` to always return a specific set of geo IP info, we could write our unit test like this:

```
Tests:
  -
    Name: Test With Mock
    LogType: LogType.Custom
    ExpectedResult: true
    Mocks:
      - objectName: get_counter
        returnValue: 1
      - objectName: geoinfo_from_ip
        returnValue: >-
                          {
                          "region": "UnitTestRegion",
                          "city": "UnitTestCityNew",
                          "country": "UnitTestCountry"
                          }
    Log:
      {
        "hostName": "test-01.prod.acme.io",
        "user": "martin_smith",
        "eventTime": "June 22 5:50:52 PM"
      }
```

Mocking allows us to emulate network calls without requiring API keys or network access in our CI/CD pipeline, and without muddying the state of external tracking systems (such as the panther KV store).

### Customizing Detections

To manage custom detections, you can create a private fork of the [Panther Analysis Github repo](https://github.com/panther-labs/panther-analysis). Upon [tagged releases](https://github.com/panther-labs/panther-analysis/releases), you can pull upstream changes from this public repo.

For instructions on forking a repo, see [Github's documentation](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

#### Getting Updates

When you want to pull in the latest changes from the repository, perform the following steps from your private repo:

```
# add the public repository as a remote
git remote add panther-upstream git@github.com:panther-labs/panther-analysis.git

# Pull in the latest changes
# Note: You may need to use the `--allow-unrelated-histories`
#       flag if you did not maintain the history originally
git pull panther-upstream master

# Push the latest changes up to your forked repo and merge them
git push

```

## Writing Scheduled Queries locally

Writing [Scheduled Queries](../help/glossary.md#scheduled-query) locally means creating metadata files that define SQL queries on your own machine. Upload the files to your Panther instance (typically via the [Panther Analysis Tool](../help/glossary.md#panther-analysis-tool-pat)) to control your Scheduled Query content.

### File setup

Each saved query consists of:

* A YAML file (`.yml` or `.json` extension) containing metadata attributes of the Scheduled Query.&#x20;

You can use the open-source [Panther Analysis](https://github.com/panther-labs/panther-analysis) repo as a reference.

We also recommend managing these files in a version control system (VCS). Most commonly we see GitHub or GitLab used, which are managed git providers.

{% hint style="info" %}
It's best practice to create a fork of Panther's [open-source analysis repository](https://github.com/panther-labs/panther-analysis), but you can also create your own repo from scratch.
{% endhint %}

### Writing Scheduled Queries locally

Write your [Scheduled Query](../data-analytics/scheduled-queries.md) and save it in your folder of choice as `new-scheduled-query.yml`:\
def rule(event):

* Create a metadata file using the template below:

```
AnalysisType: scheduled_query
QueryName: ScheduledQuery_Example
Description: Example of a scheduled query for PAT
Enabled: true
Query:
  - Your query appears here
SnowflakeQuery:
  - Same query appears here
Tags:
  - Your tags   
Schedule:
  CronExpression: "0 0 29 2 *"
  RateMinutes: 0
  TimeoutMinutes: 2
```

When your Scheduled Query is uploaded, each of the fields you would normally populate in the UI will be auto-filled. See [Scheduled Query Specification Reference](panther-analysis-tool.md#scheduled-query-specification-reference) below for a complete list of required and optional fields.

## Data Models

To add a new data model using the `panther_analysis_tool:`

1.  Create your DataModel specification file (e.g. `data_models/aws_cloudtrail_datamodel.yml`):

    ```
    AnalysisType: datamodel
    LogTypes: 
      - AWS.CloudTrail
    DataModelID: AWS.CloudTrail
    Filename: aws_cloudtrail_data_model.py
    Enabled: true
    Mappings:
      - Name: actor_user
        Path: $.userIdentity.userName
      - Name: event_type
        Method: get_event_type
      - Name: source_ip
        Path: sourceIPAddress
      - Name: user_agent
        Path: userAgent
    ```
2.  If any `Method`s are defined, create an associated Python file (`data_models/aws_cloudtrail_datamodel.py`):\
    **Note**: The Filename specification field is required if a Method is defined in a mapping. If Method is not used in any Mappings, no Python file is required.

    ```
    from panther_base_helpers import deep_get
    def get_event_type(event):
        if event.get('eventName') == 'ConsoleLogin' and deep_get(event, 'userIdentity', 'type') == 'IAMUser':
            if event.get('responseElements', {}).get('ConsoleLogin') == 'Failure':
                return "failed_login"
            if event.get('responseElements', {}).get('ConsoleLogin') == 'Success':
                return "successful_login"
        return None

    ```
3. Use this data model in a rule by:
   1. Adding the LogType under the Rule specification `LogType` field&#x20;
   2. Adding the LogType to all the Rule's `Test` cases, in the `p_log_type` field
   3. Leveraging the `event.udm()` method in the Rule's python logic:

```
AnalysisType: rule
DedupPeriodMinutes: 60
DisplayName: DataModel Example Rule
Enabled: true
Filename: my_new_rule.py
RuleID: DataModel.Example.Rule
Severity: High
LogTypes:
  # Add LogTypes where this rule is applicable
  # and a Data Model exists for that LogType
  - AWS.CloudTrail
Tags:
  - Tags
Description: >
  This rule exists to validate the CLI workflows of the Panther CLI
Runbook: >
  First, find out who wrote this the spec format, then notify them with feedback.
Tests:
  - Name: test rule
    ExpectedResult: true
    # Add the LogType to the test specification in the 'p_log_type' field
    Log: {
      "p_log_type": "AWS.CloudTrail"
    }
```

```python
def rule(event):
    # filter events on unified data model field
    return event.udm('event_type') == 'failed_login'


def title(event):
    # use unified data model field in title
    return '{}: User [{}] from IP [{}] has exceeded the failed logins threshold'.format(
        event.get('p_log_type'), event.udm('actor_user'),
        event.udm('source_ip'))
```

## Globals locally

Global functions allow common logic to be shared across either rules or policies. To declare them as code, add them into the `global_helpers` folder with a similar pattern to rules and policies.

{% hint style="info" %}
Globals defined outside of the `global_helpers` folder will not be loaded.
{% endhint %}

First, create your Python file (`global_helpers/acmecorp.py`):

```python
from fnmatch import fnmatch

RESOURCE_PATTERN = 'acme-corp-*-[0-9]'


def matches_internal_naming(resource_name):
  return fnmatch(resource_name, RESOURCE_PATTERN)
```

Then, create your specification file:

```yaml
AnalysisType: global
GlobalID: acmecorp
Filename: acmecorp.py
Description: A set of helpers internal to acme-corp
```

Finally, use this helper in a policy (or a rule):

```python
import acmecorp


def policy(resource):
  return acmecorp.matches_internal_naming(resource['Name'])
```

## Specification Reference

Required fields are in **bold**.

### Rule Specification Reference

A complete list of rule specification fields:

| Field Name           | Description                                                                                                                                                 | Expected Value                                                               |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **`AnalysisType`**   | Indicates whether this analysis is a rule, policy, or global                                                                                                | `rule`                                                                       |
| **`Enabled`**        | Whether this rule is enabled                                                                                                                                | Boolean                                                                      |
| **`FileName`**       | The path (with file extension) to the python rule body                                                                                                      | String                                                                       |
| **`RuleID`**         | The unique identifier of the rule                                                                                                                           | String                                                                       |
| **`LogTypes`**       | The list of logs to apply this rule to                                                                                                                      | List of strings                                                              |
| **`Severity`**       | What severity this rule is                                                                                                                                  | One of the following strings: `Info`, `Low`, `Medium`, `High`, or `Critical` |
| `Description`        | A brief description of the rule                                                                                                                             | String                                                                       |
| `DedupPeriodMinutes` | The time period (in minutes) during which similar events of an alert will be grouped together                                                               | `15`,`30`,`60`,`180` (3 hours),`720` (12 hours), or `1440` (24 hours)        |
| `DisplayName`        | A friendly name to show in the UI and alerts. The `RuleID` will be displayed if this field is not set.                                                      | String                                                                       |
| `OutputIds`          | Static destination overrides. These will be used to determine how alerts from this rule are routed, taking priority over default routing based on severity. | List of strings                                                              |
| `Reference`          | The reason this rule exists, often a link to documentation                                                                                                  | String                                                                       |
| `Reports`            | A mapping of framework or report names to values this rule covers for that framework                                                                        | Map of strings to list of strings                                            |
| `Runbook`            | The actions to be carried out if this rule returns an alert, often a link to documentation                                                                  | String                                                                       |
| `SummaryAttributes`  | A list of fields that alerts should summarize.                                                                                                              | List of strings                                                              |
| `Threshold`          | How many events need to trigger this rule before an alert will be sent.                                                                                     | Integer                                                                      |
| `Tags`               | Tags used to categorize this rule                                                                                                                           | List of strings                                                              |
| `Tests`              | Unit tests for this rule.                                                                                                                                   | List of maps                                                                 |

### Policy Specification Reference

Required fields are in **bold**.

A complete list of policy specification fields:

| Field Name                  | Description                                                                                           | Expected Value                                                               |
| --------------------------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **`AnalysisType`**          | Indicates whether this specification is defining a policy or a rule                                   | `policy`                                                                     |
| **`Enabled`**               | Whether this policy is enabled                                                                        | Boolean                                                                      |
| **`FileName`**              | The path (with file extension) to the python policy body                                              | String                                                                       |
| **`PolicyID`**              | The unique identifier of the policy                                                                   | String                                                                       |
| **`ResourceTypes`**         | What resource types this policy will apply to                                                         | List of strings                                                              |
| **`Severity`**              | What severity this policy is                                                                          | One of the following strings: `Info`, `Low`, `Medium`, `High`, or `Critical` |
| `ActionDelaySeconds`        | How long (in seconds) to delay auto-remediations and alerts, if configured                            | Integer                                                                      |
| `AutoRemediationID`         | The unique identifier of the auto-remediation to execute in case of policy failure                    | String                                                                       |
| `AutoRemediationParameters` | What parameters to pass to the auto-remediation, if one is configured                                 | Map                                                                          |
| `Description`               | A brief description of the policy                                                                     | String                                                                       |
| `DisplayName`               | What name to display in the UI and alerts. The `PolicyID` will be displayed if this field is not set. | String                                                                       |
| `Reference`                 | The reason this policy exists, often a link to documentation                                          | String                                                                       |
| `Reports`                   | A mapping of framework or report names to values this policy covers for that framework                | Map of strings to list  of strings                                           |
| `Runbook`                   | The actions to be carried out if this policy fails, often a link to documentation                     | String                                                                       |
| `Tags`                      | Tags used to categorize this policy                                                                   | List of strings                                                              |
| `Tests`                     | Unit tests for this policy.                                                                           | List of maps                                                                 |



### Scheduled Query Specification Reference

Required fields are in **bold**.

A complete list of scheduled query specification fields:

| Field Name           | Description                                                                                                                                                                                                                                                                                                                             | Expected Value    |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| **`AnalysisType`**   | Indicates whether this analysis is a Rule, Policy, Scheduled Query, or global.                                                                                                                                                                                                                                                          | `scheduled_query` |
| **`QueryName`**      | A friendly name to show in the UI.                                                                                                                                                                                                                                                                                                      | String            |
| **`Enabled`**        | Whether this rule is enabled.                                                                                                                                                                                                                                                                                                           | Boolean           |
| `Tags`               | Tags used to categorize this rule.                                                                                                                                                                                                                                                                                                      | List of strings   |
| `Description`        | A brief description of the rule.                                                                                                                                                                                                                                                                                                        | String            |
| **`Query`**          | A query that can run on any backend. If this field is specified, you should not specify a SnowflakeQuery or a AthenaQuery.                                                                                                                                                                                                              | String            |
| **`SnowflakeQuery`** | A query specifically for a Snowflake backend.                                                                                                                                                                                                                                                                                           | String            |
| **`AthenaQuery`**    | A query specifically for Athena.                                                                                                                                                                                                                                                                                                        | String            |
| **`Schedule`**       | <p>The schedule that this query should run. Expressed with a CronExpression or in Rate Minutes. TimeoutMinutes is required to release the query if it takes longer than expected. Note that cron and rate minutes are mutually exclusive. </p><pre><code>CronExpression: "0 0 29 2 *"
  RateMinutes: 0
  TimeoutMinutes: 2</code></pre> | Map               |

### Data Model Specification Reference

Required fields are in **bold**.

A complete list of DataModel specification fields:



| Field Name         | Description                                                                                              | Expected Value            |
| ------------------ | -------------------------------------------------------------------------------------------------------- | ------------------------- |
| **`AnalysisType`** | Indicates whether this specification is defining a rule, policy, data model, or global                   | `datamodel`               |
| **`DataModelID`**  | The unique identifier of the data model                                                                  | String                    |
| `DisplayName`      | What name to display in the UI and alerts. The `DataModelID` will be displayed if this field is not set. | String                    |
| **`Enabled`**      | Whether this data model is enabled                                                                       | Boolean                   |
| `FileName`         | The path (with file extension) to the python DataModel body                                              | String                    |
| **`LogTypes`**     | What log types this policy will apply to                                                                 | Singleton List of strings |
| **`Mappings`**     | Mapping from source field name or method to unified data model field name                                | List of Maps              |

