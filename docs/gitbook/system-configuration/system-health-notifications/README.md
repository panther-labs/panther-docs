# System Health Notifications

Panther's System Health notifications alert users when a part of the Panther platform is not functioning correctly. This includes the following:

* Log sources turning unhealthy as a result of a failed health check
* Logs dropping off entirely from a log source
* Alert destinations that have alerts which failed to deliver

These types of alerts are classified as "System Error" in Panther. **Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors**. Additionally, you can view these alerts in your Panther account at **Alerts & Errors > System Errors**.

The following pages provide more information on these errors and explain how you can configure system health notifications for your workflows.

{% content-ref url="configuring-system-health-notification-alarms.md" %}
[configuring-system-health-notification-alarms.md](configuring-system-health-notification-alarms.md)
{% endcontent-ref %}

{% content-ref url="log-classification-error.md" %}
[log-classification-error.md](log-classification-error.md)
{% endcontent-ref %}

{% content-ref url="alert-delivery-failure.md" %}
[alert-delivery-failure.md](alert-delivery-failure.md)
{% endcontent-ref %}

{% content-ref url="cloud-security-scanning-failure.md" %}
[cloud-security-scanning-failure.md](cloud-security-scanning-failure.md)
{% endcontent-ref %}





This section covers how users can configure these system health notifications to fit their workflows.

### Log Source Health Notifications

Panther performs health checks on log sources to ensure that Panther is correctly linked to the source, has the right credentials, and is receiving data from the source consistently. When those health checks fail for a given log source, Panther will send a "System Error" alert to the **Alerts** page and to an alert destination.

To ensure proper configuration to receive alerts for System Health errors:

* Configure an alert destination that is receiving the "System Error" alert type.
* Configure alarms for log sources that will trigger an alert when data is no longer being received.

#### Configure Delivery of System Health Error Alerts

By default, Panther will send these alerts to the **Alerts** page in the Panther UI. To ensure these alerts are sent to a custom alert destination, follow the steps below:

1. Go to **Integrations** > **Alert Destinations**
2. Choose an existing Alert Destination or add a new Alert Destination
3. Add **System Errors** to the **Alert Destination Configuration** form (as seen below)

![](<../../../../.gitbook/assets/image (1).png>)

#### Configure Alarms for Log Drop-offs for your Log Sources

Panther allows users to set up "alarms" for individual log sources that will trigger an alert if data is not received over a specific time interval. This can be useful for log sources that have been incorrectly linked to Panther or are experiencing issues outside of Panther.&#x20;

Alarms can be setup in two ways, either by adding an alarm to an existing log source or adding an alarm to a new log source.

#### Setup Alarm for Existing Log Source

1. &#x20;Go to **Integrations** > **Log Sources**
2. Select the log source to configure an alarm by clicking on the three-dotted **more options** icon on the far right of the tile
3. Select **Alarm Configuration**
4. Turn the alarm off or on and view which destinations a "System Error" alert will be sent to

![](<../../../../.gitbook/assets/image (9).png>)

#### Setup Alarm for New Log Source

You can also add an alarm when onboarding a new log source:

1. Go to **Integrations > Log Sources**.
2. Click the **+** icon **** at the top of the page to add a new source.
3. Complete the onboarding workflow.
4. At the end of the log source onboarding, select **Yes** when asked to configure an alarm
5. Turn the alarm off or on and view which destinations a "System Error" alert will be sent to

Once you have alarms configured and alert destinations updated, you will receive health notifications if conditions are met to trigger them.

### Log Classification Error Notifications

Log misclassification alerts are generated when logs hit a parsing error and fail to classify when sent to Panther. When this happens, the following actions take place **by default:**

* Misclassified logs are sent to the data lake and are searchable in a table called `classification_failures` in the `panther_monitor` database.
* An alert is generated immediately after the first log fails to classify. The alert will display all log lines that are failing to classify.&#x20;

This type of alert is classified as a "System Error," similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" section of the Panther UI.

The alert details page will highlight which log lines are failing to parse correctly -- which should help inform which lines in the log type's respective schemas need to be corrected or added.

### S3 GetObject Error Notifications

S3 GetObject error alerts are generated when Panther fails to fetch S3 objects. When this happens, the following actions take place by default:

* Panther stores the S3 objects in the data lake which can be queried through the Data Explorer in a table titled `panther_monitor.data_audit`
* An alert will fire off if Panther fails to fetch any S3 object in the last 24hours. The alert will display the specific S3 objects that are failing.

This type of alert is classified as a "System Error", similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" tab in the Panther UI.&#x20;

### Destination Failure Notifications

Destination failure alerts are generated when Panther fails to deliver an alert to a destination. When this happens, the following actions take place by default:

* An alert is fired whenever a threshold of alert delivery retries fails to successfully send to an external destination.

This type of alert is classified as a "System Error", similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" tab in the Panther UI.&#x20;

### Cloud Security Scanning Failure Notifications

Cloud security scanning failure alerts are generated when Panther fails to scan a cloud resource. When this happens, the following action takes place by default:

* An alert is fired when a cloud scan fails because of an access denied error.

This error occurs when permissions are not configured properly to allow scanning to occur. This is most commonly caused by one of three scenarios:

* Our scanning role (`PantherAuditRole`) is not configured with sufficient permissions. This is an extremely rare case as the permissions of this role rarely change. This can be resolved by updating the `PantherAuditRole`
* An AWS organizations Service Control Policy (SCP) is preventing our scanning role from carrying out scans. Commonly this occurs with SCP's designed lock down certain regions or services. This can be resolved by either modifying the SCP to add an exception for our scanning role, or by modifying the Cloud Security integration to exclude certain regions or resource types.
* An AWS resource base policy is preventing our scanning role from carrying out scans. In AWS permissions are bidirectional, our role be granted permission to access a resource but the resource itself may not be granting permission to be accessed by our role. This can be resolved by either modifying the resource based policy to add an exception for our scanning role, or by modifying the Cloud Security integration to exclude certain resources or resource types.

The alert will indicate which resource scanning failed on, and the AWS error that caused the scanning to fail.

![](../../.gitbook/assets/scanning\_error.png)

This can usually be used to pinpoint the exact permissions issue. In the above example we can see `no resource-based policy allows the kms:ListResourcetags action`. This indicates to us that the issue is related to a resource based policy.

This type of alert is classified as a "System Error", similar to all other system health alerts referenced on this page. Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" tab in the Panther UI.&#x20;
