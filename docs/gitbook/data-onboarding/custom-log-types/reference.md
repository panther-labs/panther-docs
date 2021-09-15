# Log Schema Reference

In this guide, you will find common fields used to build YAML-based schemas when onboarding custom log types \(logs that do not have built-in support in Panther\). 

## LogSchema fields

* **version** \(`0`,`required`\):  The version of the log schema. This field should be set to zero \(`0`\). Its purpose is to allow backwards compatibility with future versions of the log schema.
* **fields** \([`[]FieldSchema`](reference.md#fieldschema), `required`\):  The fields in each _Log Event_.
* **parser** \([`ParserSpec`](reference.md#parserspec)\):  Define a parser that will convert non-JSON logs to JSON.

### Example

```yaml
version: 0
parser:
  csv:
    delimiter: ','
fields:
- name: action
  type: string
  required: true
- name: time
  type: timestamp
  timeFormat: unix
```

### ParserSpec

A _ParserSpec_ specifies a parser to use to convert non-JSON input to JSON. Only one of the following fields can be specified:

* **fastmatch** \(`FastmatchParser{}`\): Use `fastmatch` parser
* **regex** \(`RegexParser{}`\): Use `regex` parser
* **csv** \(`CSVParser{}`\): Use `csv` parser

#### Parser `fastmatch` fields <a id="parser-fastmatch-fields"></a>

* **match** \(`[]string`, `required`, `non-empty`\): One or more patterns to match log lines against.
* **emptyValues** \(`[]string`\): Values to consider as `null`.
* **expandFields** \(`map[string]string`\): Additional fields to be injected by expanding text templates.
* **trimSpace** \(`bool`\): Trim space surrounding each value

#### Parser `regex` fields <a id="parser-regex-fields"></a>

* **match** \(`[]string`, `required`, `non-empty`\): A pattern to match log lines against \(can be split it into parts for documentation purposes\).
* **patternDefinitions** \(`map[string]string`\): Additional named patterns to use in match pattern.
* **emptyValues** \(`[]string`\): Values to consider as `null`.
* **expandFields** \(`map[string]string`\): Additional fields to be injected by expanding text templates.
* **trimSpace** \(`bool`\): Trim space surrounding each value

#### Parser `csv` <a id="parser-csv-fields"></a>

* **delimiter** \(`string`, `required`, `non-empty`\): A character to use as field delimiter.
* **hasHeader** \(`bool`\): Use first row to derive column names \(unless `columns` is set also in which case the header is just skipped\).
* **columns** \(`[]string`, `required(without hasHeader)`, `non-empty`\): Names for each column in the CSV file. If not set, the first row is used as a header.
* **emptyValues** \(`[]string`\): Values to consider as `null`.
* **trimSpace** \(`bool`\): Trim space surrounding each value
* **expandFields** \(`map[string]string`\): Additional fields to be injected by expanding text templates.

### FieldSchema

A _FieldSchema_ defines a field and its value. The field is defined by:

* **name** \(`String`,`required`\):  The name of the field.
* **required** \(`Boolean`\):  If the field is required or not.
* **description** \(`String`\):  Some text documenting the field.

And its value is defined using the fields of a [`ValueSchema`](reference.md#valueschema).

### ValueSchema

A `ValueSchema` defines a value and how it should be processed. Each `ValueSchema` has a `type` field that can be of the following values:

| Value Type | Description |
| :--- | :--- |
| `string` | A string value |
| `int` | A 32-bit integer number in the range `-2147483648`, `2147483647` |
| `smallint` | A 16-bit integer number in the range `-32768`, `32767` |
| `bigint` | A 64-bit integer number in the range `-9223372036854775808`, `9223372036854775807` |
| `float` | A 64-bit floating point number |
| `boolean` | A boolean value `true` / `false` |
| `timestamp` | A timestamp value |
| `array` | A JSON array where each element is of the same type |
| `object` | A JSON object of _known_ keys |
| `json` | Any valid JSON value \(JSON object, array, number, string, boolean\) |

The fields of a `ValueSchema` depend on the value of the `type` field.

| Type | Field | Value | Description |
| :--- | :--- | :--- | :--- |
| `object` | **fields**  \(required\) | [`[]FieldSpec`](reference.md#fieldschema) | An array of `FieldSpec` objects describing the fields of the object. |
| `array` | **element** \(required\) | [`ValueSchema`](reference.md#valueschema) | A `ValueSchema` describing the elements of an array. |
| `timestamp` | **timeFormat** \(required\) | `String` | The format to use for parsing the timestamp. \(see [Timestamps](reference.md#timestamps)\) |
| `timestamp` | **isEventTime** | `Boolean` | A flag to tell Panther to use this timestamp as the _Log Event Timestamp_. |
| `string` | **indicators** | `[]String` | Tells Panther to extract indicators from this value \(see [Indicators](reference.md#indicators)\) |
| `string` | **validate** | [`Validation`](reference.md#validation) | Validation rules for the string value |

### Timestamps

Timestamps are defined by setting the `type` field to `timestamp` and specifying the timestamp format using the `timeFormat` field. Timestamp formats can be either one of the built-in timestamp formats:

* `rfc3339` The most common timestamp format.
* `unix` Timestamp expressed in seconds since UNIX epoch time. It can handle fractions of seconds as a decimal part.
* `unix_ms` Timestamp expressed in milliseconds since UNIX epoch time.
* `unix_us` Timestamp expressed in microseconds since UNIX epoch time.
* `unix_ns` Timestamp expressed in nanoseconds since UNIX epoch time.

or you can define a custom format by using [strftime](https://strftime.org) notation. For example:

```yaml
# The field is a timestmap using a custom timestamp format like "2020-09-14 14:29:21"
- name: ts
  type: timestamp
  timeFormat: "%Y-%m-%d %H:%M:%S" # note the quotes required for proper YAML syntax
```

Timestamp values can also be marked with `isEventTime: true` to tell Panther that it should use this timestamp as the `p_event_time` field. It is possible to set `isEventTime` on multiple fields. This covers the cases where some logs have optional or mutually exclusive fields holding event time information. Since there can only be a single `p_event_time` for every _Log Event_, the priority is defined using the order of fields in the schema.

### Indicators

Values of `string` type can be used as indicators. In order to mark a field as an indicator, you must set the `indicators` field to an array of [indicator scanner names](../../development/writing-parsers.md#indicator-strings). This will instruct Panther to parse the string and store any indicator values it finds to the relevant field. For example:

```yaml
# Will scan the value as IP address and store it to `p_any_ip_addresses`
- name: remote_ip
  type: string
  indicators: [ ip ]

# Will scan the value as URL and store domain name or ip address to `p_any_domain_names` or `p_any_ip_addresses`
- name: target_url
  type: string
  indicators: [ url ]
```

### Validation - Allow/Deny lists

Values of `string` type can be further restricted by declaring a list of values to `allow` or `deny`. This allows to have different log types that have common overlapping fields but differ on values of those fields.

```yaml
# Will only allow 'login' and 'logout' event types to match this log type
- name: event_type
  type: string
  validate:
    allow: [ "login", "logout"]
    
# Will match any event type other than 'login' and 'logout'
- name: event_type
  type: string
  validate:
    deny: [ "login", "logout"]
```

### Validation by string type

{% hint style="info" %}
This feature will be available in 1.24
{% endhint %}

Values of `string` type can be restricted to match well-known formats. Currently, Panther supports the `ip` and `cidr` formats to require that a string value be a valid IP address or CIDR range. Note that the `ip` and `cidr` validation types can be combined with `allow` or `deny` rules but it is somewhat redundant, for example, if you allow two IP addresses, then adding an `ip` validation will simply ensure that your validation will not include false positives if the IP addresses in your list are not valid.

```javascript
# Will allow valid ipv4 IP addresses e.g. 100.100.100.100
- name: address
  type: string
  validate:
    ip: "ipv4"
    
# Will allow valid ipv6 CIDR ranges 
# e.g. 2001:0db8:85a3:0000:0000:0000:0000:0000/64
- name: address
  type: string
  validate:
    cidr: "ipv6"
    
# Will allow any valid ipv4 or ipv6 address
- name: address
  type: string
  validate:
    ip: "any"    
```

### Embedded JSON values

Sometimes JSON values are delivered 'embedded' in a string. For example the input JSON could be of the form:

```javascript
{
  "timestamp": "2021-03-24T18:15:23Z",
  "message": "{\"foo\":\"bar\"}"
}
```

To have Panther parse the escaped JSON inside the string you can use an `isEmbeddedJSON: true` flag. This flag is valid for values of type `object`, `array` and `json`.

By defining `message` as:

```yaml
- name: message
  type: object
  isEmbeddedJSON: true
  fields:
    - name: foo
      type: string
```

each event will be stored as:

```javascript
{
  "timestamp": "2021-03-24T18:15:23Z",
  "message": {
    "foo": "bar"
  }
}
```

## Using JSON Schema in an IDE

If your editor/IDE supports [JSON Schema](https://json-schema.org/), you can use [this JSON Schema file](https://gist.github.com/alxarch/e87e15b04ab2bcb0d9739d7f3a36b096) for validation and autocompletion. You can also use the resources below as well to create custom JSON schemas:

### JetBrains

You can find instructions on how to configure JetBrains IDEs to use custom JSON Schemas [here](https://www.jetbrains.com/help/phpstorm/json.html#ws_json_schema_add_custom).

### VSCode

You can find instructions on how to configure VSCode to use JSON Schema [here](https://code.visualstudio.com/Docs/languages/json#_json-schemas-and-settings).



