# Globals

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

New globals can be created from the [Panther Analysis Tool](panther-analysis-tool.md#globals) or in the Panther UI.

To create a new global, navigate to `Settings` &gt; `Global Modules`:

![List Globals](../.gitbook/assets/globals-list%20%287%29%20%283%29%20%285%29.png)

Click `CREATE NEW`:

![Create New Global](../.gitbook/assets/globals-create%20%287%29%20%281%29%20%288%29.png)

Type your Python functions, then click `CREATE`. This global can now be imported in your rules or policies.

