# Slack

Panther has the ability to pull the following logs from Slack:

* **Audit logs**, by querying the [Audit Logs API](https://api.slack.com/admins/audit-logs).&#x20;
  * The Audit Logs API is available to **Slack Enterprise Grid** customers **only**.
* **Access logs**, by querying the [team.accessLogs API](https://api.slack.com/methods/team.accessLogs).&#x20;
  * This API is available in all Slack paid plans.&#x20;
  * Note: Due to Slack's rate limits, Panther pulls only the events where the user or the access location or the access device is new.
* **Integration logs**, by querying the [team.integrationLogs API](https://api.slack.com/methods/team.integrationLogs).&#x20;
  * This API is available in all Slack paid plans.

Panther will query the API every 1 minute.

In order for Panther to access the Slack API you need to create a new Slack source on Panther, create a Slack App, and provide the app credentials to Panther.

## Create a new Slack Source in Panther

1. Log in to your Panther Console.
2. Go to **Integrations** > **Log** **Sources** from the sidebar menu.
3. Click **Add Source**.
4. Select **Slack** from the list of available types.
5. Enter a name for the source (e.g. `My Slack logs`) and then select your Slack plan. \
   ![](../../.gitbook/assets/slack-onboarding.png)
   * The available log types depend on which plan you are subscribed to. To find your Slack plan, click the name of your Slack workspace at the top left of the Slack app.
6. Click **Continue Setup**.
7. On the Set Credentials page, click **Copy Redirect URL** and save it somewhere temporarily, as you will need it in the next steps.&#x20;

![](../../.gitbook/assets/slack-setup-page2.png)

Keep this browser window open while you work through the next steps.

## Create a new Slack App

Next, you will create a Slack app with permissions to pull logs from Slack. For security and availability reasons, we recommend creating a **new** Slack App that will be used only by Panther.

You can create an app for:

* [**Audit logs**](https://docs.runpanther.io/data-onboarding/saas-logs/slack#audit-logs)
* [**Access or Integration logs**](https://docs.runpanther.io/data-onboarding/saas-logs/slack#access-logs)

### Create a Slack App to pull Audit Logs <a href="#audit-logs" id="audit-logs"></a>

1. [Sign in to the Slack workspace](https://slack.com/workspace-signin) belonging to the Enterprise grid you want to monitor.&#x20;
   * You must sign-in as an **owner** of the organization.
2. You will be presented with a screen displaying all the workspaces in your Enterprise Grid. Click **Launch in Slack** on a workspace you want to monitor.&#x20;
3.  Go to [Slack apps](https://api.slack.com/apps) and click **Create New App**.\
    ![](../../.gitbook/assets/slack-new-app.png)

    * Enter an **App Name** e.g. `Panther monitoring`.
    * Select the workspace you signed in earlier.&#x20;

    ![](../../.gitbook/assets/create-slack-app.png)
4. Click **Create App**.
   * The App will be created in the selected workspace but later you will be able to use it to monitor the entire Enterprise Grid organization.
5. In the left sidebar menu, click **OAuth & Permissions**.
6. Scroll down to the **Redirect URLs** section.
7. Click **Add** and enter the **Redirect URL** that you copied from the Panther Console in the previous section of this documentation.\
   ![](../../.gitbook/assets/slack-redirect.png)
8. Click **Save URLs**.
9. Scroll down to the **User Token Scopes** section. Add the `auditlogs:read` scope.\
   ![](../../.gitbook/assets/slack-scopes.png)
10. In the left sidebar, go to **Settings >** **Manage Distribution.**&#x20;
11. Under the section titled "Share Your App with Other Workspaces," enable the following options:
    * **Enable Features & Functionality**
    * **Add OAuth Redirect URLs**
    * **Remove Hard Coded Information**
    * **Use HTTPS For Your Features**
12. Click **Activate Public Distribution**.
    * Note: This does not make your Slack App accessible to other organizations. Slack requires this setting to pull [audit logs](https://api.slack.com/admins/audit-logs).\
      ![](../../.gitbook/assets/slack-public-dist.png)
13. In the left sidebar, go to **Settings** > **Basic Information**.
14. In the **App Credentials** section, Copy the **Client ID** and **Client Secret**.
15. Follow the steps under [Finalize Slack Onboarding in Panther](https://docs.runpanther.io/data-onboarding/saas-logs/slack#finalize) to complete this process.

![](../../.gitbook/assets/slack-setup-page9.png)



### Create a Slack App to pull Access or Integration Logs <a href="#access-logs" id="access-logs"></a>

1. [Sign in to the Slack workspace](https://slack.com/workspace-signin) belonging to the Enterprise grid you want to monitor.&#x20;
   * You must sign-in as an **owner** of the organization.
2. You will be presented with a screen displaying all the workspaces in your Enterprise Grid. Click **Launch in Slack** on a workspace you want to monitor.&#x20;
3.  Go to [Slack apps](https://api.slack.com/apps) and click **Create New App**.\
    ![](../../.gitbook/assets/slack-new-app.png)

    * Enter an **App Name** e.g. `Panther monitoring`.
    * Select the workspace you signed in earlier.&#x20;

    ![](../../.gitbook/assets/create-slack-app.png)
4. Click **Create App**.
   * The App will be created in the selected workspace but later you will be able to use it to monitor the entire Enterprise Grid organization.
5. In the left sidebar menu, click **OAuth & Permissions**.
6. Scroll down to the **Redirect URLs** section.
7. Click **Add** and enter the **Redirect URL** that you copied from the Panther Console in the previous section of this documentation.\
   ![](../../.gitbook/assets/slack-redirect.png)
8. Click **Save URLs**.
9. Scroll down to the section titled **Scopes** > **User Token Scopes**. Add the `admin` scope.
10. In the left sidebar, go to **Settings** > **Basic Information**.
11. In the **App Credentials** section, Copy the **Client ID** and **Client Secret**.
12. Follow the steps under [Finalize Slack Onboarding in Panther](https://docs.runpanther.io/data-onboarding/saas-logs/slack#finalize) to complete this process.

![](../../.gitbook/assets/slack-setup-page9.png)

## Finalize Slack onboarding in Panther <a href="#finalize" id="finalize"></a>

1. Go back to the Slack onboarding wizard in the Panther Console.
2. Paste **Client ID** and **Client Secret** credentials of the Slack App you just created.
3. Click **Continue Setup**.&#x20;
   * The credentials will be stored, encrypted, in the Panther backend.
4. Click **Save Source**.
5. On the Verify Setup screen, click **Grant Access**.
   * You will be redirected to a Slack page to install your app.
   * For Audit Logs, make sure you install it to the **Enterprise Organization** and **not** to a specific workspace!
6. Click **Allow.**

Your new Slack Source should be healthy and ready to fetch audit logs from Slack!

{% hint style="warning" %}
Note: The integration will stop working if:

* the account of the user that installed the app to the organization is deactivated
* the app was deleted, the access token was revoked, or the app credentials are rotated
{% endhint %}
