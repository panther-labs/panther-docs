# System Health Notifications

Panther's System Health notifications alert users when a part of the Panther platform is not functioning correctly. This includes the following:

* Log sources turning unhealthy as a result of a failed health check
* Logs dropping off entirely from a log source
* Alert destinations that have alerts which failed to deliver

These types of alerts are classified as "System Error" in Panther. System Errors will always have a "CRITICAL" severity level. **Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors**. Additionally, you can view these alerts in your Panther account at **Alerts & Errors > System Errors**.

The following pages provide more information on these errors and explain how you can configure system health notifications for your workflows.

{% content-ref url="system-health-notifications/configuring-system-health-notification-alarms.md" %}
[configuring-system-health-notification-alarms.md](system-health-notifications/configuring-system-health-notification-alarms.md)
{% endcontent-ref %}

{% content-ref url="system-health-notifications/log-classification-error.md" %}
[log-classification-error.md](system-health-notifications/log-classification-error.md)
{% endcontent-ref %}

{% content-ref url="system-health-notifications/alert-delivery-failure.md" %}
[alert-delivery-failure.md](system-health-notifications/alert-delivery-failure.md)
{% endcontent-ref %}

{% content-ref url="system-health-notifications/cloud-security-scanning-failure.md" %}
[cloud-security-scanning-failure.md](system-health-notifications/cloud-security-scanning-failure.md)
{% endcontent-ref %}
