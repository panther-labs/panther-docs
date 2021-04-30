# System Health Notifications

{% hint style="info" %}
This feature is available in Panther version 1.17
{% endhint %}

Panther System Health notification system alerts users when a part of the Panther platform is not functioning correctly. Currently, that includes the following system health errors:

* Log sources turning unhealthy as a result of a failed health check
* Logs dropping off entirely from a log source

This section covers how users can configure these system health notifications to fit their workflows. Learn more below!

### Log Source Health Notifications

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
3. Select **Add Alarm**
4. Choose a time interval after which you would like to receive a "System Error" alert

![](../.gitbook/assets/image%20%285%29.png)

You can also add an alarm when onboarding a new log source, see below:

1. In the Log Source page, click **Add Source** at the top of the page
2. Complete the onboarding experience
3. At the end of the experience, select **Yes** when asked to configure an alarm
4. Choose a time interval after which you would like to receive a "System Error" alert

Once you have alarms configured and alert destinations updated, you should be receiving health alerts!

