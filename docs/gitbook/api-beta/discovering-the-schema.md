# Discovering the Schema



There are three options to discover the GraphQL schema:

* Download the publicly available GraphQL schema file _(quickest)_
* Use Panther's GraphQL Playground _(most user-friendly)_
* Perform an [introspection query](https://graphql.org/learn/introspection/) against the GraphQL endpoint _(best for tools and services)_

__

### Downloading the GraphQL schema file

You can download the latest version of the GraphQL schema file [here](https://panther-community-us-east-1.s3.amazonaws.com/v1.28.0-RC/graphql/schema.public.graphql).

### Using the GraphQL Playground

Panther's API Playground is a user-friendly way of browsing and discovering what's supported in our API. Please refer to our [API Playground docs](https://docs.runpanther.io/api/api-playground) for information on how to use this as a discoverability mechanism.

### Performing an introspection query

An introspection query yields all the GraphQL API's entities in a format that most third-party libraries can parse. This discoverability option is useful if you want to make another library or service aware of the supported operations and types that the Panther API has. These libraries typically issue their own version of an introspection query, so they only need to be pointed to an API URL.

For security purposes, the introspection query is an authorized operation. This means that you'll need to add an `X-API-Key` header to your HTTP call with the value of an [API Token](https://docs.runpanther.io/api/generating-an-api-token) in order for the introspection to work. &#x20;

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
