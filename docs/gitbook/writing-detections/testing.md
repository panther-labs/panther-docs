# Testing

## Concept

Detection Testing ensures that detections will behave as expected and will generate alerts once deployed and running. Testing also promotes reliability over time as code evolves and protects against regressions.

Testing works by defining a "test" input to be used as the `event` for the detection and expecting whether or not an alert would be generated. A Detection can have zero or many test cases and it's recommended to have at least two (one false positive and one true positive).

Tests can be defined either in the Panther Console or programmatically with the [Panther Analysis Tool](panther-analysis-tool.md).

Keeping with the previous example (in [Rules](rules.md)), let's write two tests for this detection:

```python
def rule(event):
  return event.get('status') == 200 and 'admin-panel' in event.get('request')

    
def title(event):
  return f"Successful admin panel login detected from {event.get('remoteAddr')}"


def dedup(event):
  return event.get('remoteAddr')
```

`Name`: Successful admin-panel Logins

`Test event should trigger an alert`: Yes

```javascript
{
  "httpReferer": "https://domain1.com/?p=1",
  "httpUserAgent": "Chrome/80.0.3987.132 Safari/537.36",
  "remoteAddr": "180.76.15.143",
  "request": "GET /admin-panel/ HTTP/1.1",
  "status": 200,
  "time": "2019-02-06 00:00:38 +0000 UTC"
}
```

`Name`: Errored requests to the access-policy page

`Test event should trigger an alert`: No

```javascript
{
  "httpReferer": "https://domain1.com/?p=1",
  "httpUserAgent": "Chrome/80.0.3987.132 Safari/537.36",
  "remoteAddr": "180.76.15.143",
  "request": "GET /access-policy/ HTTP/1.1",
  "status": 500,
  "time": "2019-02-06 00:00:38 +0000 UTC"
}
```

{% hint style="info" %}
Use as many combinations as you would like to ensure the highest reliability with your detections.
{% endhint %}

## Mocks

Panther's testing framework also allows for basic Python call mocking.

When writing a detection that requires an external API call, mocks can be utilized to mimic the server response in the unit tests without having to actually execute an API call.&#x20;

Mocks are defined by a given `Mock Name` and `Return Value` which respectively denote the name of the object to patch and the `string` to be returned when the patched object is invoked.

Added mocks are defined on the unit test level allowing users to define different mocks for each unit test. The section to add mocks to a unit test is located directly underneath the unit test resource definition:

![](<../.gitbook/assets/image (2).png>)

Mocks are allowed on the **global** and **built-in** python namespaces, this includes:

1. Imported Modules and Functions
2. Global Variables
3. Built-in Python Functions
4. User-Defined Functions

Python provides two distinct forms of importing, which can be mocked as such:

* `import package`&#x20;
  * Mock Name: `package`
* `from package import module`
  * Mock Name: `module`

{% hint style="info" %}
This example is based on the [AWS Config Global Resources](https://github.com/panther-labs/panther-analysis/blob/master/aws\_config\_policies/aws\_config\_global\_resources.py) detection.
{% endhint %}

The detection utilizes a global helper function `resource_lookup` from `panther_oss_helpers` which queries the `resources-api` and returns the resource attributes. However, the unit test should be able to be performed without any external API calls.

![Example Data Without Mocking](<../.gitbook/assets/image (15).png>)

This test fails as there is no corresponding resource mapping to the generic example data.

#### Diving Into the Detection

```python
# --- Snipped ---
from panther_oss_helpers import resource_lookup
# --- Snipped ---
def policy(resource):
    # --- Snipped ---
    for recorder_name in resource.get("Recorders", []):
        recorder = resource_lookup(recorder_name)
        resource_records_global_resources = bool(
            deep_get(recorder, "RecordingGroup", "IncludeGlobalResourceTypes")
            and deep_get(recorder, "Status", "Recording")
        )
        if resource_records_global_resources:
            return True
    return False
    # --- Snipped ---
```

The detection uses the `from panther_oss_helpers import resource_lookup` convention which means the mock should be defined for the `resource_lookup` function.

Mocks provide a way to leverage real world data to test the detection logic.

{% hint style="info" %}
**Used Return Value:**&#x20;

{ "AccountId": "012345678910", "Name": "Default", "RecordingGroup": { "AllSupported": true, "IncludeGlobalResourceTypes": true, "ResourceTypes": null }, "Region": "us-east-1", "ResourceId": "012345678910:us-east-1:AWS.Config.Recorder", "ResourceType": "AWS.Config.Recorder", "RoleARN": "arn:aws:iam::012345678910:role/PantherAWSConfig", "Status": { "LastErrorCode": null, "LastErrorMessage": null, "LastStartTime": "2018-10-05T22:45:01.838Z", "LastStatus": "SUCCESS", "LastStatusChangeTime": "2021-05-28T17:45:14.916Z", "LastStopTime": null, "Name": "Default", "Recording": true }, "Tags": null, "TimeCreated": null }
{% endhint %}

![Missing Mock Case](<../.gitbook/assets/image (19) (1).png>)

While this resource should be compliant, the unit test fails. \
Detections that do not expect a `string` to be returned requires a small tweak for mocks.

In order to get this unit test working as expected, the following modifications need to be made:

```python
# --- Snipped ---
import json
# Another option is to use: from ast import literal_eval
# --- Snipped ---
def policy(resource):
    # --- Snipped ---
        recorder = resource_lookup(recorder_name)
        if isinstance(recorder, str):
            recorder = json.loads(recorder)
    # --- Snipped ---
```

Once this modification is added, you can now test the detection logic with real data!

![Successful Unit Test Mock](<../.gitbook/assets/image (17).png>)

#### Mocks from the CLI

Unit test mocking is also supported with CLI based workflows for writing detections. For details on adding unit test mocks to a CLI based detection, see the [unit test mocking](panther-analysis-tool.md#unit-test-mocking) section of the Panther Analysis Tool docs.
