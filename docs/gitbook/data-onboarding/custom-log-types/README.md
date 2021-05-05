# Custom Log Types

## Overview

Panther allows users to define their own log types by adding a _Custom Log Type_ entry. _Custom Log Types_ are identified by a `Custom.` prefix in their name and can be used wherever a 'native' _Log Type_ is used:

* You can use a _Custom Log Type_ when onboarding data through SQS or S3
* You can write Rules for these _Log Types_.
* You can query the data in Data Explorer. Panther will create a new table for the _Custom Log Type_, once you onboard a source that uses it.
* You can query the data through Indicator Search

## Adding a Custom Log Type

You can add a _Custom Log Type_ by navigating to _Log Analysis_ -&gt; _Custom Logs_ and clicking on the 'New Schema' button in the upper right corner.

![Create custom logs screen](../../.gitbook/assets/custom-logs-create.png)

Here you must enter a name for the _Custom Log Type_ \(ie `Custom.SampleAPI`\) and write or paste your YAML _Log Schema_ definition. Use the 'Validate Syntax' button at the bottom to verify your schema contains no errors and hit 'Save'.

> Note that the 'Validate Syntax' button only checks the **syntax** of the _Log Schema_. 'Save' might still fail due to name conflicts.

If all went well, you can now navigate to _Log Analysis_ -&gt; _Sources and add either add a new source or modify an existing one to use the new `Custom.SampleAPI` \_Log Type_. Once Panther receives events from this _Source_, it will and process the logs and store the _Log Events_ to the `custom_sampleapi` table. You can now write _Rules_ to match against these logs and query them using the _Data Explorer_.

## Editing a Custom Log Type

Panther allows limited **editing** of _Custom Log Types_. Specifically:

* You _can_ modify the `parser` configuration to fix bugs or add new patterns.
* You _can_ add new fields to the schema.
* You _cannot_ rename existing fields.
* You _cannot_ deleted existing fields \(doing so would allow renaming in two steps\).
* You _cannot_ change the `type` of an existing field \(this includes the element type for `array` fields\).

You can edit a _Custom Log Type_ by clicking on the _Edit_ action in the details page of a _Custom Log Type_. Modify the YAML and click _Update_ to submit your change. _Validate Syntax_ can check the YAML for structural compliance but the rules described above can only be checked on _Update_. The update will be rejected if the rules are not followed.

## Deleting a Custom Log Type

A _Custom Log Type_ can be deleted **if no** _**Source**_ **is using it**.

A deleted _Custom Log Type_ is removed from the listing and its tables are hidden from the Data Explorer view.

Deleting a _Custom Log Type_ does not affect any data already stored in the data lake. All data is still queryable through _Data Explorer_ or _Indicator Search_. However, this requires reserving the _Custom Log Type_ name, even after it has been deleted. of a _Custom Log Type_ even after it has been deleted. Trying to add a log with the same name at a later time, will result in failure due to the name **conflict**.

You can delete a _Custom Log Type_ by clicking on the _Delete_ action in the details page of a _Custom Log Type_. The action will succeed only if the _Custom Log Type_ is not currently in use by any source.

## Writing a Log Schema for JSON logs

> You can make use of our [CLI tool](./#generating-a-schema-from-json-samples) to help you generate your Log Schema

To parse log files where each line is JSON you have to define a _Log Schema_ that describes the structure of each log entry.

For example, suppose you have logs that adhere to the following JSON structure:

```javascript
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

You can define a _Log Schema_ for these logs using:

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

The YAML specification can either be edited directly in the Panther UI or [prepared offline in your editor/IDE of choice](reference.md#using-json-schema-in-an-ide). For more information on the structure and fields in a _Log Schema_, see the [Log Schema Reference](reference.md).

## Writing a Log Schema for text logs

Panther handles logs that are not structured as JSON by using a 'parser' that translates each log line into key/value pairs and feeds it as JSON to the rest of the pipeline. You can define a text parser using the `parser` field of the _Log Schema_. Panther provides the following parsers for non-JSON formatted logs:

| Name | Description |
| :--- | :--- |
| [fastmatch](example-fastmatch.md) | Match each line of text against one or more simple patterns |
| [regex](example-regex.md) | Use regular expression patterns to handle more complex matching such as conditional fields, case-insensitive matching etc |
| [csv](example-csv.md) | Treat log files as CSV mapping colunm names to field names |

## Pantherlog CLI

Panther provides a simple CLI tool to help work with Custom Logs feature. The tool is called `pantherlog` and an executable for each platform is provided with the release. The executables can be downloaded from the `panther-community` S3 bucket, see more details on the operations help [page](../../help/ops-home.md#tools). 

### Generating a Schema from JSON samples

You can use the tool to generate a schema file out of sample files in [new-line delimited JSON](http://ndjson.org/) format. The tool will scan the provided logs and print the inferred schema to `stdout`.

For example, to infer the schema of logs `sample_logs.jsonl` and output to `schema.yml`, use:

```text
$ ./pantherlog infer sample_logs.jsonl > schema.yml
```

> **WARNING**: The tool has the following limitations:
>
> * It will identify a string as a timestamp, only if the string is in RFC3339 format. Make sure to review the schema after it is generated by the tool
>
>   and identify fields that should be of type `timestamp` instead.
>
> * It will not mark any timestamp field as `isEventTime:true`. Make sure to select the appropriate `timestamp` field and mark it as `isEventTime:true`.
>
>   For more information regarding `isEventTime:true` see [timestamp](reference.md#timestamps).
>
> * It is able to infer only 3 types of indicators: `ip`, `aws_arn`, `url`. Make sure to review the fields and add more indicators as appropriate.
>
> Make sure to review the schema generated and edit it appropriately before deploying to your production environment!

### Trying out a Schema

You can use the tool to validate a schema file and use it to parse log files. Note that the events in the log files need to be separated by new line. Processed logs are writen to `stdout` and errors to `stderr`.

For example, to parse logs in `sample_logs.jsonl` with the log schema in `schema.yml`, use:

```text
$ ./pantherlog parse --path schema.yml --schemas Schema.Name sample_logs.jsonl
```

The tool can also accept input via `stdin` so it can be used in a pipeline:

```text
$ cat sample_logs.jsonl | ./pantherlog parse --path schema.yml
```

### Running tests for a Schema

You can use the tool to run unit tests. You can define unit tests for your Custom Schema in YAML files. The format for the unit tests is described in [writing parsers](../../development/writing-parsers.md#step-0-writing-a-test). To run tests defined in a `schema_tests.yml` file for a custom schema defined in `schema.yml` use:

```text
$ ./pantherlog test schema.yml schema_tests.yml
```

The first argument is a file or directory containing schema YAML files. The rest of the arguments are test files to run. If you don't specify any test files arguments, and the first argument is a directory, the tool will look for tests in YAML files with a `_tests.yml` suffix.

