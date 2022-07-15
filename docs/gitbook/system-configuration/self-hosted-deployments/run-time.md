# Runtime Environment

{% hint style="info" %}
**Panther SaaS customers** must file a support ticket to have a custom set of libraries or layer applied to their hosted instance of Panther. The following Python libraries are available to be used in Panther in addition to `boto3` provided by [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html):
{% endhint %}

| Package          | Version          | Description                 | License   |
| ---------------- | ---------------- | --------------------------- | --------- |
| `jsonpath-ng`    | `1.5.2`          | JSONPath Implementation     | Apache v2 |
| `policyuniverse` | `1.3.3.20210223` | Parse AWS ARNs and Policies | Apache v2 |
| `requests`       | `2.23.0`         | Easy HTTP Requests          | Apache v2 |

For **self-hosted** instances of Panther, you can add custom libraries by editing the `PipLayer` below in the `panther_config.yml`:

```yaml
PipLayer:
  - jsonpath-ng=1.5.2
  - policyuniverse==1.3.3.20210223
  - requests==2.23.0
```

Alternatively, you can override the runtime libraries by attaching a custom Lambda layer in the `panther_config.yml`:

```yaml
BackendParameterValues:
  PythonLayerVersionArn: 'arn:aws:lambda:us-east-2:123456789012:layer:my-layer:3'
```
