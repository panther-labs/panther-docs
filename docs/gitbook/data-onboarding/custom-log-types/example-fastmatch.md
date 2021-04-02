# Text logs with fastmatch

The `fastmatch` parser uses simple string patterns that specify the position of fields within a log line. As the name suggests it is very fast and should be the preferred method to parse text logs. It can handle most cases of structured text logs where the order of fields is known. In fact, it is so fast you can specify multiple patterns that will be tested in order, so you can 'solve' cases where there are a few variations in the structure of the log line.

We will be using the following example log line that is using Apache Common Log format:

```text
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```

To parse this log type using `fastmatch` we would define the following log schema:

```yaml
version: 0
parser:
  fastmatch:
    # Define an array of patterns to match against.
    # In this example we only use one pattern because the log format is the same for all lines.
    # If we wanted to include the Apache Extended Log format, we could provide an additional pattern.
    match:
      - '%{remote_ip} %{identity} %{user} [%{timestamp}] "%{method} %{request_uri} %{protocol}" %{status} %{bytes_sent}'
    emptyValues: [ '-' ] # specify that `-` string values are considered null
fields:
  - name: remote_ip
    type: string
    indicators:
      - ip
  - name: identity
    type: string
  - name: user
    type: string
  - name: timestamp
    type: timestamp
    isEventTime: true
    timeFormat: '%d/%b/%Y:%H:%M:%S %z'
  - name: method
    type: string
  - name: request_uri
    type: string
  - name: protocol
    type: string
  - name: status
    type: int
  - name: bytes_sent
    type: bigint
```

## Understanding fastmatch patterns

The patterns use `%{field_name}` placeholders to set where in the log line a field is expected. For example to match the text

```text
2020-10-10T14:32:05 [FOO_SERVICE@127.0.0.1] [DEBUG] "" Something when wrong
```

We can use this pattern \(surrounded by single quotes for clarity\):

```yaml
'%{timestamp} [%{service}@%{ip}] [%{log_level}] %{message}'
```

### Delimiters

The text between two consecutive fields defines the 'delimiter' between them.

Delimiters cannot be empty.

In the example above we cannot omit the `"@"` between `service` and `ip` in the pattern

The field _preceding_ a delimiter cannot contain the delimiter text. In the example above:

* `timestamp` cannot contain space `" "`
* `service` cannot contain `"@"`
* `ip` cannot contain `"] ["`
* `log_level` cannot contain `"] "`

### Anonymous fields

Field placeholders without names \(`%{}`\) are ignored.

### Tail capture

If the last field in a pattern does not have any delimiter text _after_ it, it will capture _everything_ until the end of the text. In the example above `message` will capture `"Something when wrong"`

### Handling quotes

In some cases fields can be quoted within the text:

```text
2020-10-10T14:32:05 "Some quoted text with \"escaped quotes\" inside"
```

To properly unescape such fields just surround the field placeholder with quotes:

```text
%{timestamp} "%{message}"
```

This works for both single and double quotes.

