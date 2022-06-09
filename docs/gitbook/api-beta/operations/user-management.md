---
description: Panther API user and role management operations
---

# User & Role Management

{% hint style="info" %}
User and Role API operations are available in version 1.36 and newer.
{% endhint %}

The Panther API supports the following user operations:

* List users
* Get a user by ID or by email address
* Invite new users
* Update a user's information and role
* Delete users
* List roles
* Get a role by ID or by name
* Create a new role
* Update a role's information and permissions
* Delete roles

See the sections below for GraphQL queries and mutations around certain common operations.

## Common user operations

Below are some of the most common GraphQL operations in Panther. These examples demonstrate how you can use a GraphQL client (or `curl`) to make a call to Panther's GraphQL API.&#x20;

#### Listing Users

```graphql
query users {
  users {
    id
    givenName
    familyName
    email
    status
    createdAt
    role {
      name
      id
      permissions
    }
  }
}
```

#### Retrieving a User

{% tabs %}
{% tab title="User by Email Address" %}
```graphql
query user {
  userByEmail(email: "example@domain.com") {
    id
    givenName
    familyName
    email
    role {
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
  userById(id: "8h81hfh-ij828db") {
    id
    givenName
    familyName
    email
    role {
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

#### Inviting a new User

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
        user {
            id
            status
        }
    }
  }
```

#### Updating a User

```graphql
mutation updateUser {
    updateUser(input: {
        id: "8h81hfh-ij828db"
        familyName: "newName"
    })
    {
        user {
            email
            givenName
            familyName
        }
    }
}
```

#### Deleting a User

```graphql
mutation deleteUser{
  deleteUser(input: {
    id: "8h81hfh-ij828db"
  })
  {
    id # the delete operation doesn't return a `user` object; just an ID
  }
}
```

#### Listing Roles

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

#### Retrieving a Role

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

#### Creating a new Role

```graphql
mutation createRole {
  createRole(input: {
    name: "new-role"
    permissions: [
      UserRead
      AlertRead
    ]
  })
    role {
      name
      id
      permissions
    }
}
```

#### Updating a Role

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
    role {
      name
      id
      permissions
    }
}
```

#### Deleting a Role

```graphql
mutation deleteRole {
  deleteRole(input: {
    id: "e146c8d8-c6a5"
  })
    {
      id # The delete operation doesn't return a `role` object. Just an ID.
    }
}
```

## End-to-End Examples

Below, we will build on the [Common Operations](user-management.md#common-operations) examples to showcase an end-to-end flow.

#### **Create a new user administrator role and invite a user to that role.**

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

create_user_admin = gql(
  """
  mutation newAdminRole {
    createRole (input: {
      name: "user-admin"
      permissions: [
        UserRead
        UserModify
        OrganizationAPITokenRead
        GeneralSettingsRead
      ]
    }) 
    {
      role {
        name
        id
      }
    }
  }
  """
)

invite_user = gql(
  """
  mutation inviteUser($input: InviteUserInput!) {
    inviteUser (input: $input) {
      user {
        id
        email
      }
    }
  }
  """
)

new_role_data = client.execute(
  create_user_admin
)

print(f'new role ID is {new_role_data["createRole"]["role"]["id"]}')

response_data = client.execute(
  invite_user,
  variable_values= {
    "input": {
      "email": "newAdmin@domain.local",
      "givenName": "user",
      "familyName": "admin",
      "role": {
        "kind": "ID",
        "value": new_role_data["createRole"]["role"]["id"],
      }
    }
  }
)

print(f'Successfully invited user {response_data["inviteUser"]["user"]["email"]} with role {new_role_data["createRole"]["role"]["name"]}.'
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { GraphQLClient, gql } from "graphql-request";

const client = new GraphQLClient(
  "YOUR_PANTHER_API_URL",
  { headers: { "X-API-Key": "YOUR_API_KEY" } }
);

// `createUserAdmin` is a nickname for the query. You can fully omit it.
const createUserAdmin = gql`
  mutation newAdminRole {
    createRole(
      input: {
        name: "user-admin"
        permissions: [
          UserRead
          UserModify
          OrganizationAPITokenRead
          GeneralSettingsRead
        ]
      }
    ) {
      role {
        name
        id
      }
    }
  }
`;

// `inviteUser` is a nickname for the mutation. You can fully omit it.
const inviteUser = gql`
  mutation inviteUser($input: InviteUserInput!) {
    inviteUser(input: $input) {
      user {
        id
        email
      }
    }
  }
`;

(async () => {
  try {
    const newRoleData = await client.request(createUserAdmin);

    const inviteUserOutput = await client.request(inviteUser, {
      input: {
        email: "newAdmin@domain.local",
        givenName: "user",
        familyName: "admin",
        role: {
          kind: "ID",
          value: newRoleData.createRole.role.id
        }
      }
    });

    console.log(
      `Successfully invited user ${inviteUserOutput.inviteUser.user.email} with role ${newRoleData.createRole.role.name}.`
    );
  } catch (err) {
    console.error(err);
  }
})();

```
{% endtab %}
{% endtabs %}

