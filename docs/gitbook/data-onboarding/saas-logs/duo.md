# Duo

{% hint style="info" %}
This feature is only available in Panther Enterprise
{% endhint %}

Panther can collect the following Duo logs via the [Duo API](https://duo.com/docs/adminapi#logs):

* [Authentication Logs \(v2\)](https://duo.com/docs/adminapi#authentication-logs)
* [Administrator Logs](https://duo.com/docs/adminapi#administrator-logs)
* [Telephony Logs](https://duo.com/docs/adminapi#telephony-logs)
* [Offline Enrollment Logs](https://duo.com/docs/adminapi#offline-enrollment-logs%20%20%20%20%20)

In order for Panther to access the Duo API you need to create a new 'Duo App' and provide the app credentials to Panther.

## Create a Duo application

1. Follow the instructions [here](https://duo.com/docs/adminapi#first-steps) to create a new Duo application.

   Note that only administrators with the Owner role can create or modify an Admin API application in the Duo Admin Panel.

2. Make sure to grant the application **Grant read log** permissions.

## Create a new Duo Source in Panther

1. Login to your Panther deployment
2. Go to **Log analysis** &gt; **Sources** from the sidebar menu
3. Click **Add Source**
4. Select **Duo** from the list of available types
5. Add a friendly name for the source
6. Select the type of logs you want to monitor
7. Click on **Continue Setup**
8. Fill in the fields below:
   1. **Integration Key**: The integration key of the Duo app
   2. **Secret Key**: The secret key of the Duo app
   3. **API Hostname**: The API hostname of the Duo app
9. Click on **Continue Setup**

You are done! You can now start writing detections and exploring your Duo data.

