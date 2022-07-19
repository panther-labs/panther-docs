---
description: Setting up Okta SSO to log in to the Panther Console
---

# Okta SSO

## Overview

Panther supports integrating with Okta as a SAML provider to enable logging in to the Panther Console via SSO.

For more information on features, terminology, and limitations of SSO integrations with the Panther Console, please see the Panther documentation: [SAML/SSO Integration](https://docs.panther.com/system-configuration/saml).

## How to configure SAML SSO to the Panther Console with Okta

### Obtain the Okta SSO parameters from Panther

1. Log in to the Panther Console.
2. In the left sidebar, click **Settings > General**.
3. Click the SAML Configuration tab.

Keep this browser window open, as you will need the **Audience** and **ACS URL** values in the next steps.

![The General Settings page in Panther is open to the SAML Configuration tab, which displays the Audience and ACS URL fields.](../../.gitbook/assets/panther-sso.png)

### Create the Okta App

1. Log in to your Okta administrative console.
2.  &#x20;Click the **Applications** tab, then click **Create App Integration**.

    ![](<../../.gitbook/assets/Screen Shot 2022-06-24 at 10.52.46 AM (1).png>)
3. Within the "Create a new app integration" modal, fill in the form to configure the new app:
   * **Sign on Method**: Select SAML 2.0\
     ![](<../../.gitbook/assets/image (19).png>)
4. Click **Next**.
5. Configure the general settings:
   * **App name**: Add a memorable name such as "Panther Console."&#x20;
   * **App logo**: Upload a Panther logo to help users quickly identify this app.
   * **App visibility**: Configure the visibility of this application for your users.
6. Click **Next**.&#x20;
7.  In the _SAML Settings_ section, configure the following under **General**:

    * **Single sign on URL**: Enter the **ACS URL** you copied from the Panther Console in earlier steps of this documentation.
    * **Audience**: Enter the **Audience** you copied from the Panther Console in earlier steps of this documentation.

    ![](<../../.gitbook/assets/image (36) (2).png>)
8. Configure the following under **Attribute Statements**:
   * **Name**: `PantherEmail`, **Value**: `user.email`
   * **Name**: `PantherFirstName`, **Value**: `user.firstName`
   * **Name**: `PantherLastName`, **Value**: `user.lastName`\
     ``![](<../../.gitbook/assets/image (53).png>)``
9. The Group Attribute statements can be left blank. Click **Next**.
10. Click **Finish**.
11. On the next screen, navigate to **SAML Setup** along the right-hand side of the screen.

    ![](<../../.gitbook/assets/Screen Shot 2022-06-24 at 11.12.08 AM.png>)
12. Click **View SAML setup instructions** which will open up a new browser tab.
13. Copy the **Identity Provider Single Sign-On URL**. Okta displays the URL in the following format:

    * https://\[OKTA\_ACCT].[okta.com/app/\[OKTA\_APP\_STR\]/\[APP\_ID\]/sso/saml](http://okta.com/app/\[OKTA\_APP\_STR]/\[APP\_ID]/sso/saml)

    Adjust the URL as follows in order to use it with Panther:

    * https://\[OKTA\_ACCT].[okta.com/app/\[APP\_ID\]/sso/saml/metadata](http://okta.com/app/\[APP\_ID]/sso/saml/metadata)
    * Copy this URL as you will need it in the following steps.
14. Grant access to the appropriate users and groups in the **Assignments tab**.

### Create an Okta Bookmark app

Amazon Cognito, which powers Panther's user management, does not support IdP-initiated logins. However, you can simulate an IdP-initiated flow with an Okta Bookmark app, which will allow users to click a tile in Okta to sign in to Panther. To configure a Bookmark app for Panther, follow the instructions in the Okta Help Center: [Simulate an IdP-initiated flow using the Bookmark App](https://help.okta.com/en/prod/Content/Topics/Apps/Apps\_Bookmark\_App.htm).

### Configure Okta SAML in Panther

1. Navigate back to the [SAML configuration](okta.md#obtain-the-g-suite-sso-parameters-from-panther) you started earlier in this documentation.
2. Next to "Enable SAML", set the toggle to **ON**.&#x20;
3. In the "Default Role" field, choose the Panther role that your new users will be assigned by default when they first log in via SSO.
4. In the **Identity Provider URL** field, paste the metadata URL from Okta that you obtained in the previous steps of this documentation.
5. Click **Save Changes**.

To test your setup, go to your Panther sign-in page and click **Login with SSO**.

![The Panther login page displays a "Login with SSO" button at the bottom.](<../../../../.gitbook/assets/panther-login-sso (6) (1) (1) (1) (11) (1) (1) (1) (10) (24).png>)
