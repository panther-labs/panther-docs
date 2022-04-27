# Using the Panther API

{% hint style="info" %}
This page assumes that you've already [generated an API Token](https://docs.runpanther.io/api-beta/generating-an-api-token) to interact with the API and that you've already experimented with it in the [Playground](https://docs.runpanther.io/api-beta/api-playground).
{% endhint %}

This page contains instructions on how to invoke the API, and common operations and end-to-end examples.&#x20;

For information specific to alerting or data lake operations, please see the documentation pages: [Alerts & Errors](alerts-and-errors.md) | [Data Lake Queries](data-lake-queries.md)

## Prerequisites

To invoke the API using an HTTP curl operation, you will need the following information:

* The GraphQL endpoint to hit
  * The GraphQL API endpoint is tied to your Panther domain and the API URL format is `https://api.{YOUR_PANTHER_DOMAIN}/public/graphql`.&#x20;
  * For example, if your Panther domain is _https://acme.runpanther.net_, then your GraphQL API URL is `https://api.acme.runpanther.net/public/graphql`.
* The auth-related header
  * The auth-related header is called `X-API-Key` and its value should always be a valid API Token that you generated through the Panther UI.
* A GraphQL query
  * The GraphQL query differs from use case to use case. Please refer to our [schema discoverability page](https://docs.runpanther.io/api-beta/discovering-the-schema) or our [common operations](./#common-operations) for more on this topic.



## Invoking the API

There are two ways to invoke a GraphQL-over-HTTP API:

* Manually construct an HTTP call
* Install and use a GraphQL Client to abstract the transport-related complexities _(recommended)_

#### Option 1: Manually Constructing HTTP Calls

An example request:

```
curl 'YOUR_PANTHER_GRAPHQL_API_URL' \
-H 'Content-Type: application/json' \
-H 'X-API-Key: {YOUR_API_KEY}' \
-d '{"query":"\n query Foo {\n alerts {\n edges {\n node {\n id\n }\n }\n }\n }","variables":{}}' 
```

The query above returns the first page of all of your Panther alerts. If it's the first time you're using GraphQL, please note the following:

* There's only one endpoint.
* The HTTP operation is always a `POST.`
* The API operations are defined in `POST`'s body.
* The body of the `POST` operation **always** contains the following keys:
  * `query` - a GraphQL string defining the GraphQL operation that should be executed
  * `variables` - an optional set of variables that will be passed along to the query
  * `operationName` - an optional "nickname" for this operation
* You must always select a set of fields to return (if the operation returns data.)

Note: The only thing that would change from one GraphQL operation to another is the body of the HTTP `POST`.

#### Option 2: Installing and Using GraphQL Clients _(Recommended)_

While all GraphQL operations are essentially simple HTTP calls, the advantage of using a GraphQL client is that it is more user-friendly.

We recommend using:

* ``[`graphql-request`](https://github.com/prisma-labs/graphql-request) for your NodeJS projects
* ``[`gql`](https://github.com/graphql-python/gql) for your Python projects
* ``[`go-graphql-client`](https://github.com/hasura/go-graphql-client) for your Go projects

Below you'll find some examples of how you would construct a GraphQL query to fetch the first page of alerts in your system:

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// npm install graphql graphql-request

import { GraphQLClient, gql } from 'graphql-request';

const client = new GraphQLClient(
  'YOUR_PANTHER_API_URL', 
  { headers: { 'X-API-Key': 'YOUR_API_KEY' } 
});

// `PaginateAlerts` is a nickname for the operation
const query = gql` 
  query PaginateAlerts {
    alerts {
      edges {
        node {
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
`;

client.request(query).then((data) => console.log(data));
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

# `PaginateAlerts` is a nickname for the operation
query = gql(
  """
  query PaginateAlerts {
    alerts {
      edges {
        node {
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
  """
)

result = client.execute(query)
print(result)
```
{% endtab %}

{% tab title="Golang" %}
```go
package main

import (
    "context"
    "fmt"
    "github.com/hasura/graphql"
)

// Strongly-typed languages don't pair well with GraphQL
var query struct {
    Alerts struct {
        Edges []struct {
            Node struct {
                Id graphql.String
                Title graphql.String
                Severity graphql.String
                Status  graphql.String
            }
        }
        PageInfo struct {
            HasNextPage graphql.Boolean
            Cursor graphql.String
        }
    }
}

client := graphql.
    NewClient("YOUR_PANTHER_API_URL", nil).
    WithRequestModifier(func(req *http.Request) {
        req.Header.Set("X-API-KEY", "YOUR_API_KEY")
    })

if err := client.Query(context.Background(), &query, nil); err != nil {
    // Handle error
}

fmt.Println(query.Alerts.PageInfo.HasNextPage)

```
{% endtab %}
{% endtabs %}

You can find all available operations of the API, as well as detailed end-to-end examples in the subpages of the current page. For a high level list, check out our [supported operations](../#supported-operations).



#### Getting your Panther Version

```graphql
query PantherVersion {
    generalSettings {
        pantherVersion
    }
}
```
