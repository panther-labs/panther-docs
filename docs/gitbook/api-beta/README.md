# Panther API (BETA)

## Overview

{% hint style="info" %}
The Panther API is currently in public beta. Please share any bug reports and feature requests with your account team.
{% endhint %}

Panther offers a public GraphQL-over-HTTP API, meaning you can write GraphQL queries and invoke the API using a typical HTTP request. For more information on GraphQL, please see [GraphQL's documentation](https://graphql.org/learn/).

## How to use Panther's API

To get started with Panther's API:

1. Create an API Token.
   * See [Generating an API Token](generating-an-api-token.md) for instructions.
2. Obtain your unique HTTP URL.
   * See [Using the Panther API](using-the-panther-api/#prerequisites) for more information.
3. Invoke the API.
   * See [Invoking the API](using-the-panther-api/#invoking-the-api) for more information.
4. Start writing GraphQL queries.&#x20;
   * View the [supported operations](./#supported-operations) to see what's available.

## Supported Operations

The Panther API supports an ever-growing set of capabilities that allow you to build your security workflows. Currently, the following capabilities are available through the API:

### Alert Management

Our alerting API allows you to:

* List your alerts and errors with optional filters
* Fetch the details of a particular alert
* Update the status of one or more alerts

For more information visit the [Alerts & Errors](using-the-panther-api/alerts-and-errors.md) page.

### Data Lake Querying

Our Data Lake API allows you to:

* List your data lake databases, tables, and columns
* Execute a data lake (Data Explorer) query using SQL
* Execute an Indicator Search query&#x20;
* Cancel any currently-running query
* Fetch the details of any previously executed query
* List all currently-running or previously-executed queries with optional filters

For more information visit the [Data Lake Queries](using-the-panther-api/data-lake-queries.md) page.
