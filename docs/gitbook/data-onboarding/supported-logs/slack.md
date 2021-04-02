# Slack

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Slack.AccessLogs

Access logs for users on a Slack workspace. Note: Due to Slack's rate limits, Panther pulls only the events where the user or the access location or the access device is new. Panther will not update the `date_last`, `count` fields of an event. Reference: [https://api.slack.com/methods/team.accessLogs](https://api.slack.com/methods/team.accessLogs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`user_id`** | `string` | The id of the user accessing Slack. |
| `username` | `string` | The username of the user accessing Slack. |
| **`date_first`** | `timestamp` | Unix timestamp of the first access log entry for this user, IP address, and user agent combination. |
| **`date_last`** | `timestamp` | Unix timestamp of the most recent access log entry for this user, IP address, and user agent combination. Note: Panther will not update this field even if it is updated in the Slack API. |
| **`count`** | `bigint` | The total number of access log entries for that combination. Note: Panther will not update this field even if it is updated in the Slack API. |
| **`ip`** | `string` | The IP address of the device used to access Slack. |
| `user_agent` | `string` | The reported user agent string from the browser or client application. |
| `isp` | `string` | Best guess at the internet service provider owning the IP address. |
| `country` | `string` | Best guesses on where the access originated, based on the IP address. |
| `region` | `string` | Best guesses on where the access originated, based on the IP address. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Slack.AuditLogs

Slack audit logs provide a view of the actions users perform in an Enterprise Grid organization. Reference: [https://api.slack.com/enterprise/audit-logs](https://api.slack.com/enterprise/audit-logs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`id`** | `string` | The event id |
| **`date_create`** | `timestamp` | Creation timestamp for the event |
| **`action`** | `string` | The action performed. See https://api.slack.com/enterprise/audit-logs\#audit\_logs\_actions |
| **`actor`** | `{   "type":string,   "user":{     "id":string,     "name":string,     "email":string,     "team":string } }` | An actor will always be a user on a workspace and will be identified by their user ID, such as W123AB456. |
| **`entity`** | `{   "type":string,   "user":{     "id":string,     "name":string,     "email":string,     "team":string },   "channel":{     "id":string,     "name":string,     "privacy":string,     "is_shared":boolean,     "is_org_shared":boolean,     "teams_shared_with":[string] },   "file":{     "id":string,     "name":string,     "title":string,     "filetype":string },   "app":{     "id":string,     "name":string,     "is_distributed":boolean,     "is_directory_approved":boolean,     "scopes":[string] },   "workspace":{     "id":string,     "name":string,     "domain":string },   "enterprise":{     "id":string,     "name":string,     "domain":string },   "workflow":{     "id":string,     "name":string },   "message":{     "team":string,     "channel":string,     "timestamp":string } }` | An entity is the thing that the actor has taken the action upon and it will be the Slack ID of the thing. |
| **`context`** | `{   "ua":string,   "ip_address":string,   "location":{     "type":string,     "id":string,     "domain":string,     "name":string } }` | Context is the location that the actor took the action on the entity. It will always be either a Workspace or an Enterprise, with the appropriate ID. |
| `details` | `string` | Additional details about the audit log event |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_emails` | `[string]` | Panther added field with collection of email addresses associated with the row |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Slack.IntegrationLogs

Integration activity logs for a team, including when integrations are added, modified and removed. Reference: [https://api.slack.com/methods/team.integrationLogs](https://api.slack.com/methods/team.integrationLogs)

| Column | Type | Description |
| :--- | :--- | :--- |
| **`user_id`** | `string` | The id of the user performing the action. |
| `user_name` | `string` | The username of the user performing the action. |
| `service_id` | `string` | The service id for which this log is about. |
| `service_type` | `string` | The service type for which this log is about. |
| `app_id` | `string` | The app id for which this log is about. |
| `app_type` | `string` | The app type for which this log is about. |
| **`date`** | `timestamp` | The date when the action happened. |
| **`change_type`** | `string` | The type of this action \(added, removed, enabled, disabled, updated\). |
| **`scope`** | `string` | The scope used for this action. |
| `channel` | `string` | The related channel. |
| `reason` | `string` | The reason of the disable action, populated if this event refers to such an action. |
| `rss_feed` | `boolean` | True if this log entry is an RSS feed. If true, more RSS feed related fields will be present. |
| `rss_feed_change_type` | `boolean` | The change type for the RSS feed. |
| `rss_feed_title` | `boolean` | The title of the RSS feed. |
| `rss_feed_url` | `boolean` | The url of the RSS feed. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

