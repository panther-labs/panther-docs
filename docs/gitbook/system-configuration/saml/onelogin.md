---
description: Integrating OneLogin with Panther
---

# OneLogin

First, go to the General Settings page and copy the values for "Audience" and "ACS URL":

![](<../../../../.gitbook/assets/panther-saml-parameters (5) (1) (1) (1) (11) (22).png>)

You will need these to configure your OneLogin App.

## Create OneLogin App

From the OneLogin Admin console, navigate to the Applications page:

![](<../../../../.gitbook/assets/onelogin1 (8) (8) (9) (2) (1) (1) (2) (9).png>)

Click the "Add App" button on the top right of the page and search for "SAML Test Connector (Advanced):"

![](<../../../../.gitbook/assets/onelogin2 (5) (5) (7) (8) (1) (1) (3) (6).png>)

Fill in the following:

1. Display Name (e.g. "Panther Enterprise")
2. Logo Icon
3. Description

{% hint style="info" %}
We recommend disabling the "visible in portal" button since SAML logins can only be initiated from Panther.
{% endhint %}

Click "Save."

Now, open the new application's "Configuration" page and fill in the "Audience" and "ACS Consumer" values found in the Panther General Settings page above:

![](<../../../../.gitbook/assets/onelogin3 (5) (5) (7) (8) (1) (1) (3) (6).png>)

In the Parameters tab, add Panther's field mappings:

* `PantherFirstName`: `First Name`
* `PantherLastName`: `Last Name`
* `PantherEmail`: `Email`

For each parameter, be sure to also check the "Include in SAML assertion" flag:

![](<../../../../.gitbook/assets/onelogin4-inset (8) (4) (1) (1) (1) (2) (9).png>)

When complete, you should see:

![](<../../../../.gitbook/assets/onelogin4 (8) (8) (9) (5) (1) (1) (2) (9).png>)

Finally, in the "SSO" tab, strengthen the algorithm to SHA-512 and copy the Issuer URL:

![](<../../../../.gitbook/assets/onelogin5 (8) (8) (9) (6) (1) (1) (2) (9).png>)

This is the "Identity provider URL" you will need to give to Panther.

Save your OneLogin App settings.

Don't forget to grant access to the appropriate users or groups!

## Configure Panther

To finalize the SSO configuration in Panther:

1. Navigate to your Panther "General Settings" page
2. Flip the "Enable SAML" button
3. Set a default [Panther Role](../rbac.md) of your choice
4. Paste the OneLogin issuer URL copied above:

![](<../../../../.gitbook/assets/onelogin-panther (2) (1) (1) (3) (8).png>)

Click "Save" and you're done!

Now clicking the "Login with SSO" button will redirect you to your company's OneLogin:

![](<../../../../.gitbook/assets/panther-login-sso (6) (1) (1) (1) (11) (22).png>)
