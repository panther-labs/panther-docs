# Note

These instructions were developed for Panther's [open source repository](https://github.com/panther-labs/panther). The portions of the instructions below that pertain to developing testing code reference these open source libraries. Panther became closed-source in 2021, and these libraries have continued to evolve. _If you are a current SaaS customer and wish to onboard a new log source type, you only need to create a test event `.yml` file (step 0) and a log schema `.go` file (step 1)._ Please submit the files you create to your Support Engineer for inclusion in a future release of Panther.


# Parsers

To add support for a new log type we should: 
1. Define the schema for the new log type 
2. Provide a parser to parse a single log item
3. Build and validate a log type entry
4. Export the log type entry so Panther can use it

## Code structure

Log types are identified by a name that has the form `Service.EventName`. The `Service` prefix groups log types based on the service that produced them (ie `NGINX`, `Juniper`, `Syslog`). All log types for a `Service` are grouped under a go module in the [parsers](https://github.com/panther-labs/panther/tree/master/internal/log_analysis/log_processor/parsers) module. For example all `AWS.*` logs live under the `awslogs` module at `internal/log_analysis/log_processor/parsers/awslogs`.

Each log type is described in its own `.go` file inside its module. For example, `AWS.CloudTrail` is defined in `cloud_trail.go` file at `internal/log_analysis/log_processor/parsers/awslogs/cloudtrail.go`.

All `AWS.*` logtypes exist in the `awslogs` module. That module exports a `LogTypes() logtypes.Group` function to declare all log types it contains. Panther knows to include all exported log types inf modules under `internal/log_analysis/log_processor/parsers/` at runtime.

As an example we will use `FooService.Event`. `FooService.Event` is a log event produced by `FooService` and describes the result of a user request. An example `FooService.Event` log line is:

```javascript
{
  "time": "2020-08-06T15:42:23.415Z",
  "message": "failed to serve a request",
  "remote_ip": "127.0.0.1",
  "referrer_url": "https://example.com",
  "request_id": "123456789",
  "user": {
    "uid": 123456,
    "groups": ["admin","ssh"]
  },
  "duration_s": 0.15
}
```

> NOTE the following steps describe the full process of defining a parser to explain the concepts. If you want you can skip to [TL;DR Give me the code](writing-parsers.md#tl-dr-give-me-the-code)

## Step 0: Writing a test

We will start by writing a test to better understand what we want to achieve and to be sure our code works as it is supposed to be working.

Our goal here is to parse a JSON string of a `Foo.Event` and produce a Panther log event that will be stored in our storage backend to be processed by the rules engine and queried in the security data lake.

Panther provides the `logtesting` package with helpers for testing log parsers. This module allows to run tests declared in YAML files using the `logtesting.RunTestsFromYAML` function. These tests verify that the parser produces the expected log event(s) for a log entry. This method ensures we haven't missed any fields or indicator values throughout the process.

We write a test for `Foo.Event` in a YAML file at `foologs/testdata/foologs_tests.yml`. The `input` is the raw log from the log source, and the `result` is the log after it has been processed by the log processor:

```yaml
name: sample_foo_event
logType: Foo.Event
input: |
  {
    "time": "2020-08-06T15:42:23.415Z",
    "message": "failed to serve a request",
    "remote_ip": "127.0.0.1",
    "referrer_url": "https://example.com",
    "request_id": "123456789",
    "user": {
      "uid": 123456,
      "groups": ["admin","ssh"]
    },
    "duration_s": 0.15
  }
result: |
  {
    "time": "2020-08-06T15:42:23.415Z",
    "message": "failed to serve a request",
    "remote_ip": "127.0.0.1",
    "referrer_url": "https://example.com",
    "request_id": "123456789",
    "user": {
      "uid": 123456,
      "groups": ["admin","ssh"]
    },
    "duration_s": 0.15,
    "p_event_time": "2020-08-06T15:42:23.415Z",
    "p_any_ip_addreses": ["127.0.0.1"],
    "p_any_domain_names": ["example.com"],
    "p_any_trace_ids": ["123456789"],
    "p_log_type": "Foo.Event"
  }
```

Note the additional fields beginning with `p_` appended to the `result`. We call these fields ['panther fields'](../data-analytics/panther-fields.md) and they are prefixed with `p_` to avoid name collisions. To cause these fields to appear in processed logs, a special tag called a `scanner` must be added to any raw event fields that you wish to appear in a given panther field (see "Indicator Strings", below). 

To run the test, we write a simple go test at `foologs/foologs_test.go`. The test will read tests from a YAML file and run them.

```go
package foologs_test

import (
  "testing"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/parsers/foologs"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/logtypes/logtesting"
)

func TestFooEvent(t *testing.T) {
  logtesting.RunTestsFromYAML(t, foologs.LogTypes(), "./testdata/foologs_tests.yml")
}
```

The test should fail at this point since we haven't defined any log types yet.

## Step 1: Defining the log type schema

Now we define the schema for the `Foo.Event` log type. Panther uses Go [structs](https://tour.golang.org/moretypes/2) to define the schema of a log type.

We can represent the schema of a `Foo.Event` as a Go struct with following in `foologs/foologs.go`:

```go
package foologs

import "github.com/panther-labs/panther/internal/log_analysis/log_processor/pantherlog"

type FooEvent struct {
    Time        pantherlog.Time    `json:"time" validate:"required" tcodec:"rfc3339" event_time:"true" description:"The foo event time"`
    Message     pantherlog.String  `json:"message" validate:"required" description:"The foo event time"`
    RemoteIP    pantherlog.String  `json:"remote_ip" panther:"ip" description:"The remote ip used for the request"`
    ReferrerURL pantherlog.String  `json:"referrer_url" panther:"url" description:"The referrer URL used for the request"`
    RequestID   pantherlog.String  `json:"request_id" panther:"trace_id" description:"The id of the request that generated this event"`
    User        *User              `json:"user" description:"The user that made the request"`
    Duration    pantherlog.Float64 `json:"duration_s" description:"The number of seconds it took to serve the request"`
}
type User struct {
  UID    pantherlog.Int64   `json:"uid" description:"The id of the user that made the request"`
  Groups []string           `json:"groups" description:"The groups the user is a member of"`
}
```

### Field types

Fields in a log event `struct` should use the types defined in the `pantherlog` [module](https://github.com/panther-labs/panther/blob/master/internal/log_analysis/log_processor/pantherlog/pantherlog.go#L33) (Note: please ask your support engineer for the current list of supported types in the closed-source version of this module). These types handle `null` values and missing JSON fields by omitting them in the output (`result` portion of the test file, above). Empty strings (`""`) and zero numeric values are included in the output in order to preserve as much as possible from the original log event.

The types are specified in the struct as `pantherlog.<Type>`, where `<Type>` is one of the types listed in the code linked above. See examples on this page.

The `RawMessage` type is for a JSON blob for which you don't wish to create sub-`struct` objects. Note that in order to search for nested attributes and values in your logs using [JSONPath](https://goessner.net/articles/JsonPath/)-like syntax in Panther Data Explorer, you must define sub-`struct` types in your log schema file. You can still search `RawMessage` fields, but you will have to use the `json_extract` [function in Athena](https://prestodb.io/docs/0.217/functions/json.html#json_extract) or the `GET_PATH` [function in Snowflake](https://docs.snowflake.com/en/sql-reference/functions/get_path.html).

Panther uses [struct tags](https://www.digitalocean.com/community/tutorials/how-to-use-struct-tags-in-go) to specify field attributes. All fields _must_ have some `description` to document the contents of this field. These documentation strings will be used to generate user documentation from this code. The `json` tag _must_ use the exact field name used in the raw log. Panther automatically adds the `omitempty` option to the `json:` tag for all fields.

Panther relies on the `validate:` tag to distinguish different log types. The [validator package](https://pkg.go.dev/gopkg.in/go-playground/validator.v9) defines the syntax for this tag. Panther specifically relies on the `validate:"required"` tag, which tells Panther which fields must be present in order to classify a log as a specific type. _The `validate:"required"` tag should be applied to the fewest fields absolutely necessary to identify a given log type_ to minimize processing overhead in Panther. Common fields such as timestamps generally should not be tagged as `"required"`. Additionally, before tagging a field as `"required"`, review a sample of log events from your source to verify that the field is present in _every_ log. _If a `"required"` field is not present in a log event, it will not be correctly identified. It may be dropped or incorrectly classified as a different log type_.

Note: The `json:"omitempty` tag is not the same as the [validator tag](https://pkg.go.dev/gopkg.in/go-playground/validator.v9#hdr-Omit_Empty) `validate:"omitempty".

#### String values

> `pantherlog.String`

#### Indicator strings

String values can be tagged with a `panther:"<scanner>"` to define [indicator fields](../data-analytics/panther-fields.md#indicator-fields). Note that a `scanner` can produce multiple indicator fields from a single value, or a different indicator field based on the value. Panther defines the following scanners:

| Scanner           | Description                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `ip`              | Adds a `p_any_ip_addresses` indicator if the value is a valid IP address                                               |
| `domain`          | Adds a `p_any_domains` indicator                                                                                       |
| `hostname`        | Adds a `p_any_ip_addresses` indicator if the value is a valid IP address otherwise it adds a `p_any_domains` indicator |
| `url`             | Adds a `p_any_domains` or a `p_any_ip_addresses` indicator using the hostname part of the URL                          |
| `net_addr`        | Adds a `p_any_domains` or a `p_any_ip_addresses` indicator by splitting a `HOST:PORT` address                          |
| `sha256`          | Adds a `p_any_sha256_hashes` indicator                                                                                 |
| `sha1`            | Adds a `p_any_sha1_hashes` indicator                                                                                   |
| `md5`             | Adds a `p_any_md5_hashes` indicator                                                                                    |
| `trace_id`        | Adds a `p_any_trace_ids` indicator                                                                                     |
| `aws_arn`         | Scans an ARN string and adds any `p_any_aws_arns`, `p_any_aws_account_id` and `p_any_instance_ids` indicators found    |
| `aws_instance_id` | Adds a `p_any_aws_instance_ids `indicator                                                                              |
| `aws_account_id`  | Adds a `p_any_aws_account_ids` indicator                                                                               |
| `aws_tag`         | Adds a `p_any_aws_tags` indicator                                                                                      |
| `username`        | Adds a `p_any_usernames` indicator                                                                                     |
| `email`           | Adds a `p_any_emails` indicator                                                                                        |

#### Timestamps

> `pantherlog.Time`

Timestamps use a separate Go type to allow easier querying of logs using date time ranges. By default timestamps use the RFC3339 format. To specify a different format for a timestamp use the `tcodec` tag.

#### Numbers

All numeric values can be parsed from either a JSON number or a JSON numerical string (ie both `42` and `"42"` are valid).

#### Floating point numbers

> `pantherlog.Float64`, `pantherlog.Float32`

#### Integers

> `pantherlog.Int64`, `pantherlog.Int32`, `pantherlog.Int16`, `pantherlog.Int8`

If you are certain about the range of values an integer can take, you can use one for the smaller sizes accordingly. This will limit the storage requirements for the columns. If you are unsure about the range limits use `pantherlog.Int64`.

#### Unsigned Integers

> `pantherlog.Uint64`, `pantherlog.Uint32`, `pantherlog.Uint16`, `pantherlog.Uint8`

If you are certain about the range of values an integer can take, you can use one for the smaller sizes accordingly. A usual example is a port number that can fit in a `pantherlog.Uint16`. This will limit the storage requirements for the columns. If you are unsure about the range limits use `pantherlog.Uint64`.

#### Booleans

> `pantherlog.Bool`

Boolean values handle `null` case by omitting the field when encoding to JSON.

#### Objects

If our log has nested objects we define a separate struct to define the object. Fields that have an object value should use a pointer so the nested object can be safely omitted in the output if it is `null` or missing in the log input:

```go
type FooEvent struct {
    User        *FooUser              `json:"user" description:"The user that made the request"`
}

type FooUser struct {
    Username    pantherlog.String     `json:"username" description:"The username"`
    Usertype    pantherlog.String     `json:"usertype" description:"The type of user"`
}

```


#### Arrays

We use a simple Go slice which will be omitted in the output if it's empty.

## Step 2: Defining a parser

Now that we have defined the schema of `Foo.Event` we need to provide a way for panther to parse log input into a panther log event. To achieve this we need to provide a type implementing `parsers.Interface`.

```go
type Interface interface {
    ParseLog(input string) (results []*pantherlog.Result, err error)
}
```

The parser takes a log item (in most cases a line of text) and tries to parse it into one or more log events. If it fails to parse the line it should return a `nil` slice and an error.

For our example the parser would be:

```go
package foologs

import (
    jsoniter "github.com/json-iterator/go"
    "github.com/panther-labs/panther/internal/log_analysis/log_processor/pantherlog"
    "github.com/panther-labs/panther/internal/log_analysis/log_processor/parsers"
)

type FooParser struct {
  pantherlog.ResultBuilder
}

func (p *FooParser) ParseLog(input string) ([]*pantherlog.Result, error) {
  event := FooEvent{}
  // We unmarshal the string using jsoniter
  if err := jsoniter.UnmarshalFromString(input, &event); err != nil {
    return nil, err
  }
  // We validate the struct using the `validate` struct tags.
  if err := parsers.ValidateStruct(&event); err != nil {
    return nil, err
  }

  result, err := p.BuildResult("Foo.Event", &event)
  if err != nil {
    return nil, err
  }

  return []*pantherlog.Result{result}, nil
}
```

Parser code follows this general pattern: 1. Parse the input text into a struct 2. Validate the parsed output to ensure the input is valid for this log type 3. Package the log event(s) into `pantherlog.Result` values so the log processor can store them.

Fields marked with a `panther` struct tag will only be processed when the `Result` is **encoded** to JSON. This is deliberate in order to be able to support both JSON and text-based log types. In the log processor pipeline this happens in the final stage when the result is written to a buffer that will be uploaded to S3.

> **Attention** This means that `EventTime` will only be set on the `Result` when it is **encoded** to JSON.

If a log event requires special logic to produce the event timestamp it can implement `pantherlog.EventTimer` interface:

```go
type EventTimer interface {
   PantherEventTime() time.Time
}
```

The `time.Time` returned by `EventTimer` instances _takes precedence_ over event timestamps defined with struct tags.

## Step 3: Build and validate a log type entry

Now that we have our parser and schema ready, we need to map the `Foo.Event` log type to them. Panther keeps track of supported log types using a registry of log types.

To describe a log type entry we use `logtypes.Config`, a struct including all the information required to build a `logtypes.Entry` for any log type. The configuration for `Foo.Event` would look like this:

```go
package foologs

var configFooEvent = logtypes.Config{
    Name: "Foo.Event",
    Description: "Foo log event",
    ReferenceURL: "https://example.com/logs/foo-event", // used in generated documentation
    Schema: FooEvent{},
    NewParser: parsers.FactoryFunc(func (_ interface{}) (parsers.Interface, error) {
      return &FooParser{}, nil
    }),
}
```

To validate the configuration and build a `logtypes.Entry` we need to use its `BuildEntry() (logtypes.Entry, error)` function

```
logtypes.Build(config logtypes.Builder) (logtypes.Entry, error)
```

We prefer to use the panicking helper `logtypes.MustBuild` so we are informed of any errors at compile time.

```go
package foologs

var logTypeFooEvent = logtypes.MustBuild(configFooEvent)
```

To export all `logtype.Entry` values declared in a package group them together in a `logtypes.Group` which will provide a read-only view of our `logtypes.Entry` collection. Note that each group has a name to describe the purpose of the grouping. We choose "Foo" for naming our group.

```go
package foologs

var exportedFooLogTypes = logtypes.Must("Foo", logTypeEvent)
```

Finally we need to export the log types using a `LogTypes() logtypes.Group` function so Panther knows to register and use them at runtime.

```go
package foologs
func LogTypes() logtypes.Group {
  return exportedFooLogTypes
}
```

Putting it all together and avoiding intermediate variables we have:

```go
package foologs

import (
  jsoniter "github.com/json-iterator/go"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/pantherlog"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/logtypes"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/parsers"
)

func LogTypes() logtypes.Group {
  return exportedFooLogTypes
}

var exportedFooLogTypes = logtypes.Must("Foo", logtypes.Config{
   Name: "Foo.Event",
   Description: "Foo log event",
   ReferenceURL: "https://example.com/logs/foo-event", // used in generated documentation
   Schema: FooEvent{},
   NewParser: parsers.FactoryFunc(func (_ interface{}) (parsers.Interface, error) {
     return &FooParser{}, nil
   }),
})

type FooEvent struct {
    Time        pantherlog.Time    `json:"time" validate:"required" tcodec:"rfc3339" event_time:"true" description:"The foo event time"`
    Message     pantherlog.String  `json:"message" validate:"required" description:"The foo event time"`
    RemoteIP    pantherlog.String  `json:"remote_ip" panther:"ip" description:"The remote ip used for the request"`
    ReferrerURL pantherlog.String  `json:"referrer_url" panther:"url" description:"The referrer URL used for the request"`
    RequestID   pantherlog.String  `json:"request_id" panther:"trace_id" description:"The id of the request that generated this event"`
    User        *User              `json:"user" description:"The user that made the request"`
    Duration    pantherlog.Float64 `json:"duration_s" description:"The number of seconds it took to serve the request"`
}
type User struct {
  UID    pantherlog.Int64   `json:"uid" description:"The id of the user that made the request"`
  Groups []string           `json:"groups" description:"The groups the user is a member of"`
}

type FooParser struct {
  pantherlog.ResultBuilder
}

func (p *FooParser) ParseLog(input string) ([]*pantherlog.Result, error) {
  event := FooEvent{}
  // We unmarshal the string using jsoniter
  if err := jsoniter.UnmarshalFromString(input, &event); err != nil {
    return nil, err
  }
  // We validate the struct using the `validate` struct tags.
  if err := parsers.ValidateStruct(&event); err != nil {
    return nil, err
  }

  result, err := p.BuildResult("Foo.Event", &event)
  if err != nil {
    return nil, err
  }

  return []*pantherlog.Result{result}, nil
}
```

That is still **a lot** of boilerplate for the simple case of parsing a JSON line containing a single log entry. The above code is an example that can be adapted to cover any case regardless of whether the logs are in JSON or whether we want to produce multiple events from a single log entry.

For the common case of a JSON log entry containing a single log event, Panther provides `logtypes.ConfigJSON` that automates the definition of the parser. In practice the code required to define such a log type would be much less as seen in the TL;DR section below.

### TL;DR Give me the code

```go
package foologs

import (
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/pantherlog"
  "github.com/panther-labs/panther/internal/log_analysis/log_processor/logtypes"
)

// FooEvent describes the schema of a Foo.Event JSON log entry.
type FooEvent struct {
    // We use `tcodec` struct tag to define the time format for the timestamp.
    // We use the `event_time` struct tag to mark a timestamp as the partitioning timestamp.
    // We use the `validate:"required"` struct tag to mark field as required.
    // This allows Panther to distinguish log entries of different log types from each other
    Time        pantherlog.Time    `json:"time" validate:"required" tcodec:"rfc3339" event_time:"true" description:"The foo event time"`
    // We use field types from the `pantherlog` package to handle JSON null values without the need for pointers and with minimum allocations.
    Message     pantherlog.String  `json:"message" validate:"required" description:"The foo event time"`
    // We use `panther` struct tags to mark strings that should be used as indicator values.
    RemoteIP    pantherlog.String  `json:"remote_ip" panther:"ip" description:"The remote ip used for the request"`
    ReferrerURL pantherlog.String  `json:"referrer_url" panther:"url" description:"The referrer URL used for the request"`
    RequestID   pantherlog.String  `json:"request_id" panther:"trace_id" description:"The id of the request that generated this event"`
    // We use a pointer to a struct (*User) to handle the case of a missing or null `user` field.
    User        *User              `json:"user" description:"The user that made the request"`
    Duration    pantherlog.Float64 `json:"duration_s" description:"The number of seconds it took to serve the request"`
}

type User struct {
  UID    pantherlog.Int64   `json:"uid" description:"The id of the user that made the request"`
  Groups []string           `json:"groups" description:"The groups the user is a member of"`
}

// LogTypes exports a group of all log type entries defined in this module.
// Panther scans for this function signature and includes these log types at runtime.
func LogTypes() logtypes.Group {
  return exportedFooLogTypes
}

// exportedFooLogTypes is a group of all log type entries exported by this module.
// By defining a variable at package level we ensure that we only build the entry once during init().
// By using `logtypes.Must` we make sure at compile time that all our log type entries are valid.
var exportedFooLogTypes = logtypes.Must("Foo", logtypes.ConfigJSON{
   Name: "Foo.Event",
   Description: "Foo log event",
   ReferenceURL: "https://example.com/logs/foo-event", // used in generated documentation
   NewEvent: func () interface{} {
     return &FooEvent{}
   },
})
```

By now our test should be passing, verifying that Panther can properly handle `Foo.Event` logs.

### Before making a pull-request

* Ensure your code is formatted, run `mage fmt`
* Ensure all tests pass `mage test:ci`
* Be sure to checkin the documentation that will be automatically generated and update the [SUMMARY.md](https://github.com/panther-labs/panther/blob/master/docs/gitbook/SUMMARY.md) if you added a new family of log types (enterprise-only).
* Deploy Panther and add a Source that uses the log types you defined. You should be able to see a new table with your added parser in Glue Data Catalog
* Do an end-to-end test. You can use [s3queue](../help/ops-home.md#tools) to copy test files into the `panther-bootstrap-auditlogs-<id>` bucket to drive log processing or use the development tool `./out/bin/devtools/<os>/<arch>/logprocessor` to read files from the local file system.
* Write a test rule for the new type to ensure data is flowing.
