# Salesforce

{% hint style="info" %}
Required fields are in **bold**.
{% endhint %}

## Salesforce.Login

Login events contain details about your org’s user login history.

Reference: [https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_login.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_login.htm)

| **`EVENT_TYPE`** | `string` | The type of event. The value is always Login. |
| :--- | :--- | :--- |
| `TIMESTAMP` | `timestamp` | The access time of Salesforce services in GMT. For example: 20130715233322.670. |
| `REQUEST_ID` | `string` | The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST\_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV. |
| **`ORGANIZATION_ID`** | `string` | The 15-character ID of the organization. For example: 00D000000000123. |
| `USER_ID` | `string` | The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943 |
| `RUN_TIME` | `bigint` | The amount of time that the request took in milliseconds. |
| `CPU_TIME` | `bigint` | The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer. |
| `URI` | `string` | The URI of the page that’s receiving the request. For example: /home/home.jsp. |
| `SESSION_KEY` | `string` | The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For Login Event Type, this field is usually null because the event is captured before a session is created. Example d7DEq/ANa7nNZZVD |
| `LOGIN_KEY` | `string` | The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I. |
| `REQUEST_STATUS` | `string` | The status of the request for a page view or user interface action. Possible values are:  S—Success. Salesforce handled the request successfully. If an Apex controller throws an exception, this status is also returned.  F—Failure. Typically 4xx or 5xx HTTP codes, such as no permission to view page, page took too long to render, page is read-only.  U—Undefined  A—Authorization Error  R—Redirect. Typically a 3xx HTTP code, possibly initiated by an Apex controller in a Visualforce page.  N—Not Found. 404 error. |
| `DB_TOTAL_TIME` | `bigint` | The time in nanoseconds for a database round trip. Includes time spent in the JDBC driver, network to the database, and DB\_CPU\_TIME. Compare this field to CPU\_TIME to determine whether performance issues are occurring in the database layer or in your own code. |
| `BROWSER_TYPE` | `string` | The identifier string returned by the browser used at login. Example values are:  Go-http-client/1.1  Mozilla/5.0 \(Macintosh; Intel Mac OS X 10.12; rv%3A50.0\) Gecko/20100101 Firefox/50.0  Mozilla/5.0 \(Macintosh; Intel Mac OS X 10\_11\_6\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/51.0.2704.84 Safari/537.36 |
| `API_TYPE` | `string` | The type of API request. Possible values are:  D—Apex Class  E—SOAP Enterprise  I—SOAP Cross Instance  M—SOAP Metadata  O—Old SOAP  P—SOAP Partner  S—SOAP Apex  T—SOAP Tooling  X—XmlRPC  f—Feed  l—Live Agent  p—SOAP ClientSync |
| `API_VERSION` | `string` | The version of the API that’s being used. For example: 36.0. |
| `USER_NAME` | `string` | The username that’s used for login. |
| `TLS_PROTOCOL` | `string` | The TLS protocol used for the login. There are 3 possible values: 1.0, 1.1, 1.2 |
| `CIPHER_SUITE` | `string` | The TLS cipher suite used for the login. Values are OpenSSL-style cipher suite names, with hyphen delimiters. For more information, see OpenSSL Cryptography and SSL/TLS Toolkit. |
| **`TIMESTAMP_DERIVED`** | `timestamp` | The access time of Salesforce services in ISO8601-compatible format \(YYYY-MM-DDTHH:MM:SS.sssZ\). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT. |
| `USER_ID_DERIVED` | `string` | The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0. |
| `CLIENT_IP` | `string` | The IP address of the client that’s using Salesforce services. A Salesforce internal IP \(such as a login from Salesforce Workbench or AppExchange\) is shown as "Salesforce.com IP". For example: 10.0.0.1. |
| `URI_ID_DERIVED` | `string` | The 18-character case insensitive ID of the URI of the page that’s receiving the request. |
| `LOGIN_STATUS` | `string` | The status of the login attempt. For successful logins, the value is LOGIN\_NO\_ERROR. All other values indicate errors or authentication issues. For details, see [https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_login\_status.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_login_status.htm) |
| `SOURCE_IP` | `string` | The source IP of the login request. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Salesforce.LoginAs

Login As events contain details about what a Salesforce admin did while logged in as another user.

Reference: [https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_loginas.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_loginas.htm)

| **`EVENT_TYPE`** | `string` | The type of event. The value is always LoginAs. |
| :--- | :--- | :--- |
| `TIMESTAMP` | `timestamp` | The access time of Salesforce services in GMT. For example: 20130715233322.670. |
| `REQUEST_ID` | `string` | The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST\_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV. |
| **`ORGANIZATION_ID`** | `string` | The 15-character ID of the organization. For example: 00D000000000123. |
| **`USER_ID`** | `string` | The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943 |
| `RUN_TIME` | `bigint` | The amount of time that the request took in milliseconds. |
| `CPU_TIME` | `bigint` | The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer. |
| `URI` | `string` | The URI of the page that’s receiving the request. For example: /home/home.jsp. |
| `SESSION_KEY` | `string` | The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For example: d7DEq/ANa7nNZZVD. |
| `LOGIN_KEY` | `string` | The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I. |
| `DELEGATED_USER_NAME` | `string` | The username of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation. |
| **`DELEGATED_USER_ID`** | `string` | The 15-character ID of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation. |
| **`TIMESTAMP_DERIVED`** | `timestamp` | The access time of Salesforce services in ISO8601-compatible format \(YYYY-MM-DDTHH:MM:SS.sssZ\). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT. |
| `USER_ID_DERIVED` | `string` | The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0. |
| `CLIENT_IP` | `string` | The IP address of the client that’s using Salesforce services. A Salesforce internal IP \(such as a login from Salesforce Workbench or AppExchange\) is shown as "Salesforce.com IP". For example: 10.0.0.1. |
| `URI_ID_DERIVED` | `string` | The 18-character case insensitive ID of the URI of the page that’s receiving the request. |
| `DELEGATED_USER_ID_DERIVED` | `string` | The 18-character case-insensitive ID of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |
| `p_any_usernames` | `[string]` | Panther added field with collection of usernames associated with the row |

## Salesforce.Logout

Logout events contain details of user logouts.

Reference: [https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_logout.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_logout.htm)

| **`EVENT_TYPE`** | `string` | The type of event. The value is always Logout. |
| :--- | :--- | :--- |
| `TIMESTAMP` | `timestamp` | The access time of Salesforce services in GMT. For example: 20130715233322.670. |
| `REQUEST_ID` | `string` | The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST\_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV. |
| **`ORGANIZATION_ID`** | `string` | The 15-character ID of the organization. For example: 00D000000000123. |
| **`USER_ID`** | `string` | The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943 |
| `USER_TYPE` | `string` | The category of user license of the user that logged out. Possible Values:  A: Automated Process  b: High Volume Portal  C: Customer Portal User  D: External Who  F: Self-Service  G: Guest  L: Package License Manager  N: Salesforce to Salesforce  n: CSN Only  O: Power Custom  o: Custom  P: Partner  p: Customer Portal Manager  S: Standard  X: Salesforce Administrator |
| `SESSION_TYPE` | `string` | The session type that was used when logging out. Possible Values:  A: API  I: APIOnlyUser  N: ChatterNetworks  Z: ChatterNetworksAPIOnly  C: Content  P: OauthApprovalUI  O: Oauth2  T: SiteStudio  R: SitePreview  S: SubstituteUser  B: TempContentExchange  G: TempOauthAccessTokenFrontdoor  Y: TempVisualforceExchange  F: TempUIFrontdoor  U: UI  E: UserSite  V: Visualforce  W: WDC\_API |
| `SESSION_LEVEL` | `string` | The security level of the session that was used when logging out. Possible Values: 1: Standard Session, 2: High-Assurance Session |
| `BROWSER_TYPE` | `string` | The identifier string returned by the browser used at login. Example values are:  Go-http-client/1.1  Mozilla/5.0 \(Macintosh; Intel Mac OS X 10.12; rv%3A50.0\) Gecko/20100101 Firefox/50.0  Mozilla/5.0 \(Macintosh; Intel Mac OS X 10\_11\_6\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/51.0.2704.84 Safari/537.36 |
| `PLATFORM_TYPE` | `bigint` | The code for the client platform. If a timeout caused the logout, this field is null. Example Values:  1000: Windows  2003: Macintosh/Apple OSX  5005: Android  5006: iPhone  5007: iPad |
| `RESOLUTION_TYPE` | `float` | The screen resolution of the client. If a timeout caused the logout, this field is null. |
| `APP_TYPE` | `string` | The application type that was in use upon logging out. Example Values:  1007: SFDC Application  1014: Chat  2501: CTI  2514: OAuth  3475: SFDC Partner Portal |
| `CLIENT_VERSION` | `float` | The version of the client that was in use upon logging out. |
| `API_TYPE` | `string` | The type of API request. Possible values are:  D—Apex Class  E—SOAP Enterprise  I—SOAP Cross Instance  M—SOAP Metadata  O—Old SOAP  P—SOAP Partner  S—SOAP Apex  T—SOAP Tooling  X—XmlRPC  f—Feed  l—Live Agent  p—SOAP ClientSync |
| `API_VERSION` | `string` | The version of the API that’s being used. For example: 36.0. |
| `USER_INITIATED_LOGOUT` | `boolean` | The value is 1 if the user intentionally logged out of the organization by clicking the Logout button. If the user’s session timed out due to inactivity or another implicit logout action, the value is 0. |
| `SESSION_KEY` | `string` | The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For example: d7DEq/ANa7nNZZVD. |
| `LOGIN_KEY` | `string` | The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I. |
| **`TIMESTAMP_DERIVED`** | `timestamp` | The access time of Salesforce services in ISO8601-compatible format \(YYYY-MM-DDTHH:MM:SS.sssZ\). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT. |
| `USER_ID_DERIVED` | `string` | The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0. |
| `CLIENT_IP` | `string` | The IP address of the client that’s using Salesforce services. A Salesforce internal IP \(such as a login from Salesforce Workbench or AppExchange\) is shown as "Salesforce.com IP". For example: 10.0.0.1. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

## Salesforce.URI

URI events contain details about user interaction with the web browser UI.

Reference: [https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_uri.htm](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_uri.htm)

| **`EVENT_TYPE`** | `string` | The type of event. The value is always URI. |
| :--- | :--- | :--- |
| `TIMESTAMP` | `timestamp` | The access time of Salesforce services in GMT. For example: 20130715233322.670. |
| `REQUEST_ID` | `string` | The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST\_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV. |
| **`ORGANIZATION_ID`** | `string` | The 15-character ID of the organization. For example: 00D000000000123. |
| `USER_ID` | `string` | The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943 |
| `RUN_TIME` | `bigint` | The amount of time that the request took in milliseconds. |
| `CPU_TIME` | `bigint` | The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer. |
| **`URI`** | `string` | The URI of the page that’s receiving the request. For more granular URI information for Lightning Experience and the Salesforce app, see the Lightning Error, Lightning Interaction, Lightning Page View, and Lightning Performance event types. Examples: /aura \(Lightning Experience\), /lightning \(Lightning Experience and the Salesforce app\), /home/home.jsp \(Salesforce Classic\) |
| `SESSION_KEY` | `string` | The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For Login Event Type, this field is usually null because the event is captured before a session is created. Example d7DEq/ANa7nNZZVD |
| `LOGIN_KEY` | `string` | The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I. |
| `REQUEST_STATUS` | `string` | The status of the request for a page view or user interface action. Possible values are:  S—Success. Salesforce handled the request successfully. If an Apex controller throws an exception, this status is also returned.  F—Failure. Typically 4xx or 5xx HTTP codes, such as no permission to view page, page took too long to render, page is read-only.  U—Undefined  A—Authorization Error  R—Redirect. Typically a 3xx HTTP code, possibly initiated by an Apex controller in a Visualforce page.  N—Not Found. 404 error. |
| `DB_TOTAL_TIME` | `bigint` | The time in nanoseconds for a database round trip. Includes time spent in the JDBC driver, network to the database, and DB\_CPU\_TIME. Compare this field to CPU\_TIME to determine whether performance issues are occurring in the database layer or in your own code. |
| `DB_BLOCKS` | `bigint` | Indicates how much activity is occurring in the database. A high value for this field suggests that adding indexes or filters on your queries would benefit performance. |
| `DB_CPU_TIME` | `bigint` | The CPU time in milliseconds to complete the request. Indicates the amount of activity taking place in the database layer during the request. |
| `REFERRER_URI` | `string` | The referring URI of the page that’s receiving the request. |
| **`TIMESTAMP_DERIVED`** | `timestamp` | The access time of Salesforce services in ISO8601-compatible format \(YYYY-MM-DDTHH:MM:SS.sssZ\). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT. |
| `USER_ID_DERIVED` | `string` | The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0. |
| `CLIENT_IP` | `string` | The IP address of the client that’s using Salesforce services. A Salesforce internal IP \(such as a login from Salesforce Workbench or AppExchange\) is shown as "Salesforce.com IP". For example: 10.0.0.1. |
| `URI_ID_DERIVED` | `string` | The 18-character case insensitive ID of the URI of the page that’s receiving the request. |
| **`p_event_time`** | `timestamp` | Panther added standardized event time \(UTC\) |
| **`p_parse_time`** | `timestamp` | Panther added standardized log parse time \(UTC\) |
| **`p_log_type`** | `string` | Panther added field with type of log |
| **`p_row_id`** | `string` | Panther added field with unique id \(within table\) |
| `p_source_id` | `string` | Panther added field with the source id |
| `p_source_label` | `string` | Panther added field with the source label |
| `p_any_ip_addresses` | `[string]` | Panther added field with collection of ip addresses associated with the row |
| `p_any_trace_ids` | `[string]` | Panther added field with collection of context trace identifiers |

