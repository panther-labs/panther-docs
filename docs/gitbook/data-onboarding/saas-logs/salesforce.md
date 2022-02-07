# Salesforce

Panther has the ability to fetch [Salesforce Event Monitoring](https://trailhead.salesforce.com/content/learn/modules/event\_monitoring/event\_monitoring\_intro) logs for the following event types:

* [Login](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_login.htm)
* [LoginAs](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_loginas.htm)
* [Logout](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_logout.htm)
* [URI](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_uri.htm)

{% hint style="info" %}
Salesforce customers must [enable Event Monitoring](https://help.salesforce.com/articleView?id=000339868\&type=1\&mode=1) first. An additional license may be required for this Salesforce add-on.
{% endhint %}

## Create an API User

Panther requires a user account with API and Event Log File permissions in order to retrieve Event Monitoring logs.

We recommend creating a new, dedicated user with the minimum permissions required by Panther. Salesforce requires each user to have a [unique username](https://help.salesforce.com/articleView?id=sf.basics\_intro\_usernames\_passwords.htm\&type=5), but the same email address can be included in multiple users. Thus, you can create a Panther-only account without having to manage an additional email address in your organization.

{% hint style="info" %}
In order to create and add permissions to the new user, the ['Manage Users' permission](https://help.salesforce.com/articleView?id=000324398\&type=1\&mode=1) is required.
{% endhint %}

Follow the instructions in the [Salesforce documentation](https://help.salesforce.com/articleView?id=sf.adding\_new\_users.htm\&type=5) to add a new user. For the **User License** and **Profile** fields, make sure "Salesforce" and Read Only" are selected, respectively. (see below)

![User License and Profile](../../../../.gitbook/assets/create-user-profile.png)

Complete the user registration process by setting a new password through the link sent to your email.

### Retrieve Security Token <a href="#retrieve-security-token" id="retrieve-security-token"></a>

Salesforce API access requires, in addition to the username and password, a credential named _Security Token_.

In order to request a security token for new Salesforce user account, you can follow the instructions [here](https://help.salesforce.com/articleView?id=sf.user\_security\_token.htm\&type=5). The security token will be sent via email to the account email address.

### Create and assign a new Permission Set

In order to assign permissions to the new user we need to create a new [Permission Set](https://help.salesforce.com/articleView?id=perm\_sets\_overview.htm\&type=5). Follow the instructions in the [Salesforce documentation](https://help.salesforce.com/articleView?id=sf.perm\_sets\_create.htm\&type=5) to add a new permission set that will grant Panther access to the Event Monitoring data via the SOAP/REST API.

After creating the permission set, go to **System Permissions** by clicking on the link:

![System Permissions Link](../../../../.gitbook/assets/system-permissions.png)

Click on the **Edit** button and select the following permissions:

**API Enabled** ![API Enabled](../../../../.gitbook/assets/api-enabled-permission.png)

**Event Log Files** ![Event Log Files](../../../../.gitbook/assets/view-event-log-files-permission.png)

After the System Permissions have been updated, you can assign the Permission Set to the designated user by following the instructions [here](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/perm\_sets\_assigning.htm).

## Create a new Salesforce Source in Panther

1. Login to your Panther deployment
2. Go to **Integrations** > **Log Sources**
3. Click the "plus" icon at the top right of the page to add a new log source
4. Select **Salesforce** from the list of available sources
5. Click **Start Source Setup**
6. Enter a friendly name for the source, e.g. `Salesforce Logs`
7. Be sure to select the time interval (hourly or daily) in which you want files retrieved from Salesforce _(This feature was introduced in version 1.24)_
   * _Check with your Salesforce admin to determine how your Salesforce instance is configured and what file interval is supported._
8. Select which log types you would like to monitor
9. Next, fill in the credentials of the account that Panther will use to connect to the Salesforce API:
   * **Account Username**: e.g. `panther-logs@mycompany.com`
   * **Account Password**: the account password
   * **Security Token**: the Security Token (see [here](salesforce.md#retrieve-security-token) for instructions)
10. Click **Continue Setup**. At this step the credentials are also verified.

You are done! You can now start writing detections and exploring your Salesforce data.

![New Salesforce Log Source](<../../.gitbook/assets/Screen Shot 2021-10-21 at 5.16.34 PM.png>)
