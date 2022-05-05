# Import via File Upload

{% hint style="info" %}
This feature is available in version 1.27 and newer.
{% endhint %}

### Set up a Lookup Table

**Example scenario:** Let's say you want to add metadata to distinguish developer accounts from production accounts in your AWS CloudTrail logs. Here's an example where `isProduction` has been added:

![](../../.gitbook/assets/table.png)

To configure a Lookup Table, follow these steps in your Panther Console:

1. From the left sidebar, click **Enrichment > Lookups.**
2. In the upper right side of the page, click **+** to add a new Lookup Table.\
   ![](../../.gitbook/assets/add-lookup-table.png)
3. Configure the Lookup Table Basic Information:
   1. &#x20;Enter a descriptive Lookup Name.&#x20;
      * For this example, we will use `account_metadata`.
   2. Enter a Description (optional) and a Reference (optional). Description is meant for content about the table, while Reference can be used to hyperlink to an internal resource.
   3. Next to **Enabled?** toggle the setting to **Yes**. Note: This is required to import your data later in this process.&#x20;
   4. Click **Continue**.\
      ![](../../.gitbook/assets/lookup-table-basic-info.png)
4. Configure the Associated Log Types:
   * Select the **Log Type** from the dropdown.&#x20;
   * Type in the name of the **Selectors**, the foreign key fields from the log type you want enriched with your Lookup Table. (In the example screen shot below, we selected **AWS.CloudTrail** logs and typed in `accountID` and `recipientAccountID` to represent keys in the CloudTrail logs.&#x20;
     * You also can reference attributes in nested objects using [JSON path syntax](https://goessner.net/articles/JsonPath/). For example, if you wanted to reference a field in a map you could do `$.field.subfield`.
   * Click **Add Log Type** to add another if needed.\
     ![](../../.gitbook/assets/lookup-table-log-types.png)
5. Click **Continue**.
6. Configure the Table Schema. \
   _Note: If you have not already created a new schema, please see_ [_our documentation on creating schemas_](https://docs.runpanther.io/data-onboarding/custom-log-types/example-csv)_. You can also use your Lookup Table data to infer a schema. Once you have created a schema, you will be able to choose it from the dropdown on the Table Schema page while configuring a Lookup Table._\
   _Note: **CSV schemas require column headers to work with Lookup Tables**_.
   1. Select a **Schema Name** from the dropdown.
   2. Select a **Primary Key Name** from the dropdown. This should be a unique column on the table, such as `accountID`.\
      ![](../../.gitbook/assets/lookup-table-schema.png)
7. Click **Continue**.
8. Drag and drop a file or click **Select File** to choose the file of your Lookup Table data to import. The file must be in `.csv` or `.jsonl` format. \
   ![](../../.gitbook/assets/lookup-table-import.png)
9. After you successfully import a file, click **View in Data Explorer** to **** query that table data or click **Finish Setup** to go back to a list of your custom Lookup Tables.
