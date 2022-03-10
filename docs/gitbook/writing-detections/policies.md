# Policies

Panther enables easy scanning and evaluation of cloud infrastructure configurations.

**Policies** are Python3 functions used to identify misconfigured infrastructure and generate alerts for your team.

## Policy Components

* A `policy` function with a `resource` argument that returns `True` if the resource is compliant and the policy should not send an alert, or `False` if the resource is not complaint and the policy should send an alert
* Metadata containing context for triage
* An association with a specific Resource Type

As an example, the policy below checks if an S3 bucket allows public read access:

```python
# A list of grantees that represent public access
GRANTEES = {
    'http://acs.amazonaws.com/groups/global/AuthenticatedUsers',
    'http://acs.amazonaws.com/groups/global/AllUsers'
}
PERMISSIONS = {'READ'}


def policy(resource):
    for grant in resource['Grants']:
        if grant['Grantee']['URI'] in GRANTEES and grant[
                'Permission'] in PERMISSIONS:
            return False

    return True
```

## Policy Writing Workflow

Panther policies can be written, tested, and deployed either with the Panther Console or the [panther\_analysis\_tool](https://github.com/panther-labs/panther\_analysis\_tool) CLI utility.

Each policy takes a `resource` input of a given resource type from the [supported resources](../resources/) page.

### Policy Body

The policy body MUST:

* Be valid Python3
* Define a `policy()` function that accepts one argument
* Return a `bool` from the policy function

```python
def policy(resource):
  return True
```

The Python body SHOULD:

* Name the argument to the `policy()` function `resource`

The Python body MAY:

* Import standard Python3 libraries
* Import from the user defined `aws_globals` module
* Import from the Panther defined `panther` module
* Define additional helper functions as needed
* Define variables and classes outside the scope of the rule function

Using the schemas in [supported resources](../resources/) provides details on all available fields in resources. Top level keys are always present, although they may contain `NoneType` values.

#### Example Policy

For example, let's write a Policy on an [IAM Password Policy](../resources/aws/password-policy.md) resource:

```javascript
{
    "AccountId": "123456789012",
    "AllowUsersToChangePassword": true,
    "AnyExist": true,
    "ExpirePasswords": true,
    "HardExpiry": null,
    "MaxPasswordAge": 90,
    "MinimumPasswordLength": 14,
    "Name": "AWS.PasswordPolicy",
    "PasswordReusePrevention": 24,
    "Region": "global",
    "RequireLowercaseCharacters": true,
    "RequireNumbers": true,
    "RequireSymbols": true,
    "RequireUppercaseCharacters": true,
    "ResourceId": "123456789012::AWS.PasswordPolicy",
    "ResourceType": "AWS.PasswordPolicy",
    "Tags": null,
    "TimeCreated": null
}
```

This example policy alerts when the password policy does not enforce a maximum password age:

```python
def policy(resource):
    if resource['MaxPasswordAge'] is None:
        return False
    return resource['MaxPasswordAge'] <= 90
```

In the `policy()` body, returning a value of `True` indicates the resource is compliant and no alert should be sent. Returning a value of `False` indicates the resource is non-compliant.

## First Steps with Policies

When starting your policy writing/editing journey, your team should decide between a UI or CLI driven workflow.

Then, configure the built in policies by searching for the `Configuration Required` tag. These policies are designed to be modified by you, the security professional, based on your organization's business logic.

## Writing Policies in the Panther UI

Navigate to Cloud Security > Policies, and click `Create New` in the top right corner. You have the option of creating a single new policy, or uploading a zip file containing policies created with the `panther_analysis_tool`. Clicking single will take you to the policy editor page.

![Policy Editor](<../../../.gitbook/assets/policy-creation1 (5) (1) (1) (3) (3).png>)

### Set Attributes

Keeping with the Password Policy example above, set all the necessary rule attributes:

![Attributes Set](<../../../.gitbook/assets/policy-creation2 (6) (1) (1) (3) (3).png>)

### Write Policy Body

Then write our policy logic in the `policy()` function.

![Policy Body](<../../../.gitbook/assets/policy-creation3 (7) (1) (1) (3) (4).png>)

### Configure Tests

Next, configure test cases to ensure our policy works as expected:

![Unit Tests](<../../../.gitbook/assets/policy-creation4 (7) (2) (1) (1) (3) (8).png>)

## Policy Writing Tips

### Constructing Test Resources

Manually building test cases is tedious and error prone. We suggest one of two alternatives:

1. Open `Cloud Security` > `Resources`, and apply a filter of the resource type you intend to emulate in your test. Select a resource in your environment, and on the `Attributes` card you can copy the full JSON representation of that resource by selecting copy button next to the word `root`.
2. Open the Panther [Resources documentation](../resources/), and navigate to the section for the resource you are trying to emulate. Copy the provided example resource.

Paste this in to the resource editor if you're working in the web UI, or into the `Resource` field if you are working locally. Now you can manually modify the fields relevant to your policy and the specific test case you are trying to emulate.

Option 1 is best when it is practical, as this can provide real test data for your policies. Additionally, it is often the case that you are writing/modifying a policy specifically because of an offending resource in your account. Using that exact resource's JSON representation as your test case can guarantee that similar resources will be caught by your policy in the future.

### Debugging Exceptions

Debugging exceptions can be difficult, as you do not have direct access to the python environment running the policies.

When you see a policy that is showing the state `Error` on a given resource, that means that the policy threw an exception. The best method for troubleshooting these errors is to use option 1 in the **Constructing test resources** section above and create a test case from the resource causing the exception.

Running this test case either locally or in the web UI should provide more context for the issue, and allow you to rapidly modify the policy to debug the exception without having to run the policy against all resources in your environment.

{% hint style="warning" %}
_**Note: Anything printed to stdout in the python logic will end up in CloudWatch.  For SaaS/CPaaS customers, panther engineers can see these CloudWatch logs during routine application monitoring.**_
{% endhint %}

