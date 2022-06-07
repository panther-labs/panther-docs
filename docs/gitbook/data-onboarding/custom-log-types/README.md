# Custom Log Types

## Overview

Panther allows users to define their own log types by adding a _Custom Log Type_ entry. _Custom Log Types_ are identified by a `Custom.` prefix in their name and can be used wherever a 'native' _Log Type_ is used:

* You can use a _Custom Log Type_ when onboarding data through SQS or S3
* You can write [Rules](../../writing-detections/rules.md) for these _Log Types_.
* You can query the data in Data Explorer. Panther will create a new table for the _Custom Log Type_, once you onboard a source that uses it.
* You can query the data through Indicator Search

## Generating a Schema for a Custom Log Type from Sample Logs

{% hint style="info" %}
This feature is available in versions 1.25 and above (1.25 only supports uploading JSON files, CSV files are supported in 1.26 and above).
{% endhint %}

You can generate a schema for a custom log type by uploading sample logs into the Panther UI. To get started, follow these steps:

1. Log in to your Panther account.
2. On the left sidebar, navigate to **Data > Schemas.**
3. At the top right of the page next to the search bar, click **+**.
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

## Adding a Custom Log Type Manually

You can add a _Custom Log Type_ by navigating to _Data_ -> _Schemas_ and clicking on the 'New Schema' button in the upper right corner.

![](<../../.gitbook/assets/image (22).png>)

Here you must enter a name for the _Custom Log Type_ (ie `Custom.SampleAPI`) and write or paste your YAML _Log Schema_ definition. Use the 'Validate Syntax' button at the bottom to verify your schema contains no errors and hit 'Save'.

> Note that the 'Validate Syntax' button only checks the **syntax** of the _Log Schema_. 'Save' might still fail due to name conflicts.

If all went well, you can now navigate to _Log Analysis_ -> _Sources and add either add a new source or modify an existing one to use the new `Custom.SampleAPI` \_Log Type_. Once Panther receives events from this _Source_, it will and process the logs and store the _Log Events_ to the `custom_sampleapi` table. You can now write _Rules_ to match against these logs and query them using the _Data Explorer_.

## Editing a Custom Log Type

Panther allows limited **editing** of _Custom Log Types_. Specifically:

* You _can_ modify the `parser` configuration to fix bugs or add new patterns.
* You _can_ add new fields to the schema.
* You _can_ edit, add, or remove all properties of existing fields except the `type`.
* You _cannot_ rename existing fields.
* You _cannot_ delete existing fields (doing so would allow renaming in two steps).
* You _cannot_ change the `type` of an existing field (this includes the element type for `array` fields).

You can edit a _Custom Log Type_ by clicking on the _Edit_ action in the details page of a _Custom Log Type_. Modify the YAML and click _Update_ to submit your change. _Validate Syntax_ can check the YAML for structural compliance but the rules described above can only be checked on _Update_. The update will be rejected if the rules are not followed.

## Disabling a Custom Log Type

A _Custom Log Type_ can be disabled **if no** _**Source**_ **is using it**.

A disabled _Custom Log Type_ is removed from the listing and its tables are hidden from the Data Explorer view.

Disabling a _Custom Log Type_ does not affect any data already stored in the data lake. All data is still queryable through _Data Explorer_ or _Indicator Search_. Trying to add a log with the same name at a later time, will result in failure due to the name **conflict**.

You can disable a _Custom Log Type_ by clicking on the _Enable_ toggle in the details page of a _Custom Log Type_. The action will succeed only if the _Custom Log Type_ is not currently in use by any source.

## Testing a Panther-managed Log Type

{% hint style="info" %}
This feature is available in versions 1.26 and above
{% endhint %}

Need to validate that a Panther-managed schema will work against your logs? You can test sample logs against the Panther-managed schema similarly to testing logs against a custom schema/log type (as described above). Follow the steps below:

* Visit the **Schema** page under the **Data** tab
* Click on a schema labeled as **Panther-managed**
* Once in the schema details page, scroll to the bottom of the page where you'll be able to upload logs

![](<../../.gitbook/assets/Screen Shot 2021-12-02 at 10.05.48 PM (1) (1) (1) (1) (1) (1) (1) (1) (2).png>)

&#x20;

## Writing a Log Schema for JSON logs

> You can make use of our `pantherlog` [CLI tool](./#generating-a-schema-from-json-samples) to help you generate your Log Schema

To parse log files where each line is JSON you have to define a _Log Schema_ that describes the structure of each log entry.

In the example below, the first tab displays the JSON log structure and the second tab shows the Log Schema.

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
{% endtabs %}

You can edit the YAML specifications directly in the Panther Console or they can be [prepared offline in your editor/IDE of choice](reference.md#using-json-schema-in-an-ide). For more information on the structure and fields in a _Log Schema_, see the [Log Schema Reference](reference.md).

## Writing a Log Schema for text logs

Panther handles logs that are not structured as JSON by using a 'parser' that translates each log line into key/value pairs and feeds it as JSON to the rest of the pipeline. You can define a text parser using the `parser` field of the _Log Schema_. Panther provides the following parsers for non-JSON formatted logs:

| Name                              | Description                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| [fastmatch](example-fastmatch.md) | Match each line of text against one or more simple patterns                                                               |
| [regex](example-regex.md)         | Use regular expression patterns to handle more complex matching such as conditional fields, case-insensitive matching etc |
| [csv](example-csv.md)             | Treat log files as CSV mapping colunm names to field names                                                                |

## Pantherlog CLI

Panther provides a simple CLI tool to help work with Custom Logs feature. The tool is called `pantherlog` and an executable for each platform is provided with the release. The executables can be downloaded from the `panther-community` S3 bucket, see more details on the operations help [page](../../help/operations/#tools).

### Generating a Schema from JSON samples

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

### Trying out a Schema

You can use the tool to validate a schema file and use it to parse log files. Note that the events in the log files need to be separated by new line. Processed logs are writen to `stdout` and errors to `stderr`.

For example, to parse logs in `sample_logs.jsonl` with the log schema in `schema.yml`, use:

```
$ ./pantherlog parse --path schema.yml --schemas Schema.Name sample_logs.jsonl
```

The tool can also accept input via `stdin` so it can be used in a pipeline:

```
$ cat sample_logs.jsonl | ./pantherlog parse --path schema.yml
```

### Running tests for a Schema

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

## Uploading Log Schemas with the Panther Analysis Tool

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
