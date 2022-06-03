---
description: User and Role APIs are available in version 1.36 and newer
---

# User Management

The Panther API supports the following user operations:

* List users
* Get a user by ID or by email address
* Invite new users
* Update a user's information and role
* Delete users

See the sections below for GraphQL queries and mutations around certain common operations.

## Common user operations

Below are some of the most common GraphQL operations in Panther. These examples demonstrate how you can use a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

For more information on what fields and operations exist in the API, please see the [Discovering the Schema documentation](https://docs.runpanther.io/api-beta/discovering-the-schema).

### List users

```graphql
query users {
  users {
    id
    givenName
    familyName
    email
    status
    createdAt
    role{
      name
      id
      permissions
    }
  }
}
```

### Retrieving a User

{% tabs %}
{% tab title="User by Email Address" %}
```graphql
query user {
  userByEmail(email: "example@domain.com") {
    id
    givenName
    familyName
    email
    role{
      name
      id
      permissions
    }
    status
    createdAt
  }
}
```
{% endtab %}

{% tab title="User by ID" %}
```graphql
query user {
  userByID(id: "8h81hfh-ij828db") {
    id
    givenName
    familyName
    email
    role{
      name
      id
      permissions
    }
    status
    createdAt
  }
}
```
{% endtab %}
{% endtabs %}

### Invite a New User

```graphql
mutation inviteUser {
    inviteUser(input: {
        email: "example@domain.com"
        givenName: "firstname"
        familyName: "lastname"
        role: {
            kind: NAME
            value: "Analyst"
        }
    })
    {
        user{
            id
            status
        }
    }
  }
```

### Updating a User

```graphql
mutation updateUser {
    updateUser(input: {
        id: "8h81hfh-ij828db"
        familyName: "newName"
    })
    {
        users{
            email
            givenName
            familyName
        }
    }
}
```

### Deleting a User

```graphql
mutation deleteUser{
  deleteUser(input: {
    id: "8h81hfh-ij828db"
  })
  {
    user{
      email
      id
    }
  }
}
```
