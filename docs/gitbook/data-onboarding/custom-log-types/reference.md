# Log Schema Reference

In this guide, you will find common fields used to build YAML-based schemas when onboarding [Custom Log Types](https://docs.panther.com/data-onboarding/custom-log-types) and [Lookup Table](https://docs.panther.com/enrichment/lookup-tables) schemas.&#x20;

## LogSchema fields

* **`version`** (`0,`_required_):  The version of the log schema. This field should be set to zero (`0`). Its purpose is to allow backwards compatibility with future versions of the log schema.
* **`fields`** ([`[]FieldSchema`](reference.md#fieldschema), _required_):  The fields in each _Log Event_.
* **`parser`** ([`ParserSpec`](reference.md#parserspec)):  Define a parser that will convert non-JSON logs to JSON.

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

* **`fastmatch`** (`FastmatchParser{}`): Use `fastmatch` parser
* **`regex`** (`RegexParser{}`): Use `regex` parser
* **`csv`** (`CSVParser{}`): Use `csv` parser

See the fields for `fastmatch`, `regex`, and `csv` in the tabs below.

{% tabs %}
{% tab title="fastmatch" %}
#### Parser `fastmatch` fields

* **`match`** (_required_, `[]string`): One or more patterns to match log lines against. This field cannot be empty.
* **`emptyValues`** (`[]string`): Values to consider as `null`.
* **`expandFields`** (`map[string]string`): Additional fields to be injected by expanding text templates.
* **`trimSpace`** (`bool`): Trim space surrounding each value.
{% endtab %}

{% tab title="regex" %}
#### Parser `regex` fields <a href="#parser-regex-fields" id="parser-regex-fields"></a>

* **`match`** (_required_, `[]string`): A pattern to match log lines against (can be split it into parts for documentation purposes). This field cannot be empty.
* **`patternDefinitions`** (`map[string]string`): Additional named patterns to use in match pattern.
* **`emptyValues`** (`[]string`): Values to consider as `null`.
* **`expandFields`** (`map[string]string`): Additional fields to be injected by expanding text templates.
* **`trimSpace`** (`bool`): Trim space surrounding each value.
{% endtab %}

{% tab title="csv" %}
#### Parser `csv` <a href="#parser-csv-fields" id="parser-csv-fields"></a>

* **`delimiter`** (_required_, string): A character to use as field delimiter.
* **`hasHeader`** (bool): Use first row to derive column names (unless `columns` is set also in which case the header is just skipped).
* **`columns`** (`[]string`, `required(without hasHeader)`, `non-empty`): Names for each column in the CSV file. If not set, the first row is used as a header.
* **`emptyValues`** (`[]string`): Values to consider as `null`.
* **`trimSpace`** (`bool`): Trim space surrounding each value.
* **`expandFields`** (`map[string]string`): Additional fields to be injected by expanding text templates.
{% endtab %}
{% endtabs %}

### FieldSchema

A _FieldSchema_ defines a field and its value. The field is defined by:

* **`name`** (_required_, `String`):  The name of the field.
* **`required`** (`Boolean`):  If the field is required or not.
* **`description`** (`String`):  Some text documenting the field.

Its value is defined using the fields of a [`ValueSchema`](reference.md#valueschema).

### ValueSchema

A `ValueSchema` defines a value and how it should be processed. Each `ValueSchema` has a `type` field that can be of the following values:

| Value Type  | Description                                                                        |
| ----------- | ---------------------------------------------------------------------------------- |
| `string`    | A string value                                                                     |
| `int`       | A 32-bit integer number in the range `-2147483648`, `2147483647`                   |
| `smallint`  | A 16-bit integer number in the range `-32768`, `32767`                             |
| `bigint`    | A 64-bit integer number in the range `-9223372036854775808`, `9223372036854775807` |
| `float`     | A 64-bit floating point number                                                     |
| `boolean`   | A boolean value `true` / `false`                                                   |
| `timestamp` | A timestamp value                                                                  |
| `array`     | A JSON array where each element is of the same type                                |
| `object`    | A JSON object of _known_ keys                                                      |
| `json`      | Any valid JSON value (JSON object, array, number, string, boolean)                 |

The fields of a `ValueSchema` depend on the value of the `type` field.

| Type        | Field                       | Value                                     | Description                                                                                     |
| ----------- | --------------------------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `object`    | **`fields`**  (required)    | [`[]FieldSpec`](reference.md#fieldschema) | An array of `FieldSpec` objects describing the fields of the object.                            |
| `array`     | **`element`** (required)    | [`ValueSchema`](reference.md#valueschema) | A `ValueSchema` describing the elements of an array.                                            |
| `timestamp` | **`timeFormat`** (required) | `String`                                  | The format to use for parsing the timestamp. (see [Timestamps](reference.md#timestamps))        |
| `timestamp` | **`isEventTime`**           | `Boolean`                                 | A flag to tell Panther to use this timestamp as the _Log Event Timestamp_.                      |
| `timestamp` | **`isExpiration`**          | `Boolean`                                 | (For lookup tables only) A flag to tell Panther to ignore all events after this timestamp       |
| `string`    | **`indicators`**            | `[]String`                                | Tells Panther to extract indicators from this value (see [Indicators](reference.md#indicators)) |
| `string`    | **`validate`**              | [`Validation`](reference.md#validation)   | Validation rules for the string value                                                           |

### Timestamps

Timestamps are defined by setting the `type` field to `timestamp` and specifying the timestamp format using the `timeFormat` field. Timestamp formats can be either one of the built-in timestamp formats:

| timeFormat | Example                       | Description                                                                                                 |
| ---------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `rfc3339`  | 2022-04-04 17:09:17.386345000 | The most common timestamp format.                                                                           |
| `unix`     | 1649097448                    | Timestamp expressed in seconds since UNIX epoch time. It can handle fractions of seconds as a decimal part. |
| `unix_ms`  | 1649097491531                 | Timestamp expressed in milliseconds since UNIX epoch time.                                                  |
| `unix_us`  | 1649097442000000              | Timestamp expressed in microseconds since UNIX epoch time.                                                  |
| `unix_ns`  | 1649097442000000000           | Timestamp expressed in nanoseconds since UNIX epoch time.                                                   |

or you can define a custom format by using [strftime](https://strftime.org) notation. For example:

```yaml
# The field is a timestmap using a custom timestamp format like "2020-09-14 14:29:21"
- name: ts
  type: timestamp
  timeFormat: "%Y-%m-%d %H:%M:%S" # note the quotes required for proper YAML syntax
```

Timestamp values can be marked with `isEventTime: true` to tell Panther that it should use this timestamp as the `p_event_time` field. It is possible to set `isEventTime` on multiple fields. This covers the cases where some logs have optional or mutually exclusive fields holding event time information. Since there can only be a single `p_event_time` for every _Log Event_, the priority is defined using the order of fields in the schema.

Timestamp values in Lookup Table schemas can also be marked with `isExpiration: true`. This is used to tell the Panther Rules Engine to ignore new data if the current time is after this timestamp. These can be useful to "time bound" alerts to independent indicators of compromise (IOCs) added via Lookup Tables, which make for richer alert context.

### Indicators

Values of `string` type can be used as indicators. In order to mark a field as an indicator, you must set the `indicators` field to an array of indicator scanner names. This will instruct Panther to parse the string and store any indicator values it finds to the relevant field. For example:

```yaml
# Will scan the value as IP address and store it to `p_any_ip_addresses`
- name: remote_ip
  type: string
  indicators: [ ip ]

# Will scan the value as a domain name and/or IP address.
# Will store the result in `p_any_domain_names` and/or `p_any_ip_addresses`
- name: target_url
  type: string
  indicators: [ url ]
```

The following values are valid to use in the `indicators` field (more than one may be used):

| Indicator Name    | Extracted into fields                                                                                    | Description                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| aws\_account\_id  | p\_any\_aws\_account\_ids                                                                                | If the value is a valid AWS account id then append to p\_any\_aws\_account\_ids.                                                                                                                                                                                                                                                                                                    |
| aws\_arn          | <p>p_any_aws_arns,<br>p_any_aws_instance_ids,<br>p_any_aws_account_ids,<br>p_any_emails<br><em></em></p> | <p>If value is a valid AWS ARN then append to p_any_aws_arns.<br>If the ARN contains an AWS account id, extract and append to p_any_aws_account_ids.<br>If the ARN contains an EC2 instance id, extract and append to p_any_aws_instance_ids.<br>If the ARN references an AWS STS Assume Role and contains and email address, then extract email address into p_any_emails.<br></p> |
| aws\_arn\_only    | p\_any\_aws\_arns                                                                                        | If value is a valid AWS ARN then append to p\_any\_aws\_arns.                                                                                                                                                                                                                                                                                                                       |
| aws\_instance\_id | p\_any\_aws\_instance\_ids                                                                               | If the value is a valid AWS instance id then append to p\_any\_aws\_instance\_ids.                                                                                                                                                                                                                                                                                                  |
| aws\_tag          | p\_any\_aws\_tags                                                                                        | Append value into p\_any\_aws\_tags.                                                                                                                                                                                                                                                                                                                                                |
| domain            | p\_any\_domain\_names                                                                                    | Append value to p\_any\_domain\_names.                                                                                                                                                                                                                                                                                                                                              |
| email             | p\_any\_emails                                                                                           | If value is a valid email address then append value into p\_any\_emails.                                                                                                                                                                                                                                                                                                            |
| hostname          | p\_any\_domain\_names, p\_any\_ip\_addresses                                                             | <p>Append value to p_any_domain_names.<br>If value is a valid ipv4 or ipv6 address then append to p_any_ip_addresses.</p>                                                                                                                                                                                                                                                           |
| ip                | p\_any\_ip\_addresses                                                                                    | If value is a valid ipv4 or ipv6 address then append to p\_any\_ip\_addresses.                                                                                                                                                                                                                                                                                                      |
| md5               | p\_any\_md5\_hashes                                                                                      | If the value is a valid md5 then append value into p\_any\_md5\_hashes.                                                                                                                                                                                                                                                                                                             |
| net\_addr         | p\_any\_domain\_names, p\_any\_ip\_addresses                                                             | <p>Extracts from values of the form &#x3C;host>:&#x3C;port>.<br>Append host portion to p_any_domain_names.<br>If host portion  is a valid ipv4 or ipv6 address then append to p_any_ip_addresses.</p>                                                                                                                                                                               |
| sha1              | p\_any\_sha1\_hashes                                                                                     | If the value is a valid sha1 then append to p\_any\_sha1\_hashes.                                                                                                                                                                                                                                                                                                                   |
| sha256            | p\_any\_sha256\_hashes                                                                                   | If the value is a valid sha256 then append to p\_any\_sha256\_hashes.                                                                                                                                                                                                                                                                                                               |
| trace\_id         | p\_any\_trace\_ids                                                                                       | Append value to p\_any\_trace\_ids.                                                                                                                                                                                                                                                                                                                                                 |
| url               | p\_any\_domain\_names, p\_any\_ip\_addresses                                                             | <p>Parse url, extract the host portion.<br>Append host portion to p_any_domain_names.<br>If host portion  is a valid ipv4 or ipv6 address then append to p_any_ip_addresses.</p>                                                                                                                                                                                                    |
| username          | p\_any\_usernames                                                                                        | Append value into p\_any\_usernames.                                                                                                                                                                                                                                                                                                                                                |

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



Values of `string` type can be restricted to match well-known formats. Currently, Panther supports the `ip` and `cidr` formats to require that a string value be a valid IP address or CIDR range. Note that the `ip` and `cidr` validation types can be combined with `allow` or `deny` rules but it is somewhat redundant, for example, if you allow two IP addresses, then adding an `ip` validation will simply ensure that your validation will not include false positives if the IP addresses in your list are not valid.

```yaml
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

You can find instructions on how to configure JetBrains IDEs to use custom JSON Schemas [here](https://www.jetbrains.com/help/phpstorm/json.html#ws\_json\_schema\_add\_custom).

### VSCode

You can find instructions on how to configure VSCode to use JSON Schema [here](https://code.visualstudio.com/Docs/languages/json#\_json-schemas-and-settings).

