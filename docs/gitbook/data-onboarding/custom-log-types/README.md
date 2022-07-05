# Custom Logs

## Overview

Panther allows users to define their own log types by adding a Custom Log entry. You can generate a Custom Log from data in S3 or from a sample log.

Custom Logs are identified by a `Custom.` prefix in their name and can be used wherever a 'native' _Log Type_ is used:

* You can use a Custom Log __ when onboarding data through SQS or S3.
* You can write [Rules](../../writing-detections/rules.md) for Custom Logs.
* You can query the data in Data Explorer. Panther will create a new table for the Custom Log, once you onboard a source that uses it.
* You can query the data through Indicator Search.

## Generating and testing a data schema for a Custom Log from data in S3 (BETA)

{% hint style="info" %}
This feature is available as an invite-only beta starting with version 1.36. To request access to the feature or share any bug reports or feature requests, please contact your account team.
{% endhint %}

You can generate a schema for a custom log from live data streaming from an S3 bucket and then test that schema before publishing.&#x20;

### **View Raw Data from S3**

After onboarding your S3 bucket onto Panther, you can view raw data coming into Panther before processing. To view the raw data and kick off schema inference/testing, proceed with the following:

1. Follow the instructions to [onboard an S3 bucket onto Panther](../data-transports/s3.md) without having a schema in place.
   * Skip the step where you list schemas and prefixes in the first page of the S3 onboarding wizard.&#x20;
   * You'll have the opportunity to add schemas and prefixes after the S3 bucket is onboarded.
2. Once the S3 bucket is successfully onboarded, you'll see a call-to-action to **Attach Schemas** to the bucket in the S3 bucket's Log Source Operations Page and the Log Source Listing Page. Click the **Attach Schemas** button.\
   ![](<../../.gitbook/assets/custom Logs (1).png>)&#x20;
   * **Note:** You may need to wait up to 15 minutes for data to start streaming into Panther.&#x20;
3. On the schema inference and testing workflow page, **view** the raw data that Panther has received **at the bottom of the screen** .  \
   &#x20;![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 1.45.35 PM.png>)
   * This data is displayed from `data-archiver`, a Panther-managed S3 bucket that retains raw logs for up to 30 days for every S3 log source.
   * If you still do not see data after 15 minutes, ensure that the time picker is set to the appropriate time range that corresponds with the timestamps on the events coming into Panther.

### **Infer a schema from raw data**

Using raw live data coming from S3, you can infer schemas with just a few steps.

1. Once you see data populating in **Raw Events,** you can **filter the raw events** you'd like to infer a schema for by using the time, prefix, or string filter. Filter the raw events by going to the top of the raw events table and **setting the parameters** there.\
   &#x20;![](<../../.gitbook/assets/Screen Shot 2022-06-10 at 1.59.03 PM.png>)
2. Click **Infer Schema** to generate a schema.&#x20;
3. On the **Infer New Schema** modal that pops up, enter the following:
   * **Prefix:** An existing prefix that was set up prior to inferring the schema or a new prefix.
     * The prefix you choose will filter data from the corresponding prefix in the S3 bucket to the schema you've inferred.&#x20;
     * If you don't need to specify a specific prefix, you can use the catch-all prefix that is called `*`.
   * **Name:** The name of the schema that will map to the table in the data lake once the schema is published.&#x20;
     * The name will always start with `Custom.` and must have a capital letter after.
   * **Reference URL**: An optional field that can be used to reference documentation related to the schema.
   * **Description**: An optional field that can be used to add context to the schema's purpose and design.\
     ****![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 1.53.59 PM.png>)****
4. Click **Apply Changes**.
   * The schema will then be placed in a **Draft** mode until you're ready to publish to production after testing.
5. Review the schema and its fields by going to the **Schemas** section and clicking on the **schema name**.&#x20;
   * You'll see the name you attributed to the schema with a **draft** label after the schema is inferred.
   * Since the schema is in **Draft**, you can change, remove, or add fields as needed.

![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 5.40.29 PM.png>)

### **Test schema with raw data**

Once your schemas and prefixes are defined, you can proceed to testing the schema configuration against raw data.

1. In the **Test Schemas** section at the top of the screen, **select the date range** of data that you would like to test your schemas against.&#x20;
   * This date range is specific to testing; it is separate from the date range shown below in the **view raw data/schema inference section.**\
     ****![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 5.31.03 PM (1).png>)
2. Click **Test all Schemas** to begin test.&#x20;
   * Depending on the time range and amount of data, the test may take a few minutes to complete.
   * Once the test finishes, the results appear with the amount of matched and unmatched events.&#x20;
     * **Matched events** represent the number of events that would successfully classify against the schema configuration.&#x20;
     * **Unmatched events** represent the number of events that would unsuccessfully classify.\
       ![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 5.32.49 PM.png>)
3. Click **View Report** to see a more detailed summary of errors that caused events to unsuccessfully match.&#x20;
4. **Inspect the errors and the JSON** to decipher what caused the failures. &#x20;
5. **Navigate back to the draft schema,** make changes as needed, and test the schemas again.\
   \
   ![](<../../.gitbook/assets/Screen Shot 2022-06-09 at 5.46.54 PM.png>)

## Generating a schema for a Custom Log from sample logs

You can generate a schema for a Custom Log by uploading sample logs into the Panther UI. To get started, follow these steps:

1. Log in to your Panther account.
2. On the left sidebar, navigate to **Data > Schemas.**
3. At the top right of the page next to the search bar, click **Create New**.
4. On the New Data Schema page, enter a Schema ID, Description, and Reference URL.
   * The Description is meant for content about the table, while the Reference URL can be used to link to internal resources.
5. Scroll to the bottom of the page where you'll find the option to upload sample log files.
6. Upload a sample set of logs by dragging a file from your computer over the "Infer schema from sample logs" box or by clicking **Select file** and choosing the log file. (Reminder: Panther version 1.25 supports JSON files only, version 1.26 adds support for CSV files)\
   ![](<../../.gitbook/assets/Screen Shot 2022-01-12 at 9.45.44 AM.png>)
   * After uploading a file, Panther will display the raw logs in the UI. You can expand the log lines to view the entire raw log. Note that if you add another sample set, it will override the previously-uploaded sample.
7. Click **Infer Schema from All Logs.**
   * Panther will begin to infer a schema from the raw sample logs. Depending on the number of logs uploaded, this could take some time (e.g. 30 seconds for 100 logs).
   * Once the schema is generated, it will appear in the schema editor box above the raw logs.\
     ![](<../../.gitbook/assets/Screen Shot 2022-01-12 at 9.48.35 AM.png>)
8. To ensure the schema works properly against the sample logs you uploaded and against any changes you make to the schema, click **Validate & Test Schema.**
   * This test will validate that the syntax of your schema is correct and that the log samples you have uploaded into Panther are successfully matching against the schema. You should see the results appear below the schema editor box.
   * All successfully matched logs will appear under **Matched;** each log will display the column, field, and JSON view.\
     <img src="../../.gitbook/assets/json-matched-logs.png" alt="" data-size="original">
   * All unsuccessfully matched logs will appear under **Unmatched;** each log will display the error message and the raw log.
9. Click **Save** to publish the schema.

{% hint style="info" %}
Note that the UI will only display up to 100 logs. This doesn't mean Panther can only infer from 100 logs, Panther will infer from all logs uploaded, this is just a performance characteristic to ensure fast response time when generating a schema.
{% endhint %}

## Adding a Custom Log Manually

You can add a Custom Log by navigating to _Data_ -> _Schemas_ and clicking on the 'New Schema' button in the upper right corner.

![](<../../.gitbook/assets/image (22).png>)

Here you must enter a name for the Custom Log (ie `Custom.SampleAPI`) and write or paste your YAML _Log Schema_ definition. Use the 'Validate Syntax' button at the bottom to verify your schema contains no errors and hit 'Save'.

> Note that the 'Validate Syntax' button only checks the **syntax** of the _Log Schema_. 'Save' might still fail due to name conflicts.

If all went well, you can now navigate to _Log Analysis_ -> _Sources and add either add a new source or modify an existing one to use the new `Custom.SampleAPI` \_Log Type_. Once Panther receives events from this _Source_, it will and process the logs and store the _Log Events_ to the `custom_sampleapi` table. You can now write _Rules_ to match against these logs and query them using the _Data Explorer_.

## Editing a Custom Log Type

Panther allows limited **editing** of a Custom Log. Specifically:

* You _can_ modify the `parser` configuration to fix bugs or add new patterns.
* You _can_ add new fields to the schema.
* You _can_ edit, add, or remove all properties of existing fields except the `type`.
* You _cannot_ rename existing fields.
* You _cannot_ delete existing fields (doing so would allow renaming in two steps).
* You _cannot_ change the `type` of an existing field (this includes the element type for `array` fields).

You can edit a Custom Log by clicking on the _Edit_ action in the details page of a _Custom Log Type_. Modify the YAML and click _Update_ to submit your change. _Validate Syntax_ can check the YAML for structural compliance but the rules described above can only be checked on _Update_. The update will be rejected if the rules are not followed.

## Disabling a Custom Log

A Custom Log can be disabled **if no** _**Source**_ **is using it**.

A disabled Custom Log is removed from the listing and its tables are hidden from the Data Explorer view.

Disabling a Custom Log does not affect any data already stored in the data lake. All data is still queryable through _Data Explorer_ or _Indicator Search_. Trying to add a log with the same name at a later time, will result in failure due to the name **conflict**.

You can disable a Custom Log by clicking on the _Enable_ toggle in the details page of a _Custom Log Type_. The action will succeed only if the Custom Log is not currently in use by any source.

## Testing a Panther-managed Log

{% hint style="info" %}
This feature is available in versions 1.26 and above.
{% endhint %}

Need to validate that a Panther-managed schema will work against your logs? You can test sample logs against the Panther-managed schema similarly to testing logs against a custom schema/log type (as described above). Follow the steps below:

* Visit the **Schema** page under the **Data** tab.
* Click on a schema labeled as **Panther-managed.**
* Once in the schema details page, scroll to the bottom of the page where you'll be able to upload logs.

![](<../../.gitbook/assets/Screen Shot 2021-12-02 at 10.05.48 PM (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png>)

&#x20;

## Writing a log schema for JSON logs

> You can make use of our `pantherlog` [CLI tool](./#generating-a-schema-from-json-samples) to help you generate your Log Schema

To parse log files where each line is JSON you have to define a _Log Schema_ that describes the structure of each log entry.

In the example schemas below, the first tab displays the JSON log structure and the second tab shows the Log Schema.&#x20;

{% hint style="info" %}
**Note**: Please leverage the **Minified JSON Log Example** when using the `pantherlog` tool or generating a schema within the Panther Console.
{% endhint %}

{% tabs %}
{% tab title="JSON Log Example" %}
```json
{
  "method": "GET",
  "path": "/-/metrics",
  "format": "html",
  "controller": "MetricsController",
  "action": "index",
  "status": 200,
  "params": [],
  "remote_ip": "1.1.1.1",
  "user_id": null,
  "username": null,
  "ua": null,
  "queue_duration_s": null,
  "correlation_id": "c01ce2c1-d9e3-4e69-bfa3-b27e50af0268",
  "cpu_s": 0.05,
  "db_duration_s": 0,
  "view_duration_s": 0.00039,
  "duration_s": 0.0459,
  "tag": "test",
  "time": "2019-11-14T13:12:46.156Z"
}
```
{% endtab %}

{% tab title="Log Schema Example" %}
```yaml
version: 0
fields:
- name: time
  description: Event timestamp
  required: true
  type: timestamp
  timeFormat: rfc3339
  isEventTime: true
- name: method
  description: The HTTP method used for the request
  type: string
- name: path
  description: The path used for the request
  type: string
- name: remote_ip
  description: The remote IP address the request was made from
  type: string
  indicators: [ ip ] # the value will be appended to `p_any_ip_addresses` if it's a valid ip address
- name: duration_s
  description: The number of seconds the request took to complete
  type: float
- name: format
  description: Response format
  type: string
- name: user_id
  description: The id of the user that made the request
  type: string
- name: params
  type: array
  element:
    type: object
    fields:
    - name: key
      description: The name of a Query parameter
      type: string
    - name: value
      description: The value of a Query parameter
      type: string
- name: tag
  description: Tag for the request
  type: string
- name: ua
  description: UserAgent header
  type: string
```
{% endtab %}

{% tab title="Minified JSON Log Example" %}
{"method":"GET","path":"/-/metrics","format":"html","controller":"MetricsController","action":"index","status":200,"params":\[],"remote\_ip":"1.1.1.1","user\_id":null,"username":null,"ua":null,"queue\_duration\_s":null,"correlation\_id":"c01ce2c1-d9e3-4e69-bfa3-b27e50af0268","cpu\_s":0.05,"db\_duration\_s":0,"view\_duration\_s":0.00039,"duration\_s":0.0459,"tag":"test","time":"2019-11-14T13:12:46.156Z"}
{% endtab %}
{% endtabs %}

You can edit the YAML specifications directly in the Panther Console or they can be [prepared offline in your editor/IDE of choice](reference.md#using-json-schema-in-an-ide). For more information on the structure and fields in a _Log Schema_, see the [Log Schema Reference](reference.md).

## Writing a log schema for text logs

Panther handles logs that are not structured as JSON by using a 'parser' that translates each log line into key/value pairs and feeds it as JSON to the rest of the pipeline. You can define a text parser using the `parser` field of the _Log Schema_. Panther provides the following parsers for non-JSON formatted logs:

| Name                              | Description                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| [fastmatch](example-fastmatch.md) | Match each line of text against one or more simple patterns                                                               |
| [regex](example-regex.md)         | Use regular expression patterns to handle more complex matching such as conditional fields, case-insensitive matching etc |
| [csv](example-csv.md)             | Treat log files as CSV mapping colunm names to field names                                                                |

## Pantherlog CLI

Panther provides a simple CLI tool to help work with Custom Logs feature. The tool is called `pantherlog` and an executable for each platform is provided with the release. The executables can be downloaded from the `panther-community` S3 bucket, see more details on the operations help [page](../../help/operations/#tools).

### Generating a schema from JSON samples

You can use the tool to generate a schema file out of sample files in [new-line delimited JSON](http://ndjson.org/) format. The tool will scan the provided logs and print the inferred schema to `stdout`.

For example, to infer the schema of logs `sample_logs.jsonl` and output to `schema.yml`, use:

```
$ ./pantherlog infer sample_logs.jsonl > schema.yml
```

Note that YAML keys and values are case sensitive.

> **WARNING**: The tool has the following limitations:
>
> *   It will identify a string as a timestamp, only if the string is in RFC3339 format. Make sure to review the schema after it is generated by the tool
>
>     and identify fields that should be of type `timestamp` instead.
> *   It will not mark any timestamp field as `isEventTime:true`. Make sure to select the appropriate `timestamp` field and mark it as `isEventTime:true`.
>
>     For more information regarding `isEventTime:true` see [timestamp](reference.md#timestamps).
> * It is able to infer only 3 types of indicators: `ip`, `aws_arn`, `url`. Make sure to review the fields and add more indicators as appropriate.
>
> Make sure to review the schema generated and edit it appropriately before deploying to your production environment!

{% embed url="https://www.loom.com/share/5479e9c46f914c03bff4da0464f30956" %}
Pantherlog CLI example
{% endembed %}

### Trying out a schema

You can use the tool to validate a schema file and use it to parse log files. Note that the events in the log files need to be separated by new line. Processed logs are writen to `stdout` and errors to `stderr`.

For example, to parse logs in `sample_logs.jsonl` with the log schema in `schema.yml`, use:

```
$ ./pantherlog parse --path schema.yml --schemas Schema.Name sample_logs.jsonl
```

The tool can also accept input via `stdin` so it can be used in a pipeline:

```
$ cat sample_logs.jsonl | ./pantherlog parse --path schema.yml
```

### Running tests for a schema

You can use the tool to run unit tests. You can define unit tests for your Custom Schema in YAML files. To run tests defined in a `schema_tests.yml` file for a custom schema defined in `schema.yml` use:

```
$ ./pantherlog test schema.yml schema_tests.yml
```

The first argument is a file or directory containing schema YAML files. The rest of the arguments are test files to run. If you don't specify any test files arguments, and the first argument is a directory, the tool will look for tests in YAML files with a `_tests.yml` suffix.

Below is an example of a test using the [previous JSON log sample](./#writing-a-log-schema-for-json-logs), testing against our inferred schema with the added flag `isEventTime: true` under the `time` field to ensure the correct timestamp:

{% tabs %}
{% tab title="schema_tests.yml" %}
```json
# Make sure to use camelCase when naming the schema or log type
name: Custom Log Test Name
logType: Custom.SampleLog.V1
input: |
  {
    "method": "GET",
    "path": "/-/metrics",
    "format": "html",
    "controller": "MetricsController",
    "action": "index",
    "status": 200,
    "params": [],
    "remote_ip": "1.1.1.1",
    "user_id": null,
    "username": null,
    "ua": null,
    "queue_duration_s": null,
    "correlation_id": "c01ce2c1-d9e3-4e69-bfa3-b27e50af0268",
    "cpu_s": 0.05,
    "db_duration_s": 0,
    "view_duration_s": 0.00039,
    "duration_s": 0.0459,
    "tag": "test",
    "time": "2019-11-14T13:12:46.156Z"
  }

result: |
  {
    "action": "index",
    "controller": "MetricsController",
    "correlation_id": "c01ce2c1-d9e3-4e69-bfa3-b27e50af0268",
    "cpu_s": 0.05,
    "db_duration_s": 0,
    "duration_s": 0.0459,
    "format": "html",
    "method": "GET",
    "path": "/-/metrics",
    "remote_ip": "1.1.1.1",
    "status": 200,
    "tag": "test",
    "time": "2019-11-14T13:12:46.156Z",
    "view_duration_s": 0.00039,
    "p_log_type": "Custom.SampleLog.V1",
    "p_row_id": "acde48001122a480ca9eda991001",
    "p_event_time": "2019-11-14T13:12:46.156Z",
    "p_parse_time": "2022-04-04T16:12:41.059224Z",
    "p_any_ip_addresses": [
        "1.1.1.1"
    ]
  }
```
{% endtab %}

{% tab title="schema.yml" %}
```yaml
version: 0
schema: Custom.SampleLog.V1
fields:
- name: action
  required: true
  type: string
- name: controller
  required: true
  type: string
- name: correlation_id
  required: true
  type: string
- name: cpu_s
  required: true
  type: float
- name: db_duration_s
  required: true
  type: bigint
- name: duration_s
  required: true
  type: float
- name: format
  required: true
  type: string
- name: method
  required: true
  type: string
- name: path
  required: true
  type: string
- name: remote_ip
  required: true
  type: string
  indicators:
  - ip
- name: status
  required: true
  type: bigint
- name: tag
  required: false
  type: string
- name: time
  required: true
  type: timestamp
  timeFormat: rfc3339
  isEventTime: true
- name: view_duration_s
  required: true
  type: float
```
{% endtab %}
{% endtabs %}

## Uploading log schemas with the Panther Analysis Tool

If you choose to maintain your log schemas outside of Panther, for example in order to keep them under version control and review changes before updating, you can upload the YAML files programmatically with the [Panther Analysis Tool](../../writing-detections/panther-analysis-tool.md).

The uploader command receives a base path as an argument and then proceeds to recursively discover all files with extensions `.yml` and `.yaml`.

{% hint style="warning" %}
It is recommended to keep schema files separately from other unrelated files, otherwise you may notice several unrelated errors for attempting to upload invalid schema files.
{% endhint %}

```
panther_analysis_tool update-custom-schemas --path ./schemas
```

The uploader will check if an existing schema exists and proceed with the update or create a new one if no matching schema name is found.

{% hint style="danger" %}
The `schema`field must always be defined in the YAML file and be consistent with the existing schema name for an update to succeed. For an example see [here](https://github.com/panther-labs/panther\_analysis\_tool/blob/42084578fb128f79d191e17f8259c12990f94801/tests/fixtures/custom-schemas/valid/schema-1.yml#L1).
{% endhint %}

{% hint style="info" %}
The uploaded files are validated with the same criteria as Web UI updates.
{% endhint %}

## &#x20;
