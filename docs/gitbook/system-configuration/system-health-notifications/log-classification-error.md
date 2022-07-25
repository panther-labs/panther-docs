# Log Processing Errors

### Log misclassification alerts

Log misclassification alerts generate when logs hit a parsing error and fail to classify when sent to Panther. When this happens, the following actions take place **by default:**

* Misclassified logs are sent to the data lake and are searchable in a table called `classification_failures` in the `panther_monitor` database.
* An alert is generated immediately after the first log fails to classify. The alert will display all log lines that are failing to classify.&#x20;

{% hint style="info" %}
This type of alert is classified as a "System Error." **Configure a destination to receive the "System Error" alert type to keep track of these types of errors**. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" section of the Panther Console.
{% endhint %}

The alert details page highlights the log lines that fail to parse correctly, helping to inform which lines in the log type's respective schemas need to be corrected or added.&#x20;

The alert includes a link to the respective log source's Log Source Ops page where you can view the rate at which data is misclassifying within the Health __ tab.

![](<../../.gitbook/assets/image (44).png>)

### S3 GetObject Error Notifications

S3 GetObject error alerts generate when Panther fails to fetch S3 objects. When this happens, the following actions take place by default:

* Panther stores the S3 objects in the data lake which can be queried through the Data Explorer in a table titled `panther_monitor.data_audit`.
* An alert is generated if Panther fails to fetch any S3 object in the last 24 hours. The alert displays the specific S3 objects that are failing.

{% hint style="info" %}
This type of alert is classified as a "System Error." **Configure a destination to receive the "System Error" alert type to keep track of these types of errors**. Additionally, you can view this alert in the "System Errors" sub-tab of the "Alerts & Errors" section of the Panther Console.
{% endhint %}
