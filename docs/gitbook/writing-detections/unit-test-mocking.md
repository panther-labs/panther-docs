# Unit Test Mocking

{% hint style="info" %}
This feature is available in Panther version 1.18
{% endhint %}

Panther features a simple framework to allow basic mocking for unit tests. 

When writing a detection that requires an external API call, mocks can be utilized to mimic the server response in the unit tests without having to actually execute an API call. 

Mocks are defined by a given `Mock Name` and `Return Value` which respectively denote the name of the object to patch and the `string` to be returned when the patched object is invoked.

Added mocks are defined on the unit test level allowing users to define different mocks for each unit test.  
The section to add mocks to a unit test are located directly underneath the unit test resource definition

![](../.gitbook/assets/image%20%282%29.png)

Mocks are allowed on the **global** and **built-in** python namespaces.   
This includes:

1. Imported Modules and Functions
2. Global Variables
3. Built-in Python Functions
4. User-Defined Functions

Python provides two distinct forms of importing which can be mocked as such:

* `import package` 
  * Mock Name: `package`
* `from package import module`
  * Mock Name: `module`

### Example Unit Test Mocking

{% hint style="info" %}
This example is based on the [AWS Config Global Resources](https://github.com/panther-labs/panther-analysis/blob/master/aws_config_policies/aws_config_global_resources.py) detection.
{% endhint %}

The detection utilizes a global helper function `resource_lookup` function from `panther_oss_helpers` which queries the `resources-api` and returns the resource attributes.   
However, the unit test should be able to be performed without any external API calls.

![Example Data Without Mocking](../.gitbook/assets/image%20%2815%29.png)

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
**Used Return Value:**   
{ "AccountId": "012345678910", "Name": "Default", "RecordingGroup": { "AllSupported": true, "IncludeGlobalResourceTypes": true, "ResourceTypes": null }, "Region": "us-east-1", "ResourceId": "012345678910:us-east-1:AWS.Config.Recorder", "ResourceType": "AWS.Config.Recorder", "RoleARN": "arn:aws:iam::012345678910:role/PantherAWSConfig", "Status": { "LastErrorCode": null, "LastErrorMessage": null, "LastStartTime": "2018-10-05T22:45:01.838Z", "LastStatus": "SUCCESS", "LastStatusChangeTime": "2021-05-28T17:45:14.916Z", "LastStopTime": null, "Name": "Default", "Recording": true }, "Tags": null, "TimeCreated": null }
{% endhint %}

![Missing Mock Case](../.gitbook/assets/image%20%2819%29.png)

While this resource should be compliant, the unit test fails.   
Detections that do not expect a `string` to be returned require a small tweak for mocks.

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

![Successful Unit Test Mock](../.gitbook/assets/image%20%2817%29.png)

