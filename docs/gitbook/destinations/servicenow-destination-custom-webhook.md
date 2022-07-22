---
description: Set up ServiceNow alerts using Panther's custom webhook option
---

# ServiceNow Destination (Custom Webhook)

## Overview&#x20;

With a simple Scripted Rest API configuration in the ServiceNow console, alerts fired from Panther can be mapped directly to new incidents. Leveraging Python detections and [our auxiliary functions](https://docs.panther.com/writing-detections/detection-auxiliary-functions) allows customers to dynamically create alerts with custom webhooks.

## How to configure ServiceNow to create tickets from Panther alerts

### Prerequisites

ServiceNow permissions to create a Scripted Rest API require a user with the **web\_service\_admin** role. The below ServiceNow references provide context on setting up these initial Scripted Rest API requirements:

* [How to Integrate Webhooks Into ServiceNow - Blog - ServiceNow Community](https://community.servicenow.com/community?id=community\_blog\&sys\_id=886d2a29dbd0dbc01dcaf3231f9619b0)
* [Scripted Rest APIs - ServiceNow Documentation](https://docs.servicenow.com/en-US/bundle/sandiego-application-development/page/integrate/custom-web-services/task/t\_CreateAScriptedRESTService.html)

### Step 1: Create a Scripted Rest API in ServiceNow

1. In the ServiceNow console, click the **All** tab in the upper left-hand corner.&#x20;
2. Expand the **System Web Services** and **Scripted Web Services** navigations, then click on **Scripted REST APIs.**\
   ****<img src="../.gitbook/assets/image (4).png" alt="" data-size="original">``
3. Click **New** in the upper right-hand corner.&#x20;
4. Select a Name and an ID, for example, `Panther Incident Creation` and `panther_incident_creation`, respectively.\
   \
   ![](<../.gitbook/assets/image (12) (2) (1) (1).png>)
5. Click **Submit**.
6. On the Scripted Rest API's page, search for the name you just created. Click the **hyperlinked name.**
7. Near the bottom of the page, click the **Resources tab.** Click **** the **New** button in the right-hand corner.
8. Fill out the Scripted REST Resource Alert page:
   * **Name:** Enter a descriptive name, e.g., `Panther_Alert`.  \
     ![](<../.gitbook/assets/Scripted REST ss name field.png>)
   * **HTTP method:** Select POST.\
     ![](<../.gitbook/assets/Scripted REST ss HTTP post.png>)
   *   **Script:** Paste in the schema code below into the **Script** field:

       *   ```
           (function process(/*RESTAPIRequest*/ request, /*RESTAPIResponse*/ response) {

           	// prep the different fields 
           	var data = request.body.data;
           	var title = data.title;
           	var alert = JSON.stringify(data);
           	var alertContext = JSON.stringify(data.alertContext);
           	var severity = data.severity;
           	var link = data.link;
           	var runbook = data.runbook;
           	var type = data.type;
           	var alertId = data.alertId;
           	
           	var grIncident = new GlideRecord('incident');

           	grIncident.initialize();
           	
           	grIncident.setValue('short_description', title);
           	grIncident.setValue('description', alert );
           	grIncident.setValue('category', type);
           	grIncident.setValue('subcategory', alertId);
           	
           	//Map urgency to Panther severity
           	if (severity == "CRITICAL" || severity == "HIGH") {
           		grIncident.setValue('urgency','1');
           		grIncident.setValue('impact','1');
           	} else if (severity == "LOW" || severity == "MEDIUM") {
           		grIncident.setValue('urgency','2');
           	} else {
           		grIncident.setValue('urgency','3');
           	}
           	
           	//grIncident.insert();
           	var recResponse = grIncident.insert(handleResponse);

           	function handleResponse(recResponse, answer) {
           	// Answer will be the sys_id of the created record or null
           	alert('Newly created sys_id is - ' + answer + ' exists');
           	}

           	var url = gs.getProperty('glide.servlet.uri');

                   //building the response of the API, this example returns the incident ID that got created above.
           	var body = {};
           	body.sys_id = recResponse;
           	body.link = url + "task.do?sys_id=" + recResponse;
           	response.setBody(body);
           	
           	//example test event from Panther when creating and testing destination integration
           	//{"id":"Test.Alert","createdAt":"2022-04-26T03:17:32.099054303Z","severity":"INFO","type":"RULE","link":"https://domain.runpanther.net","title":"This is a Test Alert","name":"Test Alert","alertId":"Test.Alert","alertContext":{},"description":"This is a Test Alert","runbook":"Stuck? Check out our docs: https://docs.runpanther.io","tags":["test"],"version":"abcdefg"}

           })(request, response);
           ```

           ![](<../.gitbook/assets/Scripted REST ss Script.png>)



       * Under the **Security tab,** uncheck the **Requires authentication** box.\
         ![](<../.gitbook/assets/servicenow authentication.png>)
9. Click **Submit**.

{% hint style="warning" %}
The schema provided above maps the alert payload from Panther to the relevant fields in the ServiceNow ticket. The [ServiceNow blog](https://community.servicenow.com/community?id=community\_blog\&sys\_id=886d2a29dbd0dbc01dcaf3231f9619b0) also provides a different example of receiving the POST payload. Each customer environment is different – select what works best for how the Alert payload is handled and parsed into your ServiceNow tickets.
{% endhint %}

### Step 2: Create a Custom Webhook integration in Panther

1. In the Panther Console, navigate to **Integrations > Alert Destinations > Create New > Custom Webhook**.
2. On the **Configure Your Webhook Destination** page, fill in the following:
   * Choose a memorable **Display Name** and enter your **Custom Webhook URL**.
     * Your webhook URL is in the following format: **`https://yourdomain.service-now.com/<base_api_path>`**&#x20;
     * This domain is shown in the `Rest API` section of your ServiceNow console.
   * Choose the **Severity** level and the default **Alert Types** you want from ServiceNow.&#x20;
   * Click **Add Destination.**\
     ****![](<../.gitbook/assets/custom webook destinations.png>)
3. Click the **Send Test Alert** button to make sure everything works correctly.
   * A test event should now exist in your ServiceNow Incidents table.\
     \
     &#x20;![](<../.gitbook/assets/image (5) (1) (2) (1) (1) (1).png>)
4. Click **Finish Setup.**

### Example

Click the **Test Alert** button to generate an alert and send to ServiceNow; the payload of the alert is seen below:

```json
{"id":"Test.Alert","createdAt":"2022-04-26T03:17:32.099054303Z","severity":"INFO","type":"RULE","link":"https://domain.runpanther.net","title":"This is a Test Alert","name":"Test Alert","alertId":"Test.Alert","alertContext":{},"description":"This is a Test Alert","runbook":"Stuck? Check out our docs: https://docs.runpanther.io","tags":["test"],"version":"1"}
```

Once the alert is received by ServiceNow, an incident should be created in ServiceNow Incident table:

![](<../.gitbook/assets/image (6) (1).png>)
