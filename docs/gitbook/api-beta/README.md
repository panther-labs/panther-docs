---
description: >-
  Use Panther's GraphQL API for alert, role, and user management, and data lake
  querying
---

# Panther API (Beta)

## Overview

{% hint style="info" %}
The Panther API is currently in public beta. Please share any bug reports and feature requests with your account team.
{% endhint %}

Panther offers a public GraphQL-over-HTTP API, meaning you can write GraphQL queries and invoke the API using a typical HTTP request. For more information on GraphQL, please see [GraphQL's documentation](https://graphql.org/learn/).

## How to use Panther's API

### Step 1: Creating an API Token

1. Log in to your Panther Console.
2. Navigate to **Settings > API Tokens.**
3. Click **Create an API Token**.\
   ![](../.gitbook/assets/create-api-token.png)
4. Pick a friendly name for the API Token and configure the alert and permission settings.
5. Click **Create API Token**.&#x20;

You will see a success screen that displays the value of the API Token. Please note that the API Token is sensitive information and it will not be displayed again; **make sure you copy the API Token and store it in a secure location**.

![](../.gitbook/assets/api-token-success.png)

#### Testing the API Token

{% hint style="info" %}
There may be a propagation delay of 30 to 60 seconds after adding an API Token.&#x20;
{% endhint %}

After generating an API Token, you can test to verify that it works as expected:

1. On the API Token creation success screen, click the link that says `Give it a go on our Playground.`
2. Locate the **REQUEST HEADERS** tab at the bottom-left corner of the Playground screen. Under this tab, change the default value of the `X-API-Key` header from `<ENTER_YOUR_KEY_HERE>` to the value of your API Token.
3. In the upper left corner, press the "play" icon to run the test.

![](../.gitbook/assets/api-playground-test.png)

You can discover the available queries, mutations, and fields by clicking **Documentation Explorer** on the right side panel of the Playground.&#x20;

For additional ways to discover the schema, see [Discovering the Schema.](./#step-3-discovering-the-schema)



### Step 2: Invoking the API

#### Prerequisites

To invoke the API using an HTTP curl operation, you will need the following information:

* The GraphQL endpoint to hit
  * The GraphQL API endpoint is tied to your Panther domain and the API URL format is `https://api.{YOUR_PANTHER_DOMAIN}/public/graphql`.&#x20;
  * For example, if your Panther domain is _https://acme.runpanther.net_, then your GraphQL API URL is `https://api.acme.runpanther.net/public/graphql`.
* The auth-related header
  * The auth-related header is called `X-API-Key` and its value should always be a valid API Token that you generated through the Panther UI.
* A GraphQL query
  * The GraphQL query differs from use case to use case. Please refer to our [schema discoverability page](https://docs.runpanther.io/api-beta/discovering-the-schema) or our [common operations](./#common-operations) for more on this topic.

#### Invoking the API

There are two ways to invoke a GraphQL-over-HTTP API:

* [Manually construct an HTTP call](./#invoke-the-api-option-1-manually-constructing-http-calls)
* [Install and use a GraphQL Client to abstract the transport-related complexities](./#invoke-the-api-option-2-installing-and-using-graphql-clients-recommended) _(recommended)_

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

You can find all available operations of the API, as well as detailed end-to-end examples in the subpages of the current page. For a high level list, check out our [supported operations](./#supported-operations).

#### Getting your Panther Version

```graphql
query PantherVersion {
    generalSettings {
        pantherVersion
    }
}
```

### Step 3: Discovering the Schema

There are three options to discover the GraphQL schema:

* Download the publicly available GraphQL schema file _(quickest)_
* Use [Panther's API Playground](api-playground.md) _(most user-friendly)_
* Perform an [introspection query](https://graphql.org/learn/introspection/) against the GraphQL endpoint _(best for tools and services)_

#### Option 1: Downloading the GraphQL schema file

You can download the latest version of the GraphQL schema file [here](https://panther-community-us-east-1.s3.amazonaws.com/v1.39.0/graphql/schema.public.graphql).

#### Option 2: Using the GraphQL Playground

Panther's API Playground is a user-friendly way of browsing and discovering what's supported in our API. Please refer to our [API Playground docs](https://docs.runpanther.io/api-beta/api-playground) for information on how to use this as a discoverability mechanism.

#### Option 3: Performing an introspection query

An introspection query yields all the GraphQL API's entities in a format that most third-party libraries can parse. This discoverability option is useful if you want to make another library or service aware of the supported operations and types that the Panther API has. These libraries typically issue their own version of an introspection query, so they only need to be pointed to an API URL.

For security purposes, the introspection query is an authorized operation. This means that you'll need to add an `X-API-Key` header to your HTTP call with the value of an [API Token](https://docs.runpanther.io/api-beta/generating-an-api-token) in order for the introspection to work. &#x20;

```
$ curl -H "X-API-Key: <API_KEY>" -d <INTROSPECTION_QUERY> <YOUR_PANTHER_API_URL>
```

The actual shape of the introspection query is customizable. You can ask for a limited set of entities or for all possible information about the schema. For example, a query such as the following would yield every single piece of schema information:

```graphql
query IntrospectionQuery {
    __schema {
      queryType { name }
      mutationType { name }
      types {
        ...FullType
      }
      directives {
        name
        description
        locations
        args {
          ...InputValue
        }
      }
    }
  }
  fragment FullType on __Type {
    kind
    name
    description
    fields(includeDeprecated: true) {
      name
      description
      args {
        ...InputValue
      }
      type {
        ...TypeRef
      }
      isDeprecated
      deprecationReason
    }
    inputFields {
      ...InputValue
    }
    interfaces {
      ...TypeRef
    }
    enumValues(includeDeprecated: true) {
      name
      description
      isDeprecated
      deprecationReason
    }
    possibleTypes {
      ...TypeRef
    }
  }
  fragment InputValue on __InputValue {
    name
    description
    type { ...TypeRef }
    defaultValue
  }
  fragment TypeRef on __Type {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
                ofType {
                  kind
                  name
                }
              }
            }
          }
        }
      }
    }
  }
```

## Supported Operations

The Panther API supports an ever-growing set of capabilities that allow you to build your security workflows, as well as an [API Playground](api-playground.md) to test operations.

See the Supported Operations page for a list of operations and examples.
