# System Health Notifications

Panther System Health notification system alerts users when a part of the Panther platform is not functioning correctly. Currently, that includes the following system health errors:

* Log sources turning unhealthy as a result of a failed health check
* Logs dropping off entirely from a log source

This section covers how users can configure these system health notifications to fit their workflows. Learn more below!

### Log Source Health Notifications

{% hint style="info" %}
This feature is available in Panther version 1.17
{% endhint %}

Panther performs health checks on log sources to ensure that Panther is correctly linked to the source, has the right credentials, and is receiving data from the source consistently. When those health checks fail for a given log source, Panther will send a "System Error" alert to the **Alerts** page and to an alert destination.

To ensure you're properly receiving alerts for these System Health errors, configured the following settings:

* Configured an alert destination that is receiving the "System Error" alert type.
* Configured alarms for log sources that will trigger an alert when data is no longer being received.

#### Configure Delivery of System Health Error Alerts

By default, Panther will send these alerts to the **Alerts** page in the Panther UI. To ensure these alerts are sent to a custom alert destination, follow the below steps:

1. Go to **Integrations**&gt;**Alert Destinations**
2. Choose an existing Alert Destination or add a new Alert Destination
3. When you arrive at the **Alert Destination Configuration** form, be sure to add the **System Errors** to the Alert Types field \(as seen below\).

![](../.gitbook/assets/image%20%281%29.png)

#### Configure Alarms for Log Drop-offs for your Log Sources

Panther allows users to set up "alarms" for individual log sources that will trigger an alert if data is not received over a specific time interval. This can be useful for log sources that have been incorrectly linked to Panther or are experiencing issues outside of Panther. 

You can set up an alarm in two ways. You can add an alarm to an existing log source by following the instructions below:

1.  Go to **Integrations**&gt;**Log Sources**
2. Find a log source that you would like to configure an alarm for and click on the three-dotted **more options** icon on the far right of the tile
3. Select **Alarm Configuration**
4. Turn the alarm off or on and view which destinations a "System Error" alert will be sent to

![](../.gitbook/assets/image%20%289%29.png)

You can also add an alarm when onboarding a new log source, see below:

1. In the Log Source page, click **Add Source** at the top of the page
2. Complete the onboarding experience
3. At the end of the experience, select **Yes** when asked to configure an alarm
4. Turn the alarm off or on and view which destinations a "System Error" alert will be sent to

Once you have alarms configured and alert destinations updated, you should be receiving health alerts!

### Log Classification Error Notifications

{% hint style="info" %}
This feature is available in Panther version 1.19
{% endhint %}

Log misclassification alerts are generated when logs hit a parsing error and fail to classify when sent to Panther. When this happens, the following actions take place **by default:**

* Misclassified logs are sent to the data lake and are searchable in a table called `classification_failures` in the `panther_monitor` database.
* An alert is generated immediately after the first log fails to classify. The alert will display all log lines that are failing to classify. 

This type of alert is classified as a "System Error", similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" tab in the Panther UI.

The alert details page will highlight which log lines are failing to parse correctly which should help inform which lines in the log type's respective schemas need to be corrected or added.

### S3 GetObject Error Notifications

{% hint style="info" %}
This feature is available in Panther version 1.21
{% endhint %}

S3 GetObject error alerts are generated when Panther fails to fetch S3 objects. When this happens, the following actions take place by default:

* Panther stores the S3 objects in the data lake which can be queried through the Data Explorer in a table titled `panther_monitor.data_audit`
* An alert will fire off if Panther fails to fetch any S3 object in the last 24hours. The alert will display the specific S3 objects that are failing.

This type of alert is classified as a "System Error", similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" tab in the Panther UI. 

