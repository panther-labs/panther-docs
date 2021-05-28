# Alert Field Overrides

Overrides provide flexible, programmatically customizable alerts that allow users to tailor and fine-tune alerts.

## Field Overrides

In addition to the `rule`, `title`, and `dedup` functions, Rules can also contain the following functions to control the routing and alerting of detections.

Similarly, these functions take an argument of the currently processed `event`.

Any optional functions listed below left undefined will default to the rule metadata.

{% hint style="info" %}
Referencing `destinations` was made available in v1.16 and newer versions.
{% endhint %}

{% tabs %}
{% tab title="Overrides" %}
<table>
  <thead>
    <tr>
      <th style="text-align:center">Function</th>
      <th style="text-align:center">Details</th>
      <th style="text-align:center">Expected Return</th>
      <th style="text-align:center">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center"><code>severity</code>
      </td>
      <td style="text-align:center">
        <p>Generates a tailored <code>severity</code> in the alert
          <br />Used to contextualize the detection to (de)escalate severity</p>
        <p>Note that <code>severity</code> affects alert routing</p>
      </td>
      <td style="text-align:center">
        <p>String (ENUM)</p>
        <p>INFO LOW MEDIUM HIGH CRITICAL</p>
      </td>
      <td style="text-align:center">&quot;Info&quot;</td>
    </tr>
    <tr>
      <td style="text-align:center"><code>description</code>
      </td>
      <td style="text-align:center">Generates a tailored <code>description</code> in the alert</td>
      <td style="text-align:center">String</td>
      <td style="text-align:center">&quot;Description&quot;</td>
    </tr>
    <tr>
      <td style="text-align:center"><code>reference</code>
      </td>
      <td style="text-align:center">Generates a tailored <code>reference</code> in the alert</td>
      <td style="text-align:center">String</td>
      <td style="text-align:center">&quot;Reference&quot;</td>
    </tr>
    <tr>
      <td style="text-align:center"><code>runbook</code>
      </td>
      <td style="text-align:center">Generates a tailored <code>runbook</code> in the alert</td>
      <td style="text-align:center">String</td>
      <td style="text-align:center">&quot;Runbook&quot;</td>
    </tr>
    <tr>
      <td style="text-align:center"><code>destinations</code>
      </td>
      <td style="text-align:center">Used to override all other fields used for alert routing.
        <br />Empty lists are used for alert suppression.</td>
      <td style="text-align:center">
        <p>List [String]</p>
        <p>Destination Name</p>
      </td>
      <td style="text-align:center">[&quot;Main Slack&quot;]</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Finding Destination UUID" %}
{% hint style="info" %}
Versions prior to v1.16 require `destinations` to return the destinations  
To get the UUID of a detection:

* Navigate to Settings &gt; Destinations
* Click on the name of the target destination
* Extract the UUID from the current page URL 

e.g.   
`https[:]//XX.AWS_REGION.elb.amazonaws.com/settings/destinations/01234567-1edf-4edb-8f5b-0123456789a/edit/`  
Result UUID: `01234567-1edf-4edb-8f5b-0123456789a`
{% endhint %}
{% endtab %}
{% endtabs %}

For example, the functions below can be used to fine-tune a detection:

```python
HIGH_PRIORITY_REGIONS = ['us-east-1', 'eu-west-1']

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

# Alert Overrides
# Generated alerts will be Critical if it is in a high priority region, Info otherwise 
def severity(event):
    if event['awsRegion'] in HIGH_PRIORITY_REGIONS:
        return 'Critical'
    else:
        return 'Low'

# Rules with this function defined will have generated descriptions, useful for providing more details to an Alert
def description(event): 
    return 'Unauthenticated Access to S3 Bucket {} made by {}'.format(event.get('bucket'), event.get('requester'))

# Reference can be used to link to relevant external links
def reference(event): 
    return 'https://docs.runpanther.io/cloud-security/overview'

# Runbook can be used to inform how an analyst should respond to an Alert
def runbook(event):
    return 'Investigate the requester information as well as the S3 Bucket permissions.'

# Destinations is functionally a dynamic destination override, useful for controlling the reporting of generated alerts
def destinations(event):
    if event['awsRegion'] in HIGH_PRIORITY_REGIONS:
        return ['01234567-1edf-4edb-8f5b-0123456789a']
    # Suppress the alert
    else:
        return []
```

## Routing Order of Precedence

Alert routing is based on the following order of precedence \(from lowest precedence to highest\):

1. Static Severity - Default alert routing based on the severity metadata field set for the detection.
2. Generated Severity - Destinations associated with returned `severity` function defined in the Python body.
3. Static Destinations - Destinations based on the Destination Override metadata field set for the detection.
4. Generated Destinations - Destinations returned by the `destinations` function defined in the Python body.

