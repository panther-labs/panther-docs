# Querying and Writing Detections for Panther Audit Logs

A typical strategy for using audit logs is to ingest the logs into a SIEM or similar data warehousing tool for future use. Given that Panther itself is such a tool, we set up ingestion of audit logs into Panther by default. You can interact with Panther audit logs in all the ways you would expect - they can be used in detections, data lake queries, and more.&#x20;

## Querying the Data Lake for Audit Logs

Audit logs can be found in the data lake under `panther_logs.panther_audit`. The following query shows all audited events that took place within the last day:

```
SELECT * FROM panther_logs.panther_audit WHERE p_occurs_since('1 day');
```

The result of this query would include several audit logs, an example of which can be seen below

```
{
	"XForwardedFor": [
		"72.72.72.72",
		"130.172.130.172"
	],
	"actionDescription": "Lists the details of all available data lake databases",
	"actionName": "LIST_DATA_LAKE_DATABASES",
	"actionParams": {},
	"actionResult": "SUCCEEDED",
	"actor": {
		"attributes": {
			"email": "foo.user@acmecorp.io",
			"emailVerified": false,
			"roleId": ""
		},
		"id": "AcmecorpSSO_foo.user@acmecorp.io",
		"name": "foo.user@acmecorp.io",
		"type": "USER"
	},
	"errors": null,
	"p_any_ip_addresses": [
		"72.72.72.72",
		"130.172.130.172"
	],
	"p_any_trace_ids": [
		"AcmecorpSSO_foo.user@acmecorp.io"
	],
	"p_any_usernames": [
		"foo.user@acmecorp.io"
	],
	"p_event_time": "2022-04-22 15:39:55.358",
	"p_log_type": "Panther.Audit",
	"p_parse_time": "2022-04-22 15:41:36.276",
	"p_row_id": "asdfdjklasdfjklasdfjlk",
	"p_source_id": "abc12345-ab12-cd12-ef12-abc1234567890",
	"p_source_label": "panther-audit-logs-us-east-1",
	"pantherVersion": "1.34.0",
	"sourceIP": "72.72.72.72",
	"timestamp": "2022-04-22 15:39:55.358",
	"userAgent": ""
}
```

## Writing Detections

Audit logs can be leveraged to write powerful detections for generating alerts when an unusual or important action has been taken within Panther.

In this article, we'll cover how to write a detection that alerts when a detection has been deleted.

### How to create a new Detection for audit logs

1. Log in to your Panther Console.
2. In the left sidebar, click **Detections**. On the Detections page, click **Create New**.\
   ![](<../../.gitbook/assets/Screen Shot 2022-04-18 at 12.50.13 PM (1).png>)
3. In your new Detection, click the "Rule Settings" tab then fill in the form fields.&#x20;
   * For **Log Types**, select `Panther.Audit`.\
     ![](<../../.gitbook/assets/Screen Shot 2022-04-18 at 12.50.55 PM.png>)
4. Click the "Functions & Tests" tab. Enter Python code for your rule.
   *   For example, you can enter the following code to be alerted when a Detection is deleted:&#x20;

       ```
       def rule(event):    
           return event.get('actionName') == 'DELETE_DETECTION'
       def title(event):
           return 'Detection deleted!'
       ```
5. Click **{} Create Test**.
   * Below the text editor where you defined your Detection, another text editor appears where you can write a test.&#x20;

See the [Test the Detection](querying-and-writing-detections-for-panther-audit-logs.md#create-a-test-for-the-detection) section of this documentation for next steps on generating the JSON to enter into the text editor for testing this Detection.

#### Descriptive alert titles

In the example above, we used a simple alert title:

```
def title(event):
    return 'Detection deleted!'
```

You can construct a more descriptive alert title using the values found in the `actionParams` field within the audit log:&#x20;

```
def title(event):
    deleted_detection_id = event.get('actionParams').get('input').get('detections')[0].get('id')
    actor_type = event.get('actor').get('type').lower()
    actor_readable_id = event.get('actor').get('name') if event.get('actor').get('name') else event.get('actor').get('id')
    return f"Detection '{deleted_detection_id}' deleted by {actor_type} {actor_readable_id}!"
```

See the [log schema](./#schema) for more information on the audit log fields.

Note: the `actionParams` field is different for each audited action. To understand what information is present in this field for a given action, [query the data lake for audit logs for the given action](querying-and-writing-detections-for-panther-audit-logs.md#querying-the-data-lake-for-audit-logs) and use the results to inform how you write detections for that action.

### Test the Detection

In the [How to create a new Detection for audit logs](querying-and-writing-detections-for-panther-audit-logs.md#how-to-create-a-new-detection-for-audit-logs) section above, you defined your Detection and clicked **{} Create Test** to begin the process of testing.&#x20;

In these steps, you will generate test data against the action you wrote a Detection for. In the example, we defined a Detection to check for the action of deleting a Detection in the Panther Console.

1. In a separate browser tab, open your Panther Console. Create test data relating to the action you wrote a Detection for.&#x20;
   * In the example above, we defined a Detection to check for the action of deleting a Detection in the Panther Console. For this example, you would follow these steps:
     1. Navigate to **Detections**.
     2. Create a test Detection.&#x20;
     3. After successfully creating the Detection, delete it.
2. In the left sidebar, click **Data > Data Explorer**.
3. Execute a query to find the audit log for the action you are testing against.
   *   Based on our example, we will use the following query to check for the recently deleted Detection:

       ```
       SELECT * FROM panther_logs.panther_audit WHERE actionName = 'DELETE_DETECTION'
       ORDER BY timestamp DESC
       LIMIT 1;
       ```
   * If no results are returned, wait a few minutes and retry.
4. Copy the JSON object in Data Explorer representing this log. Navigate back to the audit log Detection you defined, then paste the JSON object into the Test text editor.
5. Click **Run Test**. Verify that the Detections runs as expected and the alert title appears as expected. \
   ![](<../../.gitbook/assets/Screen Shot 2022-04-18 at 3.55.16 PM.png>)
6. Click **Save** in the upper right side of the page.

You have now written a detection using Panther audit logs!
