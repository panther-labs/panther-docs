# Rules

**Rules** are Python functions for detecting suspicious activity in logs and generating alerts. Common examples of rules include analyzing for:

* Authentication from unknown or unexpected locations
* Sensitive API calls, such as administrative changes to SaaS services
* Network traffic access to sensitive data sources, such as databases or virtual machines
* New suspicious entries added into a system's scheduled tasks, like cron
* Alerts generated from NIDS, HIDS, or other security systems

Rules may be written directly with Panther's Rule Editor or locally in an IDE and uploaded with the [Panther Analysis Tool](panther-analysis-tool.md).

{% hint style="info" %}
Although similar, Rules apply to security logs while [Policies](policies.md) apply to cloud resources.
{% endhint %}

## Concepts

Rules run on a defined set of log types such as Okta, Box, or your own [custom data](../data-onboarding/custom-log-types/).

Rules analyze one event at a time and can use thresholds, deduplication, and event grouping to analyze logs within windows of time. By default, each rule has a threshold of `1` and a deduplication period of `1h`, meaning all events returning `True` from a rule would be appended to the alert within the hour after first being generated.

As an example, let's write a rule to send an alert when an admin panel is accessed on a web server. The following NGINX log below will be used:

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

A basic rule would look like this:

* A `rule` function that looks for 200 \(OK\) web requests to any URL with the `admin-panel` string
* A `title` to say that admin panel logins have been logged into from a specific IP address
* A `dedup` function to group all events by the same IP address

```python
def rule(event):
  return event.get('status') == 200 and 'admin-panel' in event.get('request')

    
def title(event):
  return f"Successful admin panel login detected from {event.get('remoteAddr')}"


def dedup(event):
  return event.get('remoteAddr')
```

Then, the following would occur:

1. An alert would be generated and sent to the set of associated [destinations](../destinations/), which by default are based on the rule severity
2. The alert would say `Successful admin panel login detected from 180.76.15.143` 
3. Similar events with the same dedup string of `180.76.15.143` would be appended to the alert
4. The recipient of the alert could then check Panther to view all alert metadata, a summary of the events, and run SQL over all of the events to perform additional analysis 

{% hint style="success" %}
A unique alert will be generated for each unique deduplication string, which in this case, is the IP of the requestor. 
{% endhint %}

In the sections below, each concept above will be expanded upon.

## Real-Time vs Scheduled

Panther has two core mechanisms of analyzing data with rules, real-time and scheduled.

Real-Time rules are the default mechanism of analyzing data sent to Panther and have the benefit of low-latency detection and alerting. High-signal logs that not require joins with other data are best suited for real-time detection.

For querying windows of time further in the past, running statistical analysis over data, or joining separate data streams, use a [scheduled ](../data-analytics/scheduled-queries.md)rule. This works by running a SQL query on a defined interval \(or cron\) and using the result of that query as input into a Python-based rule. All other functionality described on this page remains the same.

## Functions

Panther Rules contain several Python3 functions that control the analysis logic, generated alert title, event grouping, routing of alerts, and metadata overrides.

Rules are very customizable and can import from standard Python libraries or [global helpers](globals.md). Additional functions, variables, or classes may also be defined outside of the functions defined below for advanced use or cleaner code. 

{% hint style="info" %}
Read [Runtime Environment](run-time.md) to learn more about the available libraries and how to add custom or third-party ones. 
{% endhint %}

Each function listed below takes a single argument of `event`

| Name | Description | Return Value | Default Return Value |
| :--- | :--- | :--- | :--- |
| `rule` | The main logic to analyze data and determine if an alert should send. **This is the only required function**. | `True/False` | - |
| `title` | The generated alert title | `String` | If not defined, the `Rule Display Name` or`RuleID` is used |
| `dedup` | The string to group related events with, limited to 1000 characters | `String` | If not defined, the `title`function output is used |
| `alert_context` | Additional context to pass to the alert destination\(s\) | `Dict[String: Any]` | An empty `Dict` |
| `severity` | The level of urgency of the alert | `INFO, LOW, MEDIUM, HIGH, or CRITICAL` | The severity as defined in the rule metadata |
| `description` | An explanation about why the rule exists | `String` | The description as defined in the rule metadata |
| `reference` | A reference URL to an internal document or online resource about the rule | `String` | The reference as defined in the rule metadata |
| `runbook` | A list of instructions to follow once the alert is generated | `String` | The runbook as defined in the rule metadata |
| `destinations` | The label or ID of the destinations to specifically send alerts to. An empty list will suppress all alerts. | `List[Destination Name]` | The destinations as defined in the rule metadata |

## Best Practices

### Write Tests for your Detections

Before enabling new detections, it's [recommended to write tests](testing.md) that define scenarios where alerts should or should not be generated. Best practice dictates at least one positive and one negative to ensure the most reliability.

### Use Helper Functions for Reusable Code

Once many detections are written, a set of patterns and repeated code will begin to emerge. This is a great use case for [global helper functions](globals.md), which provide a centralized location for this logic to exist across all detections. 

### Use Data Models for Generic Detections

Occasionally, analysts may want to write detections across a large set of log types. By utilizing [data models](data-models.md), generic logic can be written using event field types \(IPs, domains, users\) across all applicable log types.

### Configure Built-in Detections

By default, Panther comes installed with a number of pre-built [detection packs](detection-packs.md). Because each organization is different, a tag of `Configuration Required` is used to label the detections requiring changes prior to enabling in production. Filter detections with this tag on the main Detections page.

### Safely Accessing Event Fields

When accessing non-required event fields, it's recommended to use the Python `get` function, which works by first checking that the key exists prior to accessing its value. This avoids the common `KeyError` scenario within a rule:

```python
# Good
def rule(event):
    return event.get('field') == 'value'


# Bad
def rule(event):
    return event['field'] == 'value'
```

