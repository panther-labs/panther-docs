# Rules

Panther **Rules** are Python3 functions used to identify suspicious activity and generate helpful signal for your team.

Common log events analyzed with rules include:

* Authorization or authentication
* API calls
* Network traffic
* Running processes
* Alerts from IDS

Rules can be written in the UI or programmatically with the Panther Analysis Tool.

## Rule Components

Rules contain the following Python functions to control the detection, deduplication of events, and generation of alert titles.

All the functions take as argument the currently processed `event`.

* A required `rule` function that always returns a `boolean`. It returns `True` if the rule should send an alert or `False` if it should not send an alert
* An optional `title` function that must always return a `string`. It is used to generate the alert title sometimes interpolating values from the event. If no title function is specified, or it throws an exception, we use the Rule Id
* An optional `dedup` function that must always return a `string`. It is used to control how events are grouped in alerts. If no dedup function is specified, or it throws an exception, we group events together based on the output of the title function or the Rule Id
* An optional `alert_context` that must always return a `Dict[str, Any]`. It is used to specify the additional payload that will be sent along with the alert to the destination. If no alert\_context function is specified, or it throws an exception, we send an empty `Dict`

Additionally, rules contain metadata for setting the severity, associating [specific log type\(s\)](../data-onboarding/supported-logs/), and defining unit tests.

For example, the rule below validates if unauthenticated access has occurred on an S3 bucket:

```python
# This rule applies to S3 Server Access Logs
def rule(event):
  if event.get('bucket') not in {'my-set-of-authenticated-buckets'}:
    return False
  return 'requester' not in event

# This rule groups alert events by the bucket name
def dedup(event):
  return event.get('bucket')

# Alerts will have a title like `Unauthenticated Access to S3 Bucket my-super-secret-data`
def title(event):
  return 'Unauthenticated Access to S3 Bucket  {}'.format(event.get('bucket'))

# Alert context will contain the bucket name and the requester information
def alert_context(event):
  return {'bucket': event.get('bucket'), 'requester': event.get('requester')}
```

## Workflow

Panther rules can be written, tested, and deployed either with the UI or the [panther\_analysis\_tool](panther-analysis-tool.md).

Each rule takes an `event` input of a given log type from the [supported logs](../data-onboarding/supported-logs/) page.

### Rule Body

The rule body must:

* Be valid Python3
* Define a `rule()` function that accepts one argument, `event`
* Return `True` \(send an alert\) or `False` \(known good behavior\) from the rule function

```python
def rule(event):
  if event['energy_level'] > 9000:
    return True
```

The Python body may also:

* Import any standard Python3 libraries
* Import from the user-defined global helpers
* Define additional helper functions as needed
* Define variables and classes outside the scope of the rule function
* Define a `dedup()` function that accepts an `event` argument and returns a `string`
* Define a `title()` function that accepts an `event` argument and returns a `string`

Referencing the [supported logs](../data-onboarding/supported-logs/) page provides details on all available fields in events.

#### Example Rule

Let's write a rule on a sample [NGINX Access log](../data-onboarding/supported-logs/nginx.md):

```javascript
{
  "bodyBytesSent": 193,
  "httpReferer": "https://domain1.com/?p=1",
  "httpUserAgent": "Chrome/80.0.3987.132 Safari/537.36",
  "remoteAddr": "180.76.15.143",
  "request": "GET /admin-panel/ HTTP/1.1",
  "status": 200,
  "time": "2019-02-06 00:00:38 +0000 UTC"
}
```

This example rule would alert upon successful admin panel logins:

```python
def rule(event):
  # A common pattern is to exit the rule early to avoid unnecessary processing.
  #
  # It's also a common pattern to only proceed if requests are successful.
  if event.get('status') != 200:
    return False

  # Send an alert if a user logs into the admin panel.
  return 'admin-panel' in event.get('request')
```

In the `rule()`, returning a value of `True` indicates an alert should send. Returning a value of `False` indicates the log is not suspicious.

{% hint style="info" %}
When accessing non-required event fields, it's recommended to use `event.get('<field-name>')`
{% endhint %}

### Alert Deduplication

To avoid alert fatigue, events are grouped together \(by a deduplication string\) within a given time period \(deduplication period\), and associated with a single `AlertID`.

By default, events are merged on the key `defaultDedupString:${RuleID}` over a period of 1 hour, but can be set to:

* `15m`
* `30m`
* `1h`
* `3h`
* `12h`
* `24h`

To modify the deduplication key, use the `dedup()` function in your rule body:

```python
def dedup(event):
  return event.get('remoteAddr')
```

{% hint style="info" %}
The length of the `dedup` string is limited to `1000` characters and the rest will be truncated
{% endhint %}

The same parsed log `event` is passed into this function, and you may use any logic desired to calculate the `dedupString`.

{% hint style="info" %}
If a Falsy value is returned from `dedup()`, then the default string will be used
{% endhint %}

### Alert Threshold

In some cases you may want a rule to trigger only if N events have matched during the rule's deduplication period. You can achieve this by configuring the `Threshold` field, which is set to `1` by default.

### Alert Titles

Alert titles sent to our destinations are the default value of `New Alert: ${Display Name or ID}`. To override this message, use the `title()` function in your rule:

```python
def title(event):
  return 'Admin Panel Login'
```

The title can also be interpolated by using event attributes:

```python
def title(event):
  return 'Successful Admin Panel Login from IP {}'.format(
    event.get('remoteAddr', '<IP_NOT_FOUND>'))
```

{% hint style="info" %}
The second argument to `event.get()` is the default value returned if the key is not found. By supplying a value here, we avoid a title of 'Successful Admin Panel Login from IP None'
{% endhint %}

## First Steps with Rules

When beginning your rule writing/editing journey, your team should decide between a UI or CLI driven workflow.

Then, configure the built in rules by searching for the `Configuration Required` tag. These rules are designed to be modified by you, the security professional, based on your organization's internal business logic.

{% hint style="info" %}
See [Alert Field Overrides](alert-field-overrides.md) for more advanced detection functionality
{% endhint %}

## Writing Rules with the Panther Analysis Tool

The [panther\_analysis\_tool](panther-analysis-tool.md) is a Python command line interface for testing, packaging, and deploying Panther Policies and Rules. This enables teams to work in a more developer oriented workflow and track detections with version control systems such as `git`.

