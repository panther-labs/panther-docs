# Global Helper Functions

A common pattern in programming is to extract repeated code into helper functions, and Panther supports this pattern with the `global` analysis type.

## Built-in Globals

By default, Panther comes with built-in global helpers such as `panther` and `panther_oss_helpers`. `panther` is a default and already set up for you to define your custom logic, and `panther_oss_helpers` provides boilerplate helpers to common [caching](caching.md) and other use cases.

## Using Globals

Import global helpers in your detections by declared `ID` at the top of your analysis function body then call the global as if it were any other python library.

For example:

```python
import panther_oss_helpers


def rule(event):
  return event['name'] == 'test-bucket'


def title(event):
  # Lookup the account name from an account Id
  account_name = panther_oss_helpers.lookup_aws_account_name(event['accountId'])
  return 'Suspicious request made to account ' + account_name
```

## Adding New Globals

New globals can be created from the [Panther Analysis Tool](panther-analysis-tool.md#globals) or in your Panther Console.

To create a new global, navigate to `Analysis` > `Helpers`:

![List Globals](<../../../.gitbook/assets/globals-list (7) (3) (1) (1) (3) (8).png>)

Click `CREATE NEW`:

![Create New Global](<../../../.gitbook/assets/globals-create (7) (1) (1) (1) (3) (8).png>)

Type your Python functions, then click `CREATE`. This global can now be imported in your rules or policies.

## Popular Helpers

#### deep\_get()

Located in [**panther\_base\_helpers**](https://github.com/panther-labs/panther-analysis/blob/master/global\_helpers/panther\_base\_helpers.py)**.**

`deep_get()` can be used to return keys that are nested within the python dictionaries.  This function is useful for safely returning nested keys and avoiding an `AttributeError` when a key is not present.&#x20;

```python
def deep_get(dictionary: dict, *keys, default=None):
    """Safely return the value of an arbitrarily nested map
    Inspired by https://bit.ly/3a0hq9E
    """
    return reduce(
        lambda d, key: d.get(key, default) if isinstance(d, Mapping) else default, keys, dictionary
    )
```

Example:

With the following JSON, the deep\_get function would return the value of result.

```python
{ "outcome": { "reason": "VERIFICATION_ERROR", "result": "FAILURE" }}
```

```python
deep_get(event, "outcome", "result") == "FAILURE"
```

An example of this can be found in the [Geographically Improbable Okta Login](https://github.com/panther-labs/panther-analysis/blob/master/okta\_rules/okta\_geo\_improbable\_access.yml) detection.

#### is\_ip_\__in\_network()

Located in [**panther\_base\_helpers**](https://github.com/panther-labs/panther-analysis/blob/master/global\_helpers/panther\_base\_helpers.py)**.**

`is_ip_in_network()` is a function to check if an IP address is within a list of IP ranges.  This function can be used with a list of known internal networks for added context to the detection.

```python
def is_ip_in_network(ip_addr, networks):
    """Check that a given IP is within a list of IP ranges"""
    return any(ip_address(ip_addr) in ip_network(network) for network in networks)
```

Example:

```python
SHARED_IP_SPACE = [
    "192.168.0.0/16",
]

if is_ip_in_network(event.get("ipaddr"), SHARED_IP_SPACE):
    ...
```

An example can be found in the [OneLogin Active Login Activity](https://github.com/panther-labs/panther-analysis/blob/master/onelogin\_rules/onelogin\_active\_login\_activity.py) detection.

#### pattern\_match()

Located in [**panther\_base\_helpers**](https://github.com/panther-labs/panther-analysis/blob/master/global\_helpers/panther\_base\_helpers.py)**.**

Wrapper around [fnmatch](https://docs.python.org/3/library/fnmatch.html) for basic pattern globs. This can be used when simple pattern matching is needed without the requirement of using regex.

```python
def pattern_match(string_to_match: str, pattern: str):
    """Wrapper around fnmatch for basic pattern globs"""
    return fnmatch(string_to_match, pattern)
```

Example:

With the following JSON the pattern\_match() function would return true.&#x20;

```python
{ "operation": "REST.PUT.OBJECT" }
```

```python
pattern_match(event.get("operation", ""), "REST.*.OBJECT")
```

An example can be found in the [AWS S3 Access Error](https://github.com/panther-labs/panther-analysis/blob/master/aws\_s3\_rules/aws\_s3\_access\_error.py) detection.

#### pattern\_match\_list()

Located in [**panther\_base\_helpers**](https://github.com/panther-labs/panther-analysis/blob/master/global\_helpers/panther\_base\_helpers.py)**.**

Similar to `pattern_match()`, `pattern_match_list()` can check that a string matches any pattern in a given list.

```python
def pattern_match_list(string_to_match: str, patterns: Sequence[str]):
    """Check that a string matches any pattern in a given list"""
    return any(fnmatch(string_to_match, p) for p in patterns)
```

Example:

With the following JSON the pattern\_match\_list() function would return true.&#x20;

```python
{ "userAgent": "aws-sdk-go/1.29.7 (go1.13.7; darwin; amd64) APN/1.0 HashiCorp/1.0 Terraform/0.12.24 (+https://www.terraform.io)" }
```

```python
ALLOWED_USER_AGENTS = {
    "* HashiCorp/?.0 Terraform/*",
    # 'console.ec2.amazonaws.com',
    # 'cloudformation.amazonaws.com',
}

pattern_match_list(event.get("userAgent"), ALLOWED_USER_AGENTS)
```

An example can be found in the [AWS EC2 Manual Security Group Change](https://github.com/panther-labs/panther-analysis/blob/master/aws\_cloudtrail\_rules/aws\_ec2\_manual\_security\_group\_changes.py) detection.

#### aws\_strip\_role\_session\_id()

Located in [**panther\_base\_helpers**](https://github.com/panther-labs/panther-analysis/blob/master/global\_helpers/panther\_base\_helpers.py)**.**

`aws_strip_role_session_id()` strips the session ID our of the arn.&#x20;

```python
def aws_strip_role_session_id(user_identity_arn):
    # The ARN structure is arn:aws:sts::123456789012:assumed-role/RoleName/<sessionId>
    arn_parts = user_identity_arn.split("/")
    if arn_parts:
        return "/".join(arn_parts[:2])
    return user_identity_arn
```

Example:

With the following value, `aws_strip_role_session_id()` would return `arn:aws:sts::123456789012:assumed-role/demo`

```python
{ "arn": "arn:aws:sts::123456789012:assumed-role/demo/sessionName" }
```

```python
aws_strip_role_session_id(user_identity.get("arn", ""))
```

An example can be found in the [AWS Unauthorized API Call](https://github.com/panther-labs/panther-analysis/blob/master/aws\_cloudtrail\_rules/aws\_unauthorized\_api\_call.py) detection.

####
