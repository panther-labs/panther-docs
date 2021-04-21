# Text logs in CSV format

The process of parsing files in CSV format is based on converting each row into a simple JSON object mapping keys to values. To do that, each column must be given a name.

## CSV logs without header

To parse CSV logs without a header row, Panther needs to know which names to assign to each column.

Let's assume our logs are CSV with 7 columns: year, month, day, time, action, ip\_address, message. Some example rows of this file could be:

```text
2020,09,01,10:35:23,SEND,192.168.1.3,"PING"
2020,09,01,10:35:25,RECV,192.168.1.3,"PONG"
2020,09,01,10:35:25,RESTART,-,"System restarts"
```

We would use the following _LogSchema_ to define log type:

```yaml
version: 0
parser:
  csv:
    # CSV files come in many flavors and you can choose the delimiter character to split each row
    delimiter: "," 
    # Names in the 'columns' array will be mapped to columns in each row.
    # If you want to skip a column, you can set the name at the same index to an empty string ("")
    columns:
    - year
    - month
    - day
    - time
    - action
    - ip_address
    - message
    # CSV files sometimes use placeholder values for missing or N/A data.
    # You can define such values with 'emptyValues' and they will be ignored.
    emptyValues: ["-"]
    # The 'expandFields' directive will render a template string injecting generated fields into the key/value pairs
    expandFields:
      # Since the timestamp is split across multiple columns, we need to re-assemble it into RFC3339 format
      # The following will add a 'timestamp' field by replacing the fields from CSV values
      timestamp: '%{year}-%{month}-%{day}T%{time}Z'
fields:
- name: timestamp
  type: timestamp
  timeFormat: rfc3339
  isEventTime: true
  required: true
- name: action
  type: string
  required: true
- name: ip_address
  type: string
  indicators: [ip]
- name: message
  type: string
```

## CSV logs with header

{% hint style="danger" %}
Avoid using such schemas in combination with others. Use a separate source or S3 prefix.
{% endhint %}

To parse CSV logs that starts with a header row, Panther has two options:

* Use the names defined in the header as the names for the JSON fields or,
* Skip the header and define the names the same way we did for headerless CSV files

To use the names in the header the configuration for the parser should be:

```yaml
parser:
  csv:
    delimiter: "," 
    # Setting 'hasHeader' to true without specifying a 'columns' field,
    # tells Panther to set the column names from values in the header.
    hasHeader: true
    # In case you want to rename a column you can use the 'expandFields' directive
    expandFields:
      # Let's assume that the header contains '$cost' as column name and you want to 'normalize' it as 'cost_us_dollars'
      "cost_us_dollars": '%{$cost}'
```

To ignore the header and define your set of names for the columns use:

```yaml
parser:
  csv:
    delimiter: "," 
    # Setting 'hasHeader' to true while also specifying a 'columns' field, 
    # tells Panther to ignore the header and use the names in the 'columns' array
    hasHeader: true
    columns:
    - foo
    - bar
    - baz
```

