# Panther Analysis Tool

The `panther_analysis_tool` (PAT) is an [open source](https://github.com/panther-labs/panther\_analysis\_tool) utility for testing, packaging, and deploying Panther detections from source code.

It's designed for developer-centric workflows such as managing your Panther analysis packs programmatically or within CI/CD pipelines.

## Installation

PAT is installable with a Python package into your current environment:

```bash
pip3 install panther_analysis_tool
```

## File Organization

{% hint style="info" %}
It's best practice to create a fork of Panther's [open source](https://github.com/panther-labs/panther-analysis) analysis repository.
{% endhint %}

To get started, navigate to the locally checked out copy of your custom detections.

We recommend creating folders based on log/resource type, such as `suricata_rules` or `aws_s3_policies`. Use the open source [Panther Analysis](https://github.com/panther-labs/panther-analysis) packs as a reference.

Each analysis consists of:

1. A Python file containing your detection/audit logic
2. A valid YAML or JSON specification file containing attributes of the detection. By convention, we give this file the same name as the Python file.

## Writing Rules

Rules are Python functions to detect suspicious behaviors. Returning a value of `True` indicates suspicious activity, which triggers an alert.

First, [write your rule](rules.md) and save it (in your folder of choice) as `my_new_rule.py`:

```python
def rule(event):
  return 'prod' in event.get('hostName')
```

Then, create a specification file using the template below:

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

When this rule is uploaded, each of the fields you would normally populate in the UI will be auto-filled.

### Rule Specification Reference

A complete list of rule specification fields:

| Field Name           | Required | Description                                                                                                                                                 | Expected Value                                                               |
| -------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `AnalysisType`       | Yes      | Indicates whether this analysis is a rule, policy, or global                                                                                                | `rule`                                                                       |
| `Enabled`            | Yes      | Whether this rule is enabled                                                                                                                                | Boolean                                                                      |
| `FileName`           | Yes      | The path (with file extension) to the python rule body                                                                                                      | String                                                                       |
| `RuleID`             | Yes      | The unique identifier of the rule                                                                                                                           | String                                                                       |
| `LogTypes`           | Yes      | The list of logs to apply this rule to                                                                                                                      | List of strings                                                              |
| `Severity`           | Yes      | What severity this rule is                                                                                                                                  | One of the following strings: `Info`, `Low`, `Medium`, `High`, or `Critical` |
| `Description`        | No       | A brief description of the rule                                                                                                                             | String                                                                       |
| `DedupPeriodMinutes` | No       | The time period (in minutes) during which similar events of an alert will be grouped together                                                               | `15`,`30`,`60`,`180` (3 hours),`720` (12 hours), or `1440` (24 hours)        |
| `DisplayName`        | No       | A friendly name to show in the UI and alerts. The `RuleID` will be displayed if this field is not set.                                                      | String                                                                       |
| `OutputIds`          | No       | Static destination overrides. These will be used to determine how alerts from this rule are routed, taking priority over default routing based on severity. | List of strings                                                              |
| `Reference`          | No       | The reason this rule exists, often a link to documentation                                                                                                  | String                                                                       |
| `Reports`            | No       | A mapping of framework or report names to values this rule covers for that framework                                                                        | Map of strings to list of strings                                            |
| `Runbook`            | No       | The actions to be carried out if this rule returns an alert, often a link to documentation                                                                  | String                                                                       |
| `SummaryAttributes`  | No       | A list of fields that alerts should summarize.                                                                                                              | List of strings                                                              |
| `Threshold`          | No       | How many events need to trigger this rule before an alert will be sent.                                                                                     | Integer                                                                      |
| `Tags`               | No       | Tags used to categorize this rule                                                                                                                           | List of strings                                                              |
| `Tests`              | No       | Unit tests for this rule.                                                                                                                                   | List of maps                                                                 |

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

{% hint style="info" %}
Try to cover as many test cases as possible, including both true and false positives.
{% endhint %}

## Writing Policies

Policies are Python functions to detect misconfigured cloud infrastructure. Returning a value of `True` indicates this resource is valid and properly configured. Returning `False` indicates a policy failure, which triggers an alert.

First, [write your policy](policies.md) and save it (in your folder of choice) as `my_new_policy.py`:

```python
def polcy(resource):
  return resource['Region'] != 'us-east-1'
```

Then, create a specification file using the template below:

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

### Policy Specification Reference

A complete list of policy specification fields:

| Field Name                  | Required | Description                                                                                           | Expected Value                                                               |
| --------------------------- | -------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `AnalysisType`              | Yes      | Indicates whether this specification is defining a policy or a rule                                   | `policy`                                                                     |
| `Enabled`                   | Yes      | Whether this policy is enabled                                                                        | Boolean                                                                      |
| `FileName`                  | Yes      | The path (with file extension) to the python policy body                                              | String                                                                       |
| `PolicyID`                  | Yes      | The unique identifier of the policy                                                                   | String                                                                       |
| `ResourceTypes`             | Yes      | What resource types this policy will apply to                                                         | List of strings                                                              |
| `Severity`                  | Yes      | What severity this policy is                                                                          | One of the following strings: `Info`, `Low`, `Medium`, `High`, or `Critical` |
| `ActionDelaySeconds`        | No       | How long (in seconds) to delay auto-remediations and alerts, if configured                            | Integer                                                                      |
| `AutoRemediationID`         | No       | The unique identifier of the auto-remediation to execute in case of policy failure                    | String                                                                       |
| `AutoRemediationParameters` | No       | What parameters to pass to the auto-remediation, if one is configured                                 | Map                                                                          |
| `Description`               | No       | A brief description of the policy                                                                     | String                                                                       |
| `DisplayName`               | No       | What name to display in the UI and alerts. The `PolicyID` will be displayed if this field is not set. | String                                                                       |
| `Reference`                 | No       | The reason this policy exists, often a link to documentation                                          | String                                                                       |
| `Reports`                   | No       | A mapping of framework or report names to values this policy covers for that framework                | Map of strings to list  of strings                                           |
| `Runbook`                   | No       | The actions to be carried out if this policy fails, often a link to documentation                     | String                                                                       |
| `Tags`                      | No       | Tags used to categorize this policy                                                                   | List of strings                                                              |
| `Tests`                     | No       | Unit tests for this policy.                                                                           | List of maps                                                                 |

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

{% hint style="info" %}
The value of `Resource` can be a JSON object copied directly from the Policies > Resources explorer.
{% endhint %}

## Unit Test Mocking

Both policy and rule tests support unit test mocking. In order to configure mocks for a particular test case, add the `Mocks` key to your test case. The `Mocks` key is used to define a list of functions you want to mock, and the value that should be returned when that function is called. Multiple functions can be mocked in a single test. For example, if we have a rule test and want to mock the function `get_counter` to always return a `1` and the function `geoinfo_from_ip` to always return a specific set of geo IP info, we could write our unit test like this:

```
Tests:
  -
    Name: Test With Mock
    LogType: LogType.Custom
    ExpectedResult: true
    Mock:
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

## Running Tests

Use the Panther Analysis Tool to load the defined specification files and evaluate unit tests locally:

```bash
panther_analysis_tool test --path <folder-name>
```

To filter rules or policies based on certain attributes:

```bash
panther_analysis_tool test --path <folder-name> --filter RuleID=Category.Behavior.MoreInfo
```

## Globals

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

## Uploading to Panther

{% hint style="info" %}
**Panther SaaS customers**: Please file a support ticket to gain upload access to your Panther environment.
{% endhint %}

Make sure to configure your environment with valid AWS credentials prior to running the command below. This command will upload based on the exported value of `AWS_REGION`.

To upload your analysis packs to your Panther Console, run the following command:

```bash
panther_analysis_tool upload --path <path-to-your-rules> --out tmp
```

Analysis with the same ID are overwritten. Additionally, locally deleted rules/policies will not automatically be deleted in the database and must be removed manually. We recommend setting the Enabled property to false instead of deleting policies or rules for CLI driven workflows.

## Pack Source

See the [Detection Packs documentation](https://docs.runpanther.io/writing-detections/detection-packs) for details on using `panther_analysis_tool` with detection packs and pack sources.
