---
description: Panther API user data for measuring ingestion and alert metrics
---

# Metrics

{% hint style="info" %}
Metric operations are available in Panther version 1.39 and newer.
{% endhint %}

The Panther API provides the following user metric operations:

* Total number of bytes & events that Panther ingested and/or processed over a specific time period
* Breakdown of alerts that were generated for each Severity type over a specific time period

See the sections below \
\
See the sections below for GraphQL queries, mutations, and end-to-end workflow examples around core metrics operations.

## Common metrics operations

Below are some of the most common GraphQL metrics operations in Panther. These examples demonstrate the documents you have to send using a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

**Metrics Query**

```graphql
# `GetMetrics` is a nickname for the operation. You can omit any of the 
# fields/info you're not interested in & only query for what you're after
query GetMetrics {
  metrics(input: { 
    fromDate: "2021-01-01T00:00:00Z",
    toDate: "2021-12-31T23:59:59Z"
  }) {
    alertsPerSeverity {
      label
      value
      breakdown
    }
    alertsPerRule {
      label
      value
      entityId
    }
    eventsProcessedPerLogType {
      label
      value
      breakdown
    }
    bytesProcessedPerSource {
      label
      value
      breakdown
    }
    latencyPerLogType {
      label
      value
    }
    bytesIngestedPerSource {
      label
      value
    }
    bytesQueriedPerSource {
      label
      value
      breakdown
    }
    totalAlerts
    totalBytesIngested
    totalBytesProcessed
    totalBytesQueried
    totalEventsProcessed
  }
}
```

{% hint style="info" %}
The `breakdown` field is only useful for charts that use time as their X-axis. It produces a map of timestamps -> values as a "breakdown" of the `value` field to its constituents.
{% endhint %}

### End-to-end examples

Below, we build on the operations from the [Core Operations](metrics.md#core-operations) examples to showcase an end-to-end use case flow.

#### **Fetch Panther's log metrics**

{% tabs %}
{% tab title="Python" %}
```python
# pip install gql aiohttp

from gql import gql, Client
from gql.transport.aiohttp import AIOHTTPTransport

transport = AIOHTTPTransport(
  url="YOUR_PANTHER_API_URL",
  headers={"X-API-Key": "YOUR_API_KEY"},
)

client = Client(transport=transport, fetch_schema_from_transport=True)

get_metrics = gql(
  """
  query GetMetrics($input: MetricsInput!)  {
    metrics(input: $input) {
      totalAlerts
      totalEventsProcessed
    }
  }
  """
)

data = client.execute(
  get_metrics,
  variable_values= {
    "input": {
      "fromDate": "2022-07-01T00:00:00Z",
      "toDate": "2022-07-31T23:59:59Z",
    }
  }
)

print(f'In July, Panther processed {data["metrics"]["totalEventsProcessed"]} events and generated {data["metrics"]["totalAlerts"]} alerts')
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { GraphQLClient, gql } from "graphql-request";

const client = new GraphQLClient(
  "YOUR_PANTHER_API_URL",
  { headers: { "X-API-Key": "YOUR_API_KEY" } }
);



const getMetrics = gql`
  query GetMetrics($input: MetricsInput!)  {
    metrics(input: $input) {
      totalAlerts
      totalEventsProcessed
    }
  }
`;

(async () => {
  try {
    const data = await client.request(getMetrics, {
      input: {
        fromDate: "2022-07-01T00:00:00Z",
        toDate: "2022-07-31T23:59:59Z"
      }
    });

    console.log(
      `In July, Panther processed ${data.metrics.totalEventsProcessed} events and generated ${data.metrics.totalAlerts} alerts.`
    );
  } catch (err) {
    console.error(err);
  }
})();

```
{% endtab %}
{% endtabs %}

