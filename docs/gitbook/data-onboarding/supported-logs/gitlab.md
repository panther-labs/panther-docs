# GitLab

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## GitLab.API

GitLab log for API requests received from GitLab. NOTE: We are using the latest version of GitLab API logs. Some fields differ from the official documentation Reference: [https://docs.gitlab.com/ee/administration/logs.html\#api\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#api_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`time`** | `timestamp` | The request timestamp |
| **`severity`** | `string` | The log level |
| **`duration_s`** | `float` | The time spent serving the request \(in seconds\) |
| `db_duration_s` | `float` | The time spent quering the database \(in seconds\) |
| `view_duration_s` | `float` | The time spent rendering the view for the Rails controller \(in seconds\) |
| **`status`** | `smallint` | The HTTP response status code |
| **`method`** | `string` | The HTTP method of the request |
| **`path`** | `string` | The URL path for the request |
| `params` | `[{   "key":string,   "value":string }]` | The URL query parameters |
| **`host`** | `string` | Hostname serving the request |
| `ua` | `string` | User-Agent HTTP header |
| **`route`** | `string` | Rails route for the API endpoint |
| `remote_ip` | `string` | The remote IP address of the HTTP request |
| `user_id` | `bigint` | The user id of the request |
| `username` | `string` | The username of the request |
| `gitaly_calls` | `bigint` | Total number of calls made to Gitaly |
| `gitaly_duration_s` | `float` | Total time taken by Gitaly calls |
| `redis_calls` | `bigint` | Total number of calls made to Redis |
| `redis_duration_s` | `float` | Total time to retrieve data from Redis |
| `correlation_id` | `string` | Request unique id across logs |
| `queue_duration_s` | `float` | Total time that the request was queued inside GitLab Workhorse |
| `meta_user` | `string` | User that invoked the request |
| `meta_project` | `string` | Project associated with the request |
| `meta_root_namespace` | `string` | Root namespace |
| `meta_caller_id` | `string` | Caller ID |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## GitLab.Audit

GitLab log file containing changes to group or project settings Reference: [https://docs.gitlab.com/ee/administration/logs.html\#audit\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#audit_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`severity`** | `string` | The log level |
| **`time`** | `timestamp` | The event timestamp |
| **`author_id`** | `bigint` | User id that made the change |
| **`entity_id`** | `bigint` | Id of the entity that was modified |
| **`entity_type`** | `string` | Type of the modified entity |
| **`change`** | `string` | Type of change to the settings |
| **`from`** | `string` | Old setting value |
| **`to`** | `string` | New setting value |
| **`author_name`** | `string` | Name of the user that made the change |
| **`target_id`** | `bigint` | Target id of the modified setting |
| **`target_type`** | `string` | Target type of the modified setting |
| **`target_details`** | `string` | Details of the target of the modified setting |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |

## GitLab.Exceptions

GitLab log file containing changes to group or project settings Reference: [https://docs.gitlab.com/ee/administration/logs.html\#exceptions\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#exceptions_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`severity`** | `string` | The log level |
| **`time`** | `timestamp` | The event timestamp |
| `correlation_id` | `string` | Request unique id across logs |
| `extra_server` | `{   "os":{     "name":string,     "version":string,     "build":string },   "runtime":{     "name":string,     "version":string } }` | Information about the server on which the exception occurred |
| `extra_project_id` | `bigint` | Project id where the exception occurred |
| `extra_relation_key` | `string` | Relation on which the exception occurred |
| `extra_relation_index` | `bigint` | Relation index on which the exception occurred |
| **`exception_class`** | `string` | Class name of the exception that occurred |
| **`exception_message`** | `string` | Message of the exception that occurred |
| `exception_backtrace` | `[string]` | Stack trace of the exception that occurred |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## GitLab.Git

GitLab log file containing all failed requests from GitLab to Git repositories. Reference: [https://docs.gitlab.com/ee/administration/logs.html\#git\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#git_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`severity`** | `string` | The log level |
| **`time`** | `timestamp` | The event timestamp |
| `correlation_id` | `string` | Unique id across logs |
| **`message`** | `string` | The error message from git |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## GitLab.Integrations

GitLab log with information about integrations activities such as Jira, Asana, and Irker services. Reference: [https://docs.gitlab.com/ee/administration/logs.html\#integrations\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#integrations_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`severity`** | `string` | The log level |
| **`time`** | `timestamp` | The event timestamp |
| **`service_class`** | `string` | The class name of the integrated service |
| **`project_id`** | `bigint` | The project id the integration was running on |
| **`project_path`** | `string` | The project path the integration was running on |
| **`message`** | `string` | The log message from the service |
| **`client_url`** | `string` | The client url of the service |
| `error` | `string` | The error name if an error has occurred |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_domain_names` | `[string]` | Panther added field with collection of domain names associated with the row |

## GitLab.Production

GitLab log for Production controller requests received from GitLab Reference: [https://docs.gitlab.com/ee/administration/logs.html\#production\_jsonlog](https://docs.gitlab.com/ee/administration/logs.html#production_jsonlog)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`method`** | `string` | The HTTP method of the request |
| **`path`** | `string` | The URL path for the request |
| `format` | `string` | The response output format |
| `controller` | `string` | The Production controller class name |
| `action` | `string` | The Production controller action |
| **`status`** | `bigint` | The HTTP response status code |
| **`time`** | `timestamp` | The request timestamp |
| `params` | `[{   "key":string,   "value":string }]` | The URL query parameters |
| `remote_ip` | `string` | The remote IP address of the HTTP request |
| `user_id` | `bigint` | The user id of the request |
| `username` | `string` | The username of the request |
| `ua` | `string` | The User-Agent of the requester |
| `queue_duration_s` | `float` | Total time that the request was queued inside GitLab Workhorse |
| `gitaly_calls` | `bigint` | Total number of calls made to Gitaly |
| `gitaly_duration_s` | `float` | Total time taken by Gitaly calls |
| `redis_calls` | `bigint` | Total number of calls made to Redis |
| `redis_duration_s` | `float` | Total time to retrieve data from Redis |
| `redis_read_bytes` | `bigint` | Total bytes read from Redis |
| `redis_write_bytes` | `bigint` | Total bytes written to Redis |
| `correlation_id` | `string` | Request unique id across logs |
| `cpu_s` | `float` | Total time spent on CPU |
| `db_duration_s` | `float` | Total time to retrieve data from PostgreSQL |
| `view_duration_s` | `float` | Total time taken inside the Rails views |
| **`duration_s`** | `float` | Total time taken to retrieve the request |
| `meta_caller_id` | `string` | Caller ID |
| `location` | `string` | \(Applies only to redirects\) The redirect URL |
| `exception_class` | `string` | Class name of the exception that occurred |
| `exception_message` | `string` | Message of the exception that occurred |
| `exception_backtrace` | `[string]` | Stack trace of the exception that occurred |
| `etag_route` | `string` | Route name etag \(on redirects\) |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

