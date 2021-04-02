# Data Models

Data Models provide a way to configure a set of unified fields across all log types.

## Data Model Motivation

Suppose you want to check for a particular source ip address in all events that log network traffic. These LogTypes might not only span different categories \(DNS, Zeek, Apache, etc.\), but also different vendors. Without a common logging standard, each of these LogTypes may represent the source ip by a different name, such as `ipAddress`, `srcIP`, or `ipaddr`. The more LogTypes you want to monitor, the more complex and cumbersome this simple check becomes:

```python
(event.get('ipAddress') == '127.0.0.1' or 
event.get('srcIP') == '127.0.0.1' or 
event.get('ipaddr') == '127.0.0.1')
```

If instead we define a Data Model for each of these LogType's, we can translate the unified data model field name to the LogType field name and our logic simplifies to:

```python
event.udm('source_ip') == '127.0.0.1'
```

## Built-in Data Models

By default, Panther comes with built-in data models for several log types, such as `AWS.S3ServerAccess`, `AWS.VPCFlow`, and `Okta.SystemLog`. All currently supported data models can be found [here](https://github.com/panther-labs/panther-analysis/tree/master/data_models).

## Adding New Data Models

New data models are added in the Panther UI or via the [Panther Analysis Tool](panther-analysis-tool.md). Each log type can only have one enabled data model specified. If you want to change or update an existing data model, `disable` the existing one, and create a new, enabled one.

### Add New Data Model - UI

To create a new Data Model, navigate to `Analysis` &gt; `Data Models`:

![List Data Models](../.gitbook/assets/data-model-list.png)

Click `CREATE NEW`:

![Create New Data Model](../.gitbook/assets/data-model-create.png)

Set all the necessary Data Model attributes, such as the ID, DisplayName, the applicable LogType, the field `Mappings`, and any python logic. Click `SAVE`. This data model can now be accessed in your rule logic with the `event.udm()` method

### Add New Data Model - Panther Analysis Tool

To add a new data model using the `panther_analysis_tool`, first create your DataModel specification file \(e.g. `data_models/aws_cloudtrail_datamodel.yml`\):

```yaml
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

Then, if any `Method`s are defined, create an associated python file \(`data_models/aws_cloudtrail_datamodel.py`\):

```python
from panther_base_helpers import deep_get

def get_event_type(event):
    if event.get('eventName') == 'ConsoleLogin' and deep_get(event, 'userIdentity', 'type') == 'IAMUser':
        if event.get('responseElements', {}).get('ConsoleLogin') == 'Failure':
            return "failed_login"
        if event.get('responseElements', {}).get('ConsoleLogin') == 'Success':
            return "successful_login"
    return None
```

{% hint style="info" %}
The `Filename` specification field is _required_ if a `Method` is defined in a mapping. If `Method` is not used in any `Mappings`, no python file is required.
{% endhint %}

Finally, use this data model in a rule by:

* Adding the LogType under the Rule specification `LogType` field 
* Add the LogType to all the Rule's `Test` cases, in the `p_log_type` field
* Leveraging the `event.udm()` method in the Rule's python logic:

```text
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

### DataModel Specification Reference

A complete list of DataModel specification fields:

| Field Name | Required | Description | Expected Value |
| :--- | :--- | :--- | :--- |
| `AnalysisType` | Yes | Indicates whether this specification is defining a rule, policy, data model, or global | `datamodel` |
| `DataModelID` | Yes | The unique identifier of the data model | String |
| `DisplayName` | No | What name to display in the UI and alerts. The `DataModelID` will be displayed if this field is not set. | String |
| `Enabled` | Yes | Whether this data model is enabled | Boolean |
| `FileName` | No | The path \(with file extension\) to the python DataModel body | String |
| `LogTypes` | Yes | What log types this policy will apply to | Singleton List of strings |
| `Mappings` | Yes | Mapping from source field name or method to unified data model field name | List of Maps |

### DataModel Mappings

Mappings translate LogType fields to a unified data model fields. Each mapping entry must define a unified data model field name \(`Name`\) and either a Path \(`Path`\) or a method \(`Method`\). The `Path` can be a simple field name or a JSON Path. The method must be implemented in the file listed in the data model specification `Filename` field.

```text
Mappings:
  - Name: source_ip
    Path: srcIp
  - Name: user
    Path: $.events[*].parameters[?(@.name == 'USER_EMAIL')].value
  - Name: event_type
    Method: get_event_type
```

{% hint style="info" %}
More information about jsonpath-ng can be found [here](https://pypi.org/project/jsonpath-ng/).
{% endhint %}

## Unified Data Model Field Reference

The initial set of supported unified data model fields are described below.

| Unified Data Model Field Name | Description |
| :--- | :--- |
| `actor_user` | ID or username of the user whose action triggered the event. |
| `assigned_admin_role` | Admin role ID or name assigned to a user in the event. |
| `destination_ip` | Destination IP for the traffic |
| `destination_port` | Destination port for the traffic |
| `event_type` | Custom description for the type of event. Out of the box support for event types can be found in the global, `panther_event_type_helpers.py`. |
| `http_status` | Numeric http status code for the traffic |
| `source_ip` | Source IP for the traffic |
| `source_port` | Source port for the traffic |
| `user_agent` | User agent associated with the client in the event. |
| `user` | ID or username of the user that was acted upon to trigger the event. |

## Leveraging Existing Data Models

Rules can be upated to use unified data model field names by leveraging the `event.udm()` method. For example:

```python
def rule(event):
  return event.udm('source_ip') in DMZ_NETWORK
def title(event):
  return 'Suspicious request originating from ip: ' + event.udm('source_ip')
```

Update the rule specification to include the pertinent LogTypes:

```text
AnalysisType: rule 
Filename: example_rule.py
Description: A rule that uses datamodels
Severity: High
RuleID: Example.Rule
Enabled: true
LogTypes:
  - Logtype.With.DataModel
  - Another.Logtype.With.DataModel
```

