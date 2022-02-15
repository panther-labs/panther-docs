# Microsoft 365

Panther has the ability to pull logs from Microsoft's [Office 365 Management Activity API](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-reference). Panther will query the API every 5 minutes.

## Prerequisites

These are the steps required to enable Panther ingest Office 365 activity logs.

1\. Enable [audit log search](https://docs.microsoft.com/en-us/microsoft-365/compliance/turn-audit-log-search-on-or-off?view=o365-worldwide#turn-on-audit-log-search) for your Microsoft365 tenancy, done through the Security and Compliance Center in the Office 365 Admin Portal.

2\. Register an application in Azure Active Directory. See below for steps on how to create an application.

3\. Create a Microsoft source integration in Panther.

## Registering an application in Azure AD

1. Log in to [your Azure portal](https://portal.azure.com) and navigate to the **Azure Active Directory** service.\
   ![](../../.gitbook/assets/select-azure.png)
2. Click **App Registrations** in the left sidebar and then **New Registration**.
3. Enter a friendly name for your application. In the **Supported account types** field, select `Accounts in this organizational directory only`.&#x20;
4. Click **Register**.
5. On the left sidebar, click **Certificates and Secrets**. Then click **New Client Secret**. Add a description for the secret (e.g Panther integration) and set the **Expires** field to `24 Months`.&#x20;
6. Click **Add**.&#x20;
   * The client secret will be hidden after you navigate away from this page, so copy the **Value** field (not **Secret ID** field) before continuing.
7. On the left sidebar, click **API Permissions** and then **Add a permission**. Find and click the **Office 365 Management APIs**.
8. Click **Delegated permissions** and select all permissions: _ActivityFeed.Read, ActivityFeed.ReadDlp, ServiceHealth.Read_.\
   ![](../../.gitbook/assets/o365-permissions.png)
9. Click **Application permissions** and select all permissions: _ActivityFeed.Read, ActivityFeed.ReadDlp, ServiceHealth.Read_.
10. Click **Add permissions** at the bottom.&#x20;
    * Make sure you have added both **Delegated** and **Application** permissions in the previous two steps.
11. Click **Grant admin consent** in the API permissions page.\
    ![](../../.gitbook/assets/grant-admin-consent.png)
12. After consent has been granted, click the **Overview** tab in the left sidebar to view your **Application (client) ID** and

    **Directory (tenant) ID**. You will need to provide these to Panther in the next steps.\
    ![](../../.gitbook/assets/o365-overview-ids.png)

## Create a new Microsoft Source in Panther

1. Log in to your Panther account.
2. Go to **Integrations** > **Log Sources** from the sidebar menu.
3. Click **+** in the upper right.
4. Click the tile for **Microsoft**.
5. Click **Start Source Setup**.
6. Enter a name for the source (e.g. "Microsoft 365 logs") and select the log types to ingest.&#x20;
7. Click **Continue Setup**.
8. On the Credentials page, enter your Tenant ID, Client ID, and Secret ID Value.&#x20;
9. Click **Continue Setup**.&#x20;
10. Panther will verify access to Microsoft 365. On the Verify Setup screen, you can set up a log drop-off alarm (this will send an error message if logs aren't received within a specified time interval.)&#x20;
11. Click **Finish Setup**.

![ ](../../../../.gitbook/assets/microsoft-credentials.png)

You are done! You can now start writing detections and exploring your Microsoft 365 activity logs.

{% hint style="warning" %}
After the integration is created, it may take up to 12 hours for the Microsoft API to make data available for the first time.
{% endhint %}
