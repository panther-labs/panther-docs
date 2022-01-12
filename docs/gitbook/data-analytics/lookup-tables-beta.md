# Lookup Tables (BETA)

{% hint style="warning" %}
This feature is not available yet. It will be available in version 1.27 in 2022.
{% endhint %}

Lookup Tables allow you to add important context to your detections and alerts for improved investigation workflows. Use Lookup Tables to enhance alerts with identity/asset information, vulnerability context, network maps, and more.

Lookup Tables are best for data that is relatively static, such as information about AWS accounts or corporate subnets.

### Set up a Lookup Table

**Example scenario:** Let's say you want to add metadata to distinguish developer accounts from production accounts in your AWS CloudTrail logs. Here's an example where `isProduction` has been added:

![](../.gitbook/assets/table.png)

To configure a Lookup Table, follow these steps in the Panther UI:

1. From the left sidebar, click **Enrichment > Lookups.**
2. In the upper right side of the page, click **+** to add a new Lookup Table.

![](<../.gitbook/assets/Screen Shot 2022-01-04 at 1.46.17 PM.png>)

3\. Configure the Lookup Table Basic Information:

&#x20;     a. Enter a descriptive Lookup Name. \
&#x20;     b. Enter a Description (optional) and a Reference (optional). Description is meant for content about the table, while Reference can be used to hyperlink to an internal resource.\
&#x20;     c. Next to **Enabled?** toggle the setting to **Yes**. Note: This is required to import your data later in this process. \
&#x20;     d. Click **Continue**.

![](<../.gitbook/assets/Screen Shot 2021-11-23 at 3.34.29 PM.png>)

4\. Configure the Associated Log Types:\
&#x20;     a. Select the **Log Type** from the dropdown.  \
&#x20;     b. Type in the name of the **Selectors**, the foreign key fields from the log type you want enriched with your Lookup Table. (In the example screen shot below, we selected **AWS.CloudTrail** logs and typed in `accountID` and `recipientAccountID` to represent keys in the CloudTrail logs.)\
&#x20;     c. Click **Add Log Type** to add another if needed. \
&#x20;     d. Click **Continue**.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 3.01.05 PM.png>)

5\. Configure the Table Schema.&#x20;

Note: If you have not already created a new schema, please see [our documentation on creating schemas](https://docs.runpanther.io/data-onboarding/custom-log-types/example-csv). Once you have created a schema, you will be able to choose it from the dropdown on the Table Schema page while configuring a Lookup Table.\
\
&#x20;     a. Select a **Schema Name** from the dropdown.  \
&#x20;     b. Select a **Primary Key Name** from the dropdown. This should be a unique column on the table, such as `accountID`. \
&#x20;     c. Click **Continue**.

![](<../.gitbook/assets/Screen Shot 2021-12-01 at 4.00.55 PM.png>)

{% hint style="info" %}
**Note:** You can upload your Lookup Table data to infer the schema if you want -- here is an example:
{% endhint %}

![](<../.gitbook/assets/Screen Shot 2021-12-01 at 3.39.14 PM.png>)



6\. Drag & drop a file or click **Select File** to choose the file of your Lookup Table data to import. The file must be in `.csv` or `.jsonl` format. The maximum file size supported is 5MB.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 3.35.03 PM.png>)

7\. After you successfully import a file, click **View in Data Explorer** to **** query that table data or click **Finish Setup** to go back to a list of your custom Lookup Tables.

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 4.05.39 PM.png>)

### Write a detection using Lookup Table data

Next, you can write detections based on the additional context from your Lookup Table.&#x20;

Continuing the example mentioned above where you configure a Lookup Table to distinguish between developer and production accounts in AWS CloudTrail logs, you might want receive an alert only if the following circumstances are **both** true:

* A user logged in who did not have MFA enabled.
* The AWS account is a production (not a developer) account.&#x20;

See an example of a Python rule to detect this:

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

For rules that use `p_enrichment`, click **Enrich Test Data** in the upper right side of the JSON code editor to populate it with your Lookup Table data. This allows you to test a Python function with an event that contains `p_enrichment.`

![](<../.gitbook/assets/Screen Shot 2021-11-22 at 4.47.34 PM.png>)

### Example using CIDR matching

**Example scenario:** Let's say you want to write detections that consider the traffic logs from company IP space (e.g. VPNs and hosted systems) differently from others logs originating from public IP space. You have a list of your company's allowed CIDR blocks listed in a `.csv` file (e.g. `4.5.0.0/16`):

![](<../.gitbook/assets/CIDR (1).png>)

#### Set up a Lookup Table with the CIDR list

1. Follow the steps above under "Set up a Lookup Table" to add a new Lookup Table and configure its basic information.
2. On the Associated Log Types page, choose the Log Type and Selectors. For this example, we used `AWS.VPCFlow` logs and associated the source IP (`srcAddr`) and destination (`dstAddr`) keys.

![](../.gitbook/assets/log-types.png)

&#x20; 3\. Associate a schema for your Lookup Table: select an existing one from your list or create a new   one.

![](<../.gitbook/assets/Screen Shot 2022-01-05 at 5.06.56 PM.png>)

**Note:** The primary key column which will hold the CIDR blocks needs to have a `CIDR` validation applied in the schema that indicates this lookup table will do CIDR block matching on IP addresses. [See our log schema reference](https://docs.runpanther.io/data-onboarding/custom-log-types/reference#validation-by-string-type).

```json
# Will allow valid ip6 CIDR ranges 
# e.g. 2001:0db8:85a3:0000:0000:0000:0000:0000/64
- name: address
  type: string
  validate:
    cidr: "ipv6" 
    
# Will allow valid ipv4 IP addresses e.g. 100.100.100.100/00
- name: address
  type: string
  validate:
    cidr: "ipv4"  
```

![](<../.gitbook/assets/Screen Shot 2022-01-05 at 6.07.42 PM.png>)

4\. Drag & drop a file or click **Select File** to choose the file of your CIDR block list to import. The file must be in `.csv` or `.jsonl` format. The maximum file size supported is 5MB.&#x20;

5\. After you successfully import a file, click **View in Data Explorer** to **** query that table data or click **Finish Setup** to go back to a list of your custom Lookup Tables.

![Successful upload](<../.gitbook/assets/Screen Shot 2022-01-05 at 6.11.44 PM.png>)

![View uploaded data in Data Explorer](<../.gitbook/assets/Screen Shot 2022-01-05 at 6.12.03 PM.png>)

#### Write a detection

You might like to receive an alert if any VPC traffic comes from a source IP address that is not part of your company's allowed CIDR blocks. Here is an example of Python rule that will send an alert in this case:

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

