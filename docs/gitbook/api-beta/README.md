# Panther API (Beta)

{% hint style="info" %}
This feature is available in version 1.28.0 and newer.
{% endhint %}

Panther offers a public GraphQL-over-HTTP API, meaning you can write GraphQL queries and invoke the API using a typical HTTP request.

### GraphQL query overview

A **GraphQL schema** defines a GraphQL API's type system. It describes the complete set of possible data (objects, fields, relationships, everything) that a client can access. Calls from the client are validated and executed against the schema. The schema also defines the available queries and mutations that the client is allowed to invoke. For each of these operations, you must specify a (sometimes optional) set of arguments and the desired return values.

With GraphQL, you select fields on objects. A **field** is a unit of data you can retrieve from an object. All GraphQL operations must specify their selections down to fields which return scalar values to ensure an unambiguously shaped response. This means that if you try to return a field that is not a scalar, your operation will fail. You must keep selecting nested subfields until all fields return scalars. Even the queries and mutations that the API exposes are also **fields** on objects.

Some fields require arguments to work. An **argument** is a set of key-value pairs attached to a specific field. Some fields require multiple arguments, while some require a single input object argument or don't require any arguments at all.

For more information on GraphQL, please see [GraphQL's documentation](https://graphql.org/learn/).
