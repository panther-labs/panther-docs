# Alert Delivery Failure

Destination failure alerts are generated when Panther fails to deliver an alert to a destination.&#x20;

If you have rules where you have configured thresholds for delivery failure, an alert is generated when the threshold of alert delivery retries fails to successfully send to an external destination.

{% hint style="info" %}
This type of alert is classified as a "System Error." **Be sure to configure a destination to receive the "System Error" alert type to keep track of these types of errors**. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" section of the Panther UI.
{% endhint %}
