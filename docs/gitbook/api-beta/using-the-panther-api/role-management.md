---
description: User and Role APIs are available in version 1.36 and newer
---

# Role Management

The Panther API supports the following role operations:

* List roles
* Get a role by ID or by name
* Create a new role
* Update a role's information and permissions
* Delete roles

See the sections below for GraphQL queries and mutations around certain common operations.

## Common role operations

Below are some of the most common GraphQL operations in Panther. These examples demonstrate how you can use a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

For more information on what fields and operations exist in the API, please see the [Discovering the Schema documentation](https://docs.runpanther.io/api-beta/discovering-the-schema).

### List Roles

```graphql
query roles {
  roles( 
    # This input is optional. Without input, the query will return all roles.
    input: {
      nameContains: "admin",
      sortDir: ascending
    }) 
  {
    createdAt
    id
    logTypeAccess
    logTypeAccessKind
    name
    permissions
    updatedAt
    updatedBy {
      ... on User {
        email
        id
      }
      ... on APIToken {
        id
        name
      }
    }
  }
}
```

### Retrieving a Role

{% tabs %}
{% tab title="Role by Name" %}
```graphql
query adminRole {
  roleByName(
   name: "admin"
   ) {
    createdAt
    id
    logTypeAccess
    logTypeAccessKind
    name
    permissions
    updatedAt
    updatedBy {
      __typename
      ... on User {
        email
        id
      }
      ... on APIToken {
        id
        name
      }
    }
  }
}
```
{% endtab %}

{% tab title="Role by ID" %}
```graphql
query adminRole {
  roleById(
   id: "feeafade-c545"
   ) {
    createdAt
    id
    logTypeAccess
    logTypeAccessKind
    name
    permissions
    updatedAt
    updatedBy {
      __typename
      ... on User {
        email
        id
      }
      ... on APIToken {
        id
        name
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Creating a new Role

```graphql
mutation createRole {
  createRole(input: {
    name: "new-role"
    permissions: [
      UserRead
      AlertRead
    ]
  })
    {
      name
      id
      permissions
    }
}
```

### Updating a Role

Note: The permissions in the updateRole input must contain all desired permissions for the role.

```graphql
mutation updateRole {
  updateRole(input: {
    id: "fea8sje-92jdnhc"
    name: "updated-role-name"
    permissions: [
      UserRead
      AlertRead
      AlertModify
    ]
  })
    {
      name
      id
      permissions
    }
}
```

### Deleting a Role

```graphql
mutation deleterole {
  deleteRole(input: {
    id: "e146c8d8-c6a5"
  })
    {
      id
    }
}
```
