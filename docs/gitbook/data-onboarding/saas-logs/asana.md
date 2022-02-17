# Asana

{% hint style="info" %}
This feature is available in versions 1.29 and newer.
{% endhint %}

Panther has the ability to fetch Asana audit logs by querying the [Asana Audit Log API](https://asana.com/guide/help/api/audit-log-api).

Follow the steps below to set up an Asana source in Panther.

1. Login to your Panther account.
2. From the left sidebar menu, click **Integrations** > **Log** **Sources**.
3. Click **Add Source.**
4. Select **Asana** from the list of available log sources.
5. In the next screen, enter a friendly name for the source then click **Next.**
6. Enter the credentials required for the integration.&#x20;
   1. Open a new browser tab and [Sign in](https://app.asana.com/-/login) to your Asana account. **Make sure to login as administrator**.
   2. Click your profile picture at the top right. Click **Admin Console** and then click **Settings** on the left.
   3. At the bottom of the page you'll find the **Domain ID**. Copy it and then paste it into the **Organization Id** field in Panther.
   4. In your Asana account, click **Apps** on the left sidebar.
   5. At the bottom of the page, click **Add Service Account**.
   6. Specify a name. Copy the token and then click **Save changes**.
7. Navigate to Panther and paste the Asana token into the **Service Account Token** field in Panther.
8. Click **Continue Setup**.
   * Panther will verify access to Asana. If you see an error, be sure to review the credentials you provided to Panther.&#x20;
9. On the confirmation screen, you can set up a log drop-off alarm to send an error message if Panther does not receive logs within a specified time interval.
10. Click **Finish Setup**.
