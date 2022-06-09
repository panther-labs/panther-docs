# Data Lake Queries

The Panther API supports the following data lake operations:

* Listing your data lake databases, tables, and columns
* Executing a data lake (Data Explorer) query using SQL
* Executing an Indicator Search query&#x20;
* Canceling any currently-running query
* Fetching the details of any previously executed query
* Listing all currently-running or previously-executed queries with optional filters

See the sections below for GraphQL queries and mutations around certain common operations, as well as end-to-end examples on typical workflows.

## Common Operations

Below are some of the most common GraphQL operations in Panther. These examples demonstrate how you can use a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

#### Database Entities

{% tabs %}
{% tab title="Listing all database entities" %}
```graphql
# `AllDatabaseEntities` is a nickname for the operation
query AllDatabaseEntities {
  dataLakeDatabases {
     name
     description
     tables {
       name
       description
       columns {
         name
         description
         type
       }
     }
  }
}
```
{% endtab %}

{% tab title="Fetching the entities of a particular database" %}
```graphql
# `DatabaseEntities` is a nickname for the operation
query DatabaseEntities {
  dataLakeDatabase(name: "panther_logs.public") {
     name
     description
     tables {
       name
       description
       columns {
         name
         description
         type
       }
     }
  }
}
```
{% endtab %}
{% endtabs %}

#### Executing queries

{% tabs %}
{% tab title="Executing a data lake (Data Explorer) query" %}
```graphql
# `IssueDataLakeQuery` is a nickname for the operation
mutation IssueDataLakeQuery {
  executeDataLakeQuery(input: {
    sql: "select * from panther_logs.public.aws_alb limit 50"
  }) {
     id # the unique ID of the query
  }
}
```
{% endtab %}

{% tab title="Executing an Indicator Search query" %}
```graphql
# `IssueIndicatorSearchQuery` is a nickname for the operation
mutation IssueIndicatorSearchQuery {
  executeIndicatorSearchQuery(input: {
    indicators: ["286103014039", "126103014049"]
    startTime: "2022-04-01T00:00:00.000Z",
    endTime: "2022-04-30T23:59:59.000Z"
    indicatorName: p_any_aws_account_ids # or leave blank for auto-detect
  }) {
     id # the unique ID of the query
  }
}
```
{% endtab %}

{% tab title="Canceling a query" %}
```graphql
# `AbandonQuery` is a nickname for the operation
mutation AbandonQuery {
  cancelDataLakeQuery(input: { id: "1234-5678" }) {
     id # return the ID that got canceled
  }
}
```
{% endtab %}
{% endtabs %}

#### Fetching results for a data lake or Indicator Search query

When you execute a data lake or Indicator Search query, it can take a few seconds to a few minutes for results to come back. To confirm that the query has completed, you must check the status of the query (polling).

You can use the following query to check the query status, while also fetching its results if available:

{% tabs %}
{% tab title="Fetching the first page of results" %}
```graphql
# `QueryResults` is a nickname for the operation
query QueryResults {
  dataLakeQuery(id: "1234-1234-1234-1234") { # the unique ID of the query
    message
    status
    results {
      edges {
        node
      }
    }
  }
}
```
{% endtab %}

{% tab title="Fetching subsequent pages of results" %}
```graphql
# `QueryResults` is a nickname for the operation
query QueryResults {
  dataLakeQuery(id: "1234-1234-1234-1234") { # the unique ID of the query
    message
    status
    results(input: { cursor: "5678-5678-5678-5678" }) { # the value of `endCursor`
      edges {
        node
      }
      pageInfo
        endCursor
        hasNextPage
      }
    }
  }
```
{% endtab %}
{% endtabs %}

The expected values of `status` and `results` depend on the query's status:

* If the query is still running:
  * `status` will have a value of `running`&#x20;
  * `results` will have a value of `null`
* If the query has failed:
  * `status` will have a value of `failed` &#x20;
  * `results` will have a value of `null` and the error message will be available in the `message` key
* If the query has completed
  * `status` will have a value of `succeeded`&#x20;
  * `results` will be populated

All of the above (along with the possible values for `status)` , along with additional fields you are allowed to request can be found in our [Documentation Explorer](../api-playground.md#documentation-explorer) or [GraphQL schema file](broken-reference)).

#### Fetching metadata around a data lake or Indicator Search query

In the example above, we requested the `results` of a Panther query. It is also possible to request additional metadata around the query.&#x20;

In the following example, we request these metadata along the first page of results:

```graphql
# `QueryMetadata` is a nickname for the operation
query QueryMetadata {
  dataLakeQuery(id: "1234-1234-1234-1234") { # the unique ID of the query
    name
    isScheduled
    issuedBy {
      ... on User {
        email
      }
      ... on APIToken {
        name
      } 
    }
    sql
    message
    status
    startedAt
    completedAt
    results {
      edges {
        node
      }
    }
  }
```

#### Listing data lake and Indicator Search queries

{% tabs %}
{% tab title="Fetching the first page" %}
```graphql
# `ListDataLakeQueries` is a nickname for the operation
query ListDataLakeQueries {
  dataLakeQueries {
    name
    isScheduled
    issuedBy {
      ... on User {
        email
      }
      ... on APIToken {
        name
    } 
    }
    sql
    message
    status
    startedAt
    completedAt
    results { # we're only fetching the first page of results for each query
      edges {
        node
      }
    }
  }
```
{% endtab %}

{% tab title="Fetching subsequent pages" %}
```graphql
# `ListDataLakeQueries` is a nickname for the operation
query ListDataLakeQueries {
  dataLakeQueries(input: { cursor: "5678-5678-5678-5678" }) { # the value of `endCursor`
    name
    isScheduled
    issuedBy {
      ... on User {
        email
      }
      ... on APIToken {
        name
    } 
    }
    sql
    message
    status
    startedAt
    completedAt
    results { # we're only fetching the first page of results for each query
      edges {
        node
      }
    }
    pageInfo {
      endCursor
      hasNextPage
    }
  }
```
{% endtab %}

{% tab title="Fetching a filtered set" %}
```graphql
# `ListDataLakeQueries` is a nickname for the operation
query ListDataLakeQueries {
  dataLakeQueries(input: { contains: "aws_alb", isScheduled: true }) {
    name
    isScheduled
    issuedBy {
      ... on User {
        email
      }
      ... on APIToken {
        name
    } 
    }
    sql
    message
    status
    startedAt
    completedAt
    results { # we're only fetching the first page of results for each query
      edges {
        node
      }
    }
  }
```
{% endtab %}
{% endtabs %}

## End-to-end Examples

Below, we will build on the [Common Operations](data-lake-queries.md#common-operations) examples to showcase an end-to-end flow.

#### Execute a data lake (Data Explorer) Query

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// npm install graphql graphql-request

import { GraphQLClient, gql } from 'graphql-request';

const client = new GraphQLClient(
  'YOUR_PANTHER_API_URL', 
  { headers: { 'X-API-Key': 'YOUR_API_KEY' } 
});

// `IssueQuery` is a nickname for the query. You can fully omit it.
const issueQuery = gql`
  mutation IssueQuery($sql: String!) {
    executeDataLakeQuery(input: { sql: $sql }) {
      id
    }
  }
`;

// `GetQueryResults` is a nickname for the query. You can fully omit it.
const getQueryResults = gql`
  query GetQueryResults($id: ID!, $cursor: String) {
    dataLakeQuery(id: $id) {
      message
      status
      results(input: { cursor: $cursor }) {
        edges {
          node
        }
        pageInfo {
          endCursor
          hasNextPage
        }
      }
    }
  }
`;

(async () => {
  try {
    // an accumulator that holds all result nodes that we fetch
    let allResults = [];
    // a helper to know when to exit the loop
    let hasMore = true;
    // the pagination cursor
    let cursor = null;

    // issue a query
    const mutationData = await client.request(issueQuery, {
      sql: 'select * from panther_logs.public.aws_alb limit 5',
    });

    // Start polling the query until it returns results. From there,
    // keep fetching pages until there are no more left
    do {
      const queryData = await client.request(getQueryResults, {
        id: mutationData.executeDataLakeQuery.id,
        cursor,
      });

      // if it's still running, print a message and keep polling
      if (queryData.dataLakeQuery.status === 'running') {
        console.log(queryData.dataLakeQuery.message);
        continue;
      }

      // if it's not running & it's not completed, then it's
      // either cancelled or it has errored out. In this case,
      // throw an exception
      if (queryData.dataLakeQuery.status !== 'completed') {
        throw new Error(queryData.dataLakeQuery.message);
      }

      allResults = [...allResults, ...queryData.dataLakeQuery.results.edges.map(edge => edge.node)];

      hasMore = queryData.dataLakeQuery.results.pageInfo.hasNextPage;
      cursor = queryData.dataLakeQuery.results.pageInfo.endCursor;
    } while (hasMore);

    console.log(`Your query returned ${allResults.length} result(s)!`);
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

# `IssueQuery` is a nickname for the query. You can fully omit it.
issue_query = gql(
    """
    mutation IssueQuery($sql: String!) {
        executeDataLakeQuery(input: { sql: $sql }) {
            id
        }
    }
    """
)

# `GetQueryResults` is a nickname for the query. You can fully omit it.
get_query_results = gql(
    """
    query GetQueryResults($id: ID!, $cursor: String) {
        dataLakeQuery(id: $id) {
            message
            status
            results(input: { cursor: $cursor }) {
                edges {
                    node
                }
                pageInfo {
                    endCursor
                    hasNextPage
                }
            }
        }
    }
    """
)

# an accumulator that holds all results that we fetch from all pages
all_results = []
# a helper to know when to exit the loop.
has_more = True
# the pagination cursor
cursor = None

# Issue a Data Lake (Data Explorer) query
mutation_data = client.execute(
    issue_query,
    variable_values={
        "sql": "select * from panther_logs.public.aws_alb limit 5"
    }
)

# Start polling the query until it returns results. From there,
# keep fetching pages until there are no more left
while has_more:
    query_data = client.execute(
        get_query_results,
        variable_values = {
            "id": mutation_data["executeDataLakeQuery"]["id"],
            "cursor": cursor
        }
    )
    
    # if it's still running, print a message and keep polling
    if query_data["dataLakeQuery"]["status"] == "running":
        print(query_data["dataLakeQuery"]["message"])
        continue
    
    # if it's not running & it's not completed, then it's
    # either cancelled or it has errored out. In this case,
    # throw an exception
    if query_data["dataLakeQuery"]["status"] != "completed":
        raise Exception(query_data["dataLakeQuery"]["message"])

    all_results.extend([edge["node"] for edge in query_data["dataLakeQuery"]["results"]["edges"]])
    has_more = query_data["dataLakeQuery"]["results"]["pageInfo"]["hasNextPage"]
    cursor = query_data["dataLakeQuery"]["results"]["pageInfo"]["endCursor"]

print(f'Query returned {len(all_results)} results(s)!')
```
{% endtab %}
{% endtabs %}

#### Execute an Indicator Search query

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// npm install graphql graphql-request

import { GraphQLClient, gql } from 'graphql-request';

const client = new GraphQLClient(
  'YOUR_PANTHER_API_URL', 
  { headers: { 'X-API-Key': 'YOUR_API_KEY' } 
});

// `IssueQuery` is a nickname for the query. You can fully omit it.
const issueQuery = gql`
  mutation IssueQuery($input: ExecuteIndicatorSearchQueryInput!) {
    executeIndicatorSearchQuery(input: $input) {
      id
    }
  }
`;

// `GetQueryResults` is a nickname for the query. You can fully omit it.
const getQueryResults = gql`
  query GetQueryResults($id: ID!, $cursor: String) {
    dataLakeQuery(id: $id) {
      message
      status
      results(input: { cursor: $cursor }) {
        edges {
          node
        }
        pageInfo {
          endCursor
          hasNextPage
        }
      }
    }
  }
`;

(async () => {
  try {
    // an accumulator that holds all result nodes that we fetch
    let allResults = [];
    // a helper to know when to exit the loop
    let hasMore = true;
    // the pagination cursor
    let cursor = null;

    // issue a query
    const mutationData = await client.request(issueQuery, {
      input: {         
        indicators: ["226103014039"],
        startTime: "2022-03-29T00:00:00.001Z",
        endTime: "2022-03-30T00:00:00.001Z",
        indicatorName: "p_any_aws_account_ids"
      }
    });

    // Keep fetching pages until there are no more left
    do {
      const queryData = await client.request(getQueryResults, {
        id: mutationData.executeIndicatorSearchQuery.id,
        cursor,
      });

      // if it's still running, print a message and keep polling
      if (queryData.dataLakeQuery.status === 'running') {
        console.log(queryData.dataLakeQuery.message);
        continue;
      }

      // if it's not running & it's not completed, then it's
      // either cancelled or it has errored out. In this case,
      // throw an exception
      if (queryData.dataLakeQuery.status !== 'completed') {
        throw new Error(queryData.dataLakeQuery.message);
      }

      allResults = [...allResults, ...queryData.dataLakeQuery.results.edges.map(edge => edge.node)];

      hasMore = queryData.dataLakeQuery.results.pageInfo.hasNextPage;
      cursor = queryData.dataLakeQuery.results.pageInfo.endCursor;
    } while (hasMore);

    console.log(`Your query returned ${allResults.length} result(s)!`);
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

# `IssueQuery` is a nickname for the query. You can fully omit it.
issue_query = gql(
    """
    mutation IssueQuery($input: ExecuteIndicatorSearchQueryInput!) {
        executeIndicatorSearchQuery(input: $input) {
            id
        }
    }
    """
)

# `GetQueryResults` is a nickname for the query. You can fully omit it.
get_query_results = gql(
    """
    query GetQueryResults($id: ID!, $cursor: String) {
        dataLakeQuery(id: $id) {
            message
            status
            results(input: { cursor: $cursor }) {
                edges {
                    node
                }
                pageInfo {
                    endCursor
                    hasNextPage
                }
            }
        }
    }
    """
)

# an accumulator that holds all results that we fetch from all pages
all_results = []
# a helper to know when to exit the loop
has_more = True
# the pagination cursor
cursor = None

# Issue an Indicator Search query
mutation_data = client.execute(
    issue_query,
    variable_values={
        "input": {         
            "indicators": ["226103014039"],
            "startTime": "2022-03-29T00:00:00.001Z",
            "endTime": "2022-03-30T00:00:00.001Z",
            "indicatorName": "p_any_aws_account_ids"
        }
    }
)

# Start polling the query until it returns results. From there,
# keep fetching pages until there are no more left
while has_more:    
    query_data = client.execute(
        get_query_results,
        variable_values = {
            "id": mutation_data["executeIndicatorSearchQuery"]["id"],
            "cursor": cursor
        }
    )
    
    # if it's still running, print a message and keep polling
    if query_data["dataLakeQuery"]["status"] == "running":
        print(query_data["dataLakeQuery"]["message"])
        continue
    
    # if it's not running & it's not completed, then it's
    # either cancelled or it has errored out. In this case,
    # throw an exception
    if query_data["dataLakeQuery"]["status"] != "completed":
        raise Exception(query_data["dataLakeQuery"]["message"])

    all_results.extend([edge["node"] for edge in query_data["dataLakeQuery"]["results"]["edges"]])
    has_more = query_data["dataLakeQuery"]["results"]["pageInfo"]["hasNextPage"]
    cursor = query_data["dataLakeQuery"]["results"]["pageInfo"]["endCursor"]

print(f'Query returned {len(all_results)} results(s)!')
```
{% endtab %}
{% endtabs %}
