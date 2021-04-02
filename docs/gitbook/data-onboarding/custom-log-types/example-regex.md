# Text logs with regular expressions

For text log types with more complex structure, you can use the `regex` parser.

The `regex` parser uses named groups in regular expressions to extract field values from each line of text. You can use grok syntax \(i.e. `%{PATTERN_NAME:field_name}`\) to build complex expressions taking advantage of the built-in patterns provided by Panther or by defining your own.

For example to match the text

```text
2020-10-10T14:32:05 [FOO_SERVICE@127.0.0.1] [DEBUG] "" Something when wrong
```

We can use this grok syntax with this pattern:

```text
%{NOTSPACE:timestamp} \[%{WORD:service}@%{DATA:ip}\] \[%{WORD:log_level}\] %{GREEDYDATA:message}
```

Which is the rough equivalent of this 'raw' regular expression:

```text
(?P<timestamp>\S+) \[(?P<service>\w+)@(?P<ip>.*?)\] \[(?P<log_level>\w+)\] (?P<message>.*)
```

> **WARNING** Panther's log processor uses the [`RE2` syntax](https://github.com/google/re2/wiki/Syntax) for regular expressions. `RE2` does not support some operations common to other regular expression engines such as 'look-behind'. Make sure to check any expressions or grok patterns you copy-paste from other systems.
>
> **NOTE**: For best performance stick to simple built-in patterns such as `DATA`, `NOTSPACE`, `GREEDYDATA` and `WORD`. Avoid complex expressions unless it is required to distinguish the field name based on the value \(e.g. `(%{IP:ip_address}|%{WORD:username})`

Using the `regex` parser we will define a log type for `Juniper.Audit` logs. Panther already support these logs natively, but we will be using them here because they have variable conflicting forms and can only be 'solved' by using `regex` parser.

The sample logs for `Juniper.Audit` are:

```text
Jan 22 16:14:23 my-jwas [mws-audit][INFO] [mykonos] [10.10.0.117] Logged in successfully
Jan 23 19:16:22 my-jwas [mws-audit][INFO] [ea77722a8516b0d1135abb19b1982852] Deactivate response 1832840420318015488
Feb 7 20:29:51 my-jwas [mws-audit][INFO] [mykonos] [10.10.0.113] Login failed. Attempt: 1
Feb 14 19:02:54 my-jwas [mws-audit][INFO][mykonos] Changed configuration parameters: services.spotlight.enabled, services.spotlight.server_address
```

```yaml
version: 0
parser:
  regex:
    patternDefinitions:
      JUNIPER_TIMESTAMP: '[A-Z][a-z]{2} \d?\d \d\d:\d\d:\d\d'
      # An apikey is composed of 32 hex characters
      API_KEY: '[a-fA-F0-9]{32}'
      # A user name is any word less than 32 characters long
      USERNAME: '\w{1,31}'
    # We will be splitting the pattern in multiple parts so we can add comments helping us debug it in the future.
    # All parts are concatenated into a single pattern by Panther WITHOUT ADDING SPACES BETWEEN PARTS.
    # If you don't want to split your patterns just use an array with a single string.
    match:
    # The log line starts with a timestamp (captured as 'timestamp')
    - '^%{JUNIPER_TIMESTAMP:timestamp}'
    # Followed by this static text
    - ' my-jwas \[mws-audit\]'
    # Then comes the log level surrounded by square brackets and optional space (captured as 'log_level')
    - '\[%{DATA:log_level}\] ?' 
    # After it, we get either an api key or a user name, surrounded by square brackets,
    # which we capture as 'apikey' or 'username' depending on the match
    - '(\[%{API_KEY:apikey}|%{USERNAME:username}\]) '
    # Optionally followed by the ip address of the request in square brackets (captured as 'request_ip')
    # Note that we use 'DATA' instead of the specific 'IP' named pattern. 
    # It is not needed because 'request_ip' is always at this position and we are certain of the log type match
    # due to the distinctive ' my-jwas [mws-audit]' literal.
    - '(\[%{DATA:request_ip}\])?'
    # And finally the rest of the line is the message (captured as 'message')
    - '%{GREEDYDATA:message}'
    trimSpace: true # We want to trim the space of the message
fields:
- name: timestamp
  type: timestamp
  required: true
  timeFormat: '%b %d %H:%M:%S'
  isEventTime: false # the timestamps have no year so we cannot use them as partition time
- name: log_level
  type: string
  required: true
- name: apikey
  type: string
- name: username
  type: string
- name: request_ip
  type: string
  indicators: [ip]
- name: message
  type: string
```

