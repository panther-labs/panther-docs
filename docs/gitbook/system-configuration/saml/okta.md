---
description: Integrating Okta with Panther
title: Okta SAML Integration
---

# Okta

First, [deploy](../../quick-start.md) Panther and go to the General Settings page. Note the values for "Audience" and "ACS URL":

![](../../.gitbook/assets/panther-saml-parameters%20%285%29%20%281%29%20%2824%29.png)

## Create Okta App

From the Okta admin console, navigate to the Applications tab

![](../../.gitbook/assets/okta1%20%288%29%20%288%29%20%284%29%20%286%29.png)

Click "Add Application"

![](../../.gitbook/assets/okta-new-app%20%288%29%20%288%29%20%289%29%20%288%29%20%284%29.png)

Click "Create New App" and configure "Platform: Web" app and "Sign on method: SAML 2.0"

![](../../.gitbook/assets/okta2%20%288%29%20%288%29%20%285%29%20%281%29.png)

Click "Create" and configure the General Settings however you see fit. We recommend:

![](../../.gitbook/assets/okta3%20%288%29%20%288%29%20%286%29%20%281%29%20%281%29.png)

Click "Next" and configure section 2A, "SAML Settings", as follows:

![](../../.gitbook/assets/okta4%20%288%29%20%288%29%20%287%29%20%281%29.png)

The "Single sign on URL" and "Audience URI" were copied from the Panther General Settings page earlier. The "Group Attribute Statements" can be left blank \(not shown here\). Click "Next" and fill out feedback for Okta, linking to this documentation page if you like. Click "Finish."

Copy the "Identity Provider metadata" link shown on the next screen, under the Settings section of the "Sign On" tab:

![](../../.gitbook/assets/okta-metadata%20%288%29%20%288%29%20%289%29%20%287%29%20%286%29.png)

This is the "Identity provider URL" you will need to give to Panther.

Finally, be sure to grant access to the appropriate people/groups in the "Assignments" tab.

### Create an Okta "Bookmark app"

Amazon Cognito \(which powers Panther's user management\) does not currently support IdP-initiated logins, meaning you cannot login to Panther directly from Okta. However, you can simulate an IdP-initiated flow with an Okta Bookmark app. With the Bookmark application, end users can click a chiclet in Okta to sign into Panther. To configure a Bookmark app for Panther, follow the instructions in the [Okta docs](https://help.okta.com/en/prod/Content/Topics/Apps/Apps_Bookmark_App.htm).

## Configure Panther

From the Panther settings page, enable SAML with a default [Panther role](../rbac.md) of your choice and paste the Okta metadata URL you just copied:

![](../../.gitbook/assets/okta-panther%20%288%29%20%281%29%20%281%29.png)

Click "Save" and then you're done! Now, clicking the "Login with SSO" button will redirect you to Okta:

![](../../.gitbook/assets/panther-login-sso%20%286%29%20%281%29%20%284%29.png)

