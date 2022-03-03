---
description: Integrating SAML with Panther
---

# Generic

Integrate any SAML Identity Provider (IdP) with Panther in three easy steps:

1.  [Deploy](../../quick-start.md) Panther and navigate to the General Settings page. Note the values shown for "Audience" and "ACS URL":

    ![](<../../../../.gitbook/assets/panther-saml-parameters (5) (1) (1) (1) (21).png>)
2. Add a "test" or "manual" SAML integration to your IdP with the following settings:
   * Audience: `urn:amazon:cognito:sp:USER_POOL_ID` (copied from the General Settings in Panther)
   * ACS / Consumer URL: `https://USER_POOL_HOST/saml2/idpresponse` (copied from the General Settings in Panther)
   * SAML Attribute Mapping:
     * `PantherEmail` -> user email
     * `PantherFirstName` -> first/given name
     * `PantherLastName` -> last/family name
   * Grant access to the appropriate users
3. From the Panther settings page, enable SAML with:
   * A default [Panther role](../rbac.md) of your choice
   * The issuer/metadata URL from the SAML integration in your IdP. This URL should be a publicly accessible XML document.

{% hint style="info" %}
If your IdP lets you download the metadata XML file directly but does not provide a URL, you will need to publish that file somewhere public (it should not contain sensitive information). For example, you can upload to a public S3 bucket and then give Panther the S3 URL.

We are working on support for direct metadata uploads in the near future.
{% endhint %}

Click "Save" in the Panther settings page and then you're done! Now, clicking the "Login with SSO" button will redirect you to your Identity Provider:

![](<../../../../.gitbook/assets/panther-login-sso (6) (1) (1) (1) (21).png>)

For examples, see the [OneLogin](onelogin.md) and [Okta](okta.md) integration guides.
