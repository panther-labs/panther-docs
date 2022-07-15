# SAML/SSO Integration

Panther can be integrated with SAML providers like OneLogin, Okta, and others to enable users to log in via SSO to the Panther Console.

## Guides

Follow these step-by-step guides to enable SAML integration with one of the following:

* [G Suite](gsuite.md)
* [OneLogin](onelogin.md)
* [Okta](okta.md)
* [Other](generic.md)

## Terminology

* **Identity Provider (IdP)**: **** The system that provides authentication credentials, such as OneLogin, Okta, and others.
* **Security Assertion Markup Language (SAML)**: An open standard for exchanging authentication credentials.
* **Service Provider (SP)**: The system that receives authentication credentials. In this case, Panther Enterprise.
* ****[**Single Sign-On (SSO)**](../../help/glossary.md#sso-single-sign-on): A central hub that allows users to share one login session with multiple services. In this context, synonymous with a SAML IdP.

## Features

* **SP-initiated login flow**: **** Panther will show a special link on the login page which, when clicked, will redirect to the IdP for login
* **Auto-provisioning**: Panther SAML accounts are created on the first login; they do not need to be created in advance
* **Role integration**: **** A single [Panther Role](../rbac.md) of your choice is assigned to SAML users by default, and you can change user roles after their first login

Standard password-based logins are still supported after you enable SAML integration. Users can be created and authorized in either flow.

## Limitations

Panther does not support the following:

* **IdP-initiated login flow**: Users cannot login from OneLogin or Okta directly, they must navigate to the Panther login page first
* **SCIM**: Users deleted from the IdP are not automatically deleted from Panther (they just cannot login anymore)
* **Attribute mapping**: **** Panther roles cannot be assigned via SAML attributes

These limitations stem from Amazon Cognito, the user management service Panther is built on.
