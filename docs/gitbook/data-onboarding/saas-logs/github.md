# Github

Panther has the ability to fetch Github audit logs by querying the [Github API](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization). Panther will query the Github API for new events every 1' minute.&#x20;

In order to set up Github as a log source in Panther, you'll need to authorize Panther in Github and then set up Github as a log source in Panther. There are two ways to authorize Panther to receive Github audit logs:

* **Create a new OAuth App** in Github and provide the app credentials to Panther
* **Generate a Personal Access Token** in Github and provide credentials to Panther

## Create a new OAuth App (option 1)

{% hint style="info" %}
The steps below can only be performed if you have organization owner permission in your Github organization and a Github Enterprise subscription. If you need to configure multiple integrations for different GitHub Organizations using the same credentials you can either use a Personal Access Token or an [OAuth2 App](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app) that is created on the user account, instead of the Organization account. If any Organizations [have enabled OAuth2 App Access Restrictions](https://docs.github.com/en/organizations/restricting-access-to-your-organizations-data/enabling-oauth-app-access-restrictions-for-your-organization), the app must be first approved by an Organization admin.
{% endhint %}

1. Log into your Github Enterprise account
2. On the homepage of your organization's account, click on the **Settings** tab
3. Scroll to the bottom of the page and click on **Developer Settings** and then **OAuth Apps**
4. Click on **Register an application**
5. Enter a friendly application name e.g. `Panther Integration`.&#x20;
6. Copy and paste your Panther instance's primary URL into the **Homepage URL** field e.g. [`https://test.runpanther.xyz`](https://snowflake.staging.runpanther.xyz/)
7. At this point, you will need the **Redirect URL** from Panther to paste into the **Authorization Callback URL** field. In order to do this, you will need to log into Panther and set up Github as a log source by following the directions below. Once you've made it to Step 5, you should be able to find the **Redirect URL** and continue setting up your Github app.
8. Once all necessary fields are filled in, click on **Register Application.**
9. Once the application is registered, you will be able to view the **Client ID** and then generate a new **client secret** to paste into Panther. You are done with this part!

## Generate a Personal Access Token (option 2)

{% hint style="info" %}
The steps below can only be performed if you have organization owner permission in your Github organization and a Github Enterprise subscription. You can read more on generating a Personal Access Token in Github [here](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).
{% endhint %}

1. Log into your Github Enterprise account
2. Click on your profile then click on the **Settings** option
3. Scroll to the bottom of the page and click on **Developer Settings** and then **Personal Access Token**
4. Click **Generate new token** and **e**nter a friendly token name e.g. `Panther Integration`.&#x20;
5. Select the scopes, or permissions, you'd like to grant this token.&#x20;
   * Check the boxes next to **admin:org > read:org**.&#x20;
   * You do not need to enable the **write:org** permission.
6. Click **Generate token**&#x20;
7. Copy and paste the token into the Panther UI. You are done with this part!

## Create a new Github source in Panther

{% hint style="info" %}
Once you complete one of the two options above, you can continue with the step below.
{% endhint %}

1. Login to your Panther Console
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **Github** from the list of available types
5. In the next screen, enter in a friendly name for the source e.g. `My Github Audit logs` and the name of the Github organization you want to monitor then click **Next**
6. In this page, you'll authorize Panther to receive logs from Github. Depending on the option you chose above, follow the steps below:
   1. **Created an OAuth App**: Enter the **App Client ID** and the **Client Secret** that you acquired from Github. You should be able to find this information on the details page of the OAuth app in your Github account once you **register the application.**
   2. **Generate a Personal Access Token:** Copy the personal access token key and paste it into Panther UI.
7. Click on **next** once you've filled out those fields
8. If all fields are filled out correctly, you should be presented with the option to **Grant Access.** If you see an error message, be sure to review the organization name, Client ID, and Client Secret.&#x20;
9. Click on the **Authorize (name of organization)** button
10. You should be taken to a confirmation screen where you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval).
11. Congrats, you're done!

## Panther-Built Detections

The following detections are available for use immediately:&#x20;

* Branch Policy Override
* Branch Protection Disabled
* Org Auth Modified
* Org IP Allowlist
* Org Modified
* Repo Collaborator Change
* Repo Created
* Repo Hook Modified
* Repo Initial Access
* Repo Visibility Change
* Team Modified
* User Access Key Created
* User Role Updated

Please take a look at the files in the [github\_rules](https://github.com/panther-labs/panther-analysis/tree/master/github\_rules) repository to see how these are built.&#x20;

