# Alerts & Errors

The Panther API supports the following alerting operations:

* Listing your alerts and errors with optional filters
* Fetching the details of a particular alert
* Updating the status of one or more alerts

See the sections below for GraphQL queries and mutations around certain common operations, as well as end-to-end examples on typical workflows.

## Common Operations

Below are some of the most common GraphQL operations in Panther. These examples demonstrate how you can use a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

For more information on what fields and operations exist in the API, please see the [Discovering the Schema documentation](https://docs.runpanther.io/api-beta/discovering-the-schema).

#### Listing your Alerts

{% tabs %}
{% tab title="First page of all alerts" %}
```graphql
# `FirstPageOfAllAlerts` is a nickname for the operation
query FirstPageOfAllAlerts {
    alerts {
      edges {
        node { # you can ask for more alert-related fields here
          id
          title
          severity
          status
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
```
{% endtab %}

{% tab title="Subsequent page of alerts" %}
```graphql
# `AnotherPageOfAllAlerts` is a nickname for the operation
query AnotherPageOfAllAlerts {
    alerts(input: { cursor: "value_of_`pageInfo.endCursor`" }) {
      edges {
        node { # you can ask for more alert-related fields here
          id
          title
          severity
          status
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
```
{% endtab %}

{% tab title="Filtered page of alerts" %}
```graphql
# `FilteredPageOfAlerts` is a nickname for the operation
query FilteredPageOfAlerts {
    alerts(input: { severities: [LOW], statuses: [OPEN] }) { 
      edges {
        node { # you can ask for more alert-related fields here
          id
          title
          severity
          status
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
```
{% endtab %}
{% endtabs %}

#### Describing an Alert

```graphql
query AlertDetails {
    alert(id: "FAKE_ALERT_ID") {
       id
       title
       severity
       status
    }
  }
```

#### Updating the status of one or more Alerts

```graphql
mutation UpdateAlertStatus {
    updateAlertStatusById(
       input: {
          ids: ["FAKE_ALERT_ID_1","FAKE_ALERT_ID_2"], 
          status: CLOSED # notice how this isn't a string when hardcoded (it's an `enum`)
       }
    ) {
       alerts {
         id
         status
       }
    }
  }
```

## End-to-end Examples

Below, we will build on the [Common Operations](alerts-and-errors.md#common-operations) examples to showcase an end-to-end flow.

#### Find a particular set of alerts and mark them as _Resolved:_

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// npm install graphql graphql-request

import { GraphQLClient, gql } from 'graphql-request';

const client = new GraphQLClient(
  'YOUR_PANTHER_API_URL', 
  { headers: { 'X-API-Key': 'YOUR_API_KEY' } 
});

// `FindAlerts` is a nickname for the query. You can fully omit it.
const findAlerts = gql`
  query FindAlerts($input: AlertsInput!) {
    alerts(input: $input) {
      edges {
        node {
          id
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
`;

// `UpdateAlerts` is a nickname for the query. You can fully omit it.
const updateAlerts = gql`
  mutation UpdateAlerts($input: UpdateAlertStatusByIdInput!) {
    updateAlertStatusById(input: $input) {
      alerts {
        id
      }
    }
  }
`;

(async () => {
  try {
    
    // an accumulator that holds all alerts that we fetch all pages
    let allAlerts = [];
    // a helper to know when to exit the loop
    let hasMore = true;
    // the pagination cursor
    let cursor = null;

    // Keep fetching pages until there are no more left
    do {
      const queryData = await client.request(findAlerts, {
        input: {
          severities: ["LOW"],
          createdAtBefore: "2022-03-01T00:00:00.000Z",
          createdAtAfter: "2022-02-01T00:00:00.000Z",
          cursor
        }
      });

      allAlerts = [
        ...allAlerts,
        ...queryData.alerts.edges.map((edge) => edge.node)
      ];
      
      hasMore = queryData.alerts.pageInfo.hasNextPage;
      cursor = queryData.alerts.pageInfo.endCursor;
    } while (hasMore);

  
    const mutationData = await client.request(updateAlerts, {
      input: {
        ids: allAlerts.map((alert) => alert.id),
        status: "RESOLVED"
      }
    });

    console.log(
      `Resolved ${mutationData.updateAlertStatusById.alerts.length} alert(s)!`
    );
  } catch (err) {
    console.error(err.response);
  }
})();

```
{% endtab %}

{% tab title="Python" %}
```python
# pip install gql aiohttp

from gql import gql, Client
from gql.transport.aiohttp import AIOHTTPTransport

transport = AIOHTTPTransport(
  url="YOUR_PANTHER_API_URL",
  headers={"X-API-Key": "YOUR_API_KEY"}
)

client = Client(transport=transport, fetch_schema_from_transport=True)

# `FindAlerts` is a nickname for the query. You can fully omit it.
find_alerts = gql(
    """
    query FindAlerts($input: AlertsInput!) {
      alerts(input: $input) {
        edges {
          node {
            id
          }
        }
        pageInfo {
          hasNextPage
          endCursor
        }
      }
    }
    """
)

# `UpdateAlerts` is a nickname for the query. You can fully omit it.
update_alerts = gql(
    """
    mutation UpdateAlerts($input: UpdateAlertStatusByIdInput!) {
      updateAlertStatusById(input: $input) {
        alerts {
          id
        }
      }
    }
    """
)

# an accumulator that holds all alerts that we fetch all pages
all_alerts = []
# a helper to know when to exit the loop
has_more = True
# the pagination cursor
cursor = None

# Keep fetching pages until there are no more left
while has_more:
    query_data = client.execute(
        find_alerts,
        variable_values={
            "input": {
                "severities": ["LOW"],
                "createdAtBefore": "2022-03-01T00:00:00.000Z",
                "createdAtAfter": "2022-02-01T00:00:00.000Z",
                "cursor": cursor
            }
        }
    )

    all_alerts.extend([edge["node"] for edge in query_data["alerts"]["edges"]])
    has_more = query_data["alerts"]["pageInfo"]["hasNextPage"]
    cursor = query_data["alerts"]["pageInfo"]["endCursor"]

mutation_data = client.execute(
    update_alerts,
    variable_values={
        "input": {
            "ids": [alert["id"] for alert in all_alerts],
            "status": "RESOLVED"
        }
    }
)

print(f'Resolved {len(mutation_data["updateAlertStatusById"]["alerts"])} alert(s)!')
```
{% endtab %}
{% endtabs %}
