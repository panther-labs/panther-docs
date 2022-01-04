# Lookup Tables (BETA)

{% hint style="warning" %}
This feature is not available yet. It will be available in version 1.27 in 2022.
{% endhint %}

Lookup Tables allow you to add important context to your detections and alerts for improved investigation workflows. With Panther's Python detections-as-code, you can use Lookup Tables to decorate the alert with metadata and context, such as identity/asset information, vulnerability context and network maps.

Lookup Tables are best for data that is relatively static, such as information about AWS accounts or corporate subnets.

### Set up a Lookup Table

**Example scenario:** Let's say you want to add metadata to distinguish developer accounts and production accounts in your AWS CloudTrail logs. Here's an example:

![](<../.gitbook/assets/Screen Shot 2021-12-01 at 3.08.54 PM.png>)

Follow these steps in the Panther UI:

1. Go to the section called **Enrichment > Lookups.**
2. Click the **"+"** button on the upper right area the page to add a new Lookup Table.

![](<../.gitbook/assets/Screen Shot 2021-11-09 at 5.07.35 PM.png>)

3\. Fill in the name, and optionally a description and reference. Switch **"Yes"** to enable, which is required to import your data later in this process. Click **"Continue"** to advance.

![](<../.gitbook/assets/Screen Shot 2021-11-23 at 3.34.29 PM.png>)

4\. Select the **log type** first. Then type in the name of the **selectors**, foreign key fields from log type you want enriched with your Lookup Table. For example, you could select your **AWS.CloudTrail** logs and type in `accountID` and `recipientAccountID` that represent keys in your Cloud Trail logs. Click the **"Add Log Type"** button to add another if needed.&#x20;

Click **"Continue"** to advance.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 3.01.05 PM.png>)

5\. Tell us about the shape of your Lookup Table’s data and associate a schema to your Lookup Table. Include the primary key (unique column on the table, such as `accountID`).

![](<../.gitbook/assets/Screen Shot 2021-12-01 at 4.00.55 PM.png>)

You can **"Create a new schema"** for your table in a separate browser tab. Once you create and save the new schema, you'll be able to select it from the dropdown list and reuse it for other tables. Read [our documentation on creating schemas](https://docs.runpanther.io/data-onboarding/custom-log-types/example-csv). You can actually upload your Lookup Table data to infer the schema if want -- here is an example:

![](<../.gitbook/assets/Screen Shot 2021-12-01 at 3.39.14 PM.png>)

Click **"Continue"** to advance.

6\. Import the Lookup Table data via a `.csv` or `.jsonl` file. The maximum file size supported is 5MB.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 3.35.03 PM.png>)

7\. Once a file is successfully imported, you can query that table data by clicking "**View in Data Explorer"** or **"Finish Setup"** to go back to a list of all your custom Lookup Tables.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 4.05.39 PM.png>)

### Write a detection using Lookup Table data

You set up the Lookup Table in Panther to distinguish developer accounts and production accounts in AWS Cloudtrail logs. Next, you can write detections based on this additional context. For example, you might want receive an alert only if the login did not have MFA enabled and the AWS account is production (not a developer account). See an example of a Python rule to detect this:

```python
 from panther_base_helpers import deep_get
 def rule(event):
   is_production = deep_get(event, 'p_enrichment', 'account_metadata',
'recipientAccountId', 'isProduction')
   return not event.get('mfaEnabled') and is_production
```

The Panther rules engine will take the looked up matches and append that data to the event using the key `p_enrichment` in the following JSON structure:

```json
{ 
    'p_enrichment': {
        <name of lookup table>: { 
            <key in log that matched>: <matching row looked up>,
            ...
	    <key in log that matched>: <matching row looked up>,
	}    }
} 
```

Example:

```json
{
    'p_enrichment': {
        'account_metadata': {
            'recipientAccountId': {
                'accountID': '90123456', 
                'isProduction': 'False', 
                'email': 'dev.account@example.com', 
                }
            }
        }
}
```

#### Unit testing

For rules that use `p_enrichment` , in your unit testing you can use the "Enrich Test Data" button in the top right of the JSON code editor to populate with your Lookup Table data. This way you can test a Python function with an event that contains `p_enrichment`.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 4.47.34 PM.png>)

### Example using CIDR matching

**Example scenario:** Let's say you want to write detections that consider the traffic logs from company IP space (e.g., VPNs and hosted systems) differently from others logs originating from public IP space. You have a list of your company's allowed CIDR blocks listed in a `.csv` file (e.g. `4.5.0.0/16`).

#### Set up a Lookup Table with the CIDR list

Follow the same setup **steps 1-3 above** first.

4\. In this case you might select "AWS.VPCFlow" logs and associate the source IP (`srcAddr`) and destination (`dstAddr`) keys.

![](<../.gitbook/assets/Screen Shot 2021-11-29 at 11.45.36 AM.png>)



5\. Associate a schema for your Lookup Table: select an existing one from your list or create a new one.

{% hint style="info" %}
**NOTE:** the primary key column which will hold the CIDR blocks needs to have a `CIDR` validation applied in the schema that indicates this lookup table will do CIDR block matching on IP addresses.
{% endhint %}

6\. Import the CIDR block list via your `.csv` file.

7\. Once you've successfully uploaded your file, you can query that data by clicking **"View in Data Explorer"** or **"Finish Setup"** to go back to a list of all your custom Lookup Tables.

#### Write a detection

You'd like to receive an alert if any VPC traffic comes from a source IP address that is not part of your company's allowed CIDR blocks. Here is an example of Python rule that will send an alert in this case:

```python
def rule(event):
  if event.get('flowDirection') == 'egress': # we care about inbound
        return False
  if event.get('action') == 'REJECT': # we don't care about these either
        return False
  if deep_get(event, 'p_enrichment','Company CIDR Blocks','srcAddr'): # these are ok
        return False 
  return True # alert if NOT from an approved network range
```

