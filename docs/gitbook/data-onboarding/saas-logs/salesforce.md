---
description: Panther supports pulling logs directly from Salesforce
---

# Salesforce

## Overview

Panther has the ability to fetch [Salesforce Event Monitoring](https://trailhead.salesforce.com/content/learn/modules/event\_monitoring/event\_monitoring\_intro) logs for the following event types:

* [Login](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_login.htm)
* [LoginAs](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_loginas.htm)
* [Logout](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_logout.htm)
* [URI](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce\_api\_objects\_eventlogfile\_uri.htm)

{% hint style="info" %}
Salesforce customers must [enable Event Monitoring](https://help.salesforce.com/articleView?id=000339868\&type=1\&mode=1) before onboarding logs to Panther. An additional license may be required for this Salesforce add-on.
{% endhint %}

## How to onboard Salesforce logs to Panther

### Step 1: Create an API User in Salesforce

{% hint style="info" %}
In order to create and add permissions to the new user, the ['Manage Users' permission](https://help.salesforce.com/articleView?id=000324398\&type=1\&mode=1) is required.
{% endhint %}

Panther requires a user account with API and Event Log File permissions in order to retrieve Event Monitoring logs.

We recommend creating a new, dedicated user with the minimum permissions required by Panther. Salesforce requires each user to have a [unique username](https://help.salesforce.com/articleView?id=sf.basics\_intro\_usernames\_passwords.htm\&type=5), but the same email address can be included for multiple users. Thus, you can create a Panther-only account without having to manage an additional email address in your organization.

**To create a user**:

1. Follow the instructions in the [Salesforce documentation](https://help.salesforce.com/articleView?id=sf.adding\_new\_users.htm\&type=5) to add a new user.&#x20;
   * For User License, select "Salesforce."
   * For Profile, select "Read Only."&#x20;
2. Complete the user registration process by setting a new password through the link sent to your email.

### Step 2: Retrieve Security Token from Salesforce API <a href="#retrieve-security-token" id="retrieve-security-token"></a>

Salesforce API access requires username, password, and a credential called a _**security token**_.

To request a security token for a new Salesforce user account, follow the instructions [in this Salesforce documentation](https://help.salesforce.com/s/articleView?id=sf.user\_security\_token.htm\&type=5). The new security token is sent to the email address in your Salesforce personal settings.

### Step 3: Create and assign a new Permission Set in Salesforce

To assign permissions to the new user, you must create a new [Permission Set](https://help.salesforce.com/articleView?id=perm\_sets\_overview.htm\&type=5).&#x20;

1. Follow the instructions in Salesforce's [Create Permission Sets documentation](https://help.salesforce.com/s/articleView?id=sf.perm\_sets\_create.htm\&type=5) to add a new permission set that grants Panther access to the Event Monitoring data via the SOAP/REST API.
2. On your new Permisson Set's page, click **System Permissions:**\
   ****![](../../.gitbook/assets/sfdc-system-permissions.png)****
3. Click **Edit**, then check the boxes to enable the following permissions:
   * API Enabled
   * View Event Log Files
4. Assign the Permission Set to the designated user by following the instructions in Salesforce's documentation: [Assign Permission Sets to a Single User](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/perm\_sets\_assigning.htm).&#x20;

### Step 4: Create a new Salesforce source in Panther

1. Log in to your Panther Console.
2. Go to **Integrations** > **Log Sources.**
3. Click **Create New**.
4. Select **Salesforce** from the list of available sources.
5. Click **Start Source Setup**.
6. On the Configure Source page, fill in the form:
   * Enter a memorable name for the source, e.g. `Salesforce Logs`.
   * Select the time interval (hourly or daily) in which you want files retrieved from Salesforce&#x20;
     * Check with your Salesforce admin to determine how your Salesforce instance is configured and which file interval is supported.
   * Select which log types you would like to monitor.
7. Click **Continue Setup**.
8. Enter the credentials of the account that Panther will use to connect to the Salesforce API:
   * **Account Username**: Enter your Salesforce account username, e.g., `panther-logs@mycompany.com`.
   * **Account Password**: Enter your Salesforce account password.
   * **Security Token**: Enter the the [Security Token](salesforce.md#retrieve-security-token) that you obtained earlier in this documentation.&#x20;
9. Click **Continue Setup**.&#x20;
10. You will be directed to a confirmation screen where you can set up a log drop-off alarm.
    * This feature sends an error message if logs aren't received within a specified time interval.
11. Click **Finish Setup**.

## Supported log types

{% hint style="info" %}
Required fields in the schemas are listed as **"required: true"** just below the "name" field.
{% endhint %}

### Salesforce.Login

Login events contain details about your org’s user login history.

Reference: [Salesforce Documentation on Login Event Types.](https://developer.salesforce.com/docs/atlas.en-us.object\_reference.meta/object\_reference/sforce\_api\_objects\_eventlogfile\_login.htm)

```yaml
schema: Salesforce.Login
version: 0
referenceURL: https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_login.htm
description: 'Login events contain details about your org’s user login history.'
parser:
    csv:
        delimiter: ','
        hasHeader: true
        columns:
            - EVENT_TYPE
            - TIMESTAMP
            - REQUEST_ID
            - ORGANIZATION_ID
            - USER_ID
            - RUN_TIME
            - CPU_TIME
            - URI
            - SESSION_KEY
            - LOGIN_KEY
            - USER_TYPE
            - REQUEST_STATUS
            - DB_TOTAL_TIME
            - BROWSER_TYPE
            - API_TYPE
            - API_VERSION
            - USER_NAME
            - TLS_PROTOCOL
            - CIPHER_SUITE
            - AUTHENTICATION_METHOD_REFERENCE
            - TIMESTAMP_DERIVED
            - USER_ID_DERIVED
            - CLIENT_IP
            - URI_ID_DERIVED
            - LOGIN_STATUS
            - SOURCE_IP
fields:
    - name: EVENT_TYPE
      type: string
      required: true
      validate:
        allow: ['Login']
      description: The type of event. The value is always Login.
    - name: TIMESTAMP
      required: false
      type: timestamp
      timeFormat: '%Y%m%d%H%M%S.%f'
      description: 'The access time of Salesforce services in GMT. For example: 20130715233322.670.'
    - name: REQUEST_ID
      required: false
      type: string
      indicators:
        - trace_id
      description: >-
        The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV.
    - name: ORGANIZATION_ID
      required: true
      type: string
      description: 'The 15-character ID of the organization. For example: 00D000000000123.'
    - name: USER_ID
      required: false
      type: string
      description: >-
        The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943
    - name: RUN_TIME
      required: false
      type: bigint
      description: The amount of time that the request took in milliseconds.
    - name: CPU_TIME
      required: false
      type: bigint
      description: >-
        The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer.
    - name: URI
      required: false
      type: string
      description: 'The URI of the page that’s receiving the request. For example: /home/home.jsp.'
    - name: SESSION_KEY
      required: false
      type: string
      description: >-
        The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For Login Event Type, this field is usually null because the event is captured before a session is created. Example d7DEq/ANa7nNZZVD
    - name: LOGIN_KEY
      required: false
      type: string
      description: >-
        The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I.
    - name: REQUEST_STATUS
      required: false
      type: string
      description: >-
        The status of the request for a page view or user interface action. Possible values are:





















          S—Success. Salesforce handled the request successfully. If an Apex controller throws an exception, this status is also returned.
          F—Failure. Typically 4xx or 5xx HTTP codes, such as no permission to view page, page took too long to render, page is read-only.
          U—Undefined
          A—Authorization Error
          R—Redirect. Typically a 3xx HTTP code, possibly initiated by an Apex controller in a Visualforce page.
          N—Not Found. 404 error.
    - name: DB_TOTAL_TIME
      required: false
      type: bigint
      description: >-
        The time in nanoseconds for a database round trip. Includes time spent in the JDBC driver, network to the database, and DB_CPU_TIME. Compare this field to CPU_TIME to determine whether performance issues are occurring in the database layer or in your own code.
    - name: BROWSER_TYPE
      required: false
      type: string
      description: >-
        The identifier string returned by the browser used at login. Example values are:





















            Go-http-client/1.1
            Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv%3A50.0) Gecko/20100101 Firefox/50.0
            Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.84 Safari/537.36
    - name: API_TYPE
      required: false
      type: string
      description: >-
        The type of API request. Possible values are:





















            D—Apex Class
            E—SOAP Enterprise
            I—SOAP Cross Instance
            M—SOAP Metadata
            O—Old SOAP
            P—SOAP Partner
            S—SOAP Apex
            T—SOAP Tooling
            X—XmlRPC
            f—Feed
            l—Live Agent
            p—SOAP ClientSync
    - name: API_VERSION
      required: false
      type: string
      description: 'The version of the API that’s being used. For example: 36.0.'
    - name: USER_NAME
      required: false
      type: string
      description: The username that’s used for login.
      indicators:
        - username
    - name: TLS_PROTOCOL
      required: false
      type: string
      description: 'The TLS protocol used for the login. There are 3 possible values: 1.0, 1.1, 1.2'
    - name: CIPHER_SUITE
      required: false
      type: string
      description: >-
        The TLS cipher suite used for the login. Values are OpenSSL-style cipher suite names, with hyphen delimiters. For more information, see OpenSSL Cryptography and SSL/TLS Toolkit.
    - name: TIMESTAMP_DERIVED
      required: true
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
      description: >-
        The access time of Salesforce services in ISO8601-compatible format (YYYY-MM-DDTHH:MM:SS.sssZ). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT.
    - name: USER_ID_DERIVED
      required: false
      type: string
      description: >-
        The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0.
    - name: CLIENT_IP
      required: false
      type: string
      indicators:
        - ip
      description: >-
        The IP address of the client that’s using Salesforce services. A Salesforce internal IP (such as a login from Salesforce Workbench or AppExchange) is shown as "Salesforce.com IP". For example: 10.0.0.1.
    - name: URI_ID_DERIVED
      required: false
      type: string
      description: The 18-character case insensitive ID of the URI of the page that’s receiving the request.
    - name: LOGIN_STATUS
      required: false
      type: string
      description: >-
        The status of the login attempt. For successful logins, the value is LOGIN_NO_ERROR. All other values indicate errors or authentication issues. For details, see https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_login_status.htm
    - name: SOURCE_IP
      required: false
      type: string
      indicators:
        - ip
      description: The source IP of the login request.
    - name: AUTHENTICATION_METHOD_REFERENCE
      type: string
      description: >-
        The authentication method used by a third-party identification provider for an OpenID Connect single sign-on protocol. This field is available in API version 51.0 and later.
    - name: USER_TYPE
      type: string
      description: >-
        The category of user license. Possible values are:





















            CsnOnly — Users whose access to the application is limited to Chatter. This user type includes Chatter Free and Chatter moderator users.
            CspLitePortal — CSP Lite Portal license. Users whose access is limited because they’re organization customers and access the application through a customer portal or an Experience Cloud site.
            CustomerSuccess — Customer Success license. Users whose access is limited because they’re organization customers and access the application through a customer portal.
            Guest — Users whose access is limited so that your customers can view and interact with your site without logging in.
            PowerCustomerSuccess — Power Customer Success license. Users whose access is limited because they’re organization customers and access the application through a customer portal. Users with this license type can view and edit data they directly own or data owned by or shared with users below them in the customer portal role hierarchy.
            PowerPartner — Power Partner license. Users whose access is limited because they’re partners and typically access the application through a partner portal or site.
            SelfService — Users whose access is limited because they’re organization customers and access the application through a self-service portal.
            Standard — Standard user license. This user type also includes Salesforce Platform and Salesforce Platform One user licenses, and admins for this org.
```

### Salesforce.LoginAs

Login As events contain details about what a Salesforce admin did while logged in as another user.

Reference: [Salesforce Documentation on Login As Event Types.](https://developer.salesforce.com/docs/atlas.en-us.object\_reference.meta/object\_reference/sforce\_api\_objects\_eventlogfile\_loginas.htm)

```yaml
schema: Salesforce.LoginAs
version: 0
referenceURL: https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_loginas.htm
description: 'Login As events contain details about what a Salesforce admin did while logged in as another user.'
parser:
    csv:
        delimiter: ','
        hasHeader: true
        columns:
            - EVENT_TYPE
            - TIMESTAMP
            - REQUEST_ID
            - ORGANIZATION_ID
            - USER_ID
            - RUN_TIME
            - CPU_TIME
            - URI
            - SESSION_KEY
            - LOGIN_KEY
            - DELEGATED_USER_NAME
            - DELEGATED_USER_ID
            - TIMESTAMP_DERIVED
            - USER_ID_DERIVED
            - CLIENT_IP
            - URI_ID_DERIVED
            - DELEGATED_USER_ID_DERIVED
fields:
    - name: EVENT_TYPE
      required: true
      type: string
      validate:
        allow: ['LoginAs']
      description: The type of event. The value is always LoginAs.
    - name: TIMESTAMP
      required: false
      type: timestamp
      timeFormat: '%Y%m%d%H%M%S.%f'
      description: 'The access time of Salesforce services in GMT. For example: 20130715233322.670.'
    - name: REQUEST_ID
      required: false
      type: string
      description: >-
        The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV.
      indicators:
        - trace_id
    - name: ORGANIZATION_ID
      required: true
      type: string
      description: 'The 15-character ID of the organization. For example: 00D000000000123.'
    - name: USER_ID
      required: true
      type: string
      description: >-
        The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943
    - name: RUN_TIME
      required: false
      type: bigint
      description: The amount of time that the request took in milliseconds.
    - name: CPU_TIME
      required: false
      type: bigint
      description: >-
        The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer.
    - name: URI
      required: false
      type: string
      description: 'The URI of the page that’s receiving the request. For example: /home/home.jsp.'
    - name: SESSION_KEY
      required: false
      type: string
      description: >-
        The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For example: d7DEq/ANa7nNZZVD.
    - name: LOGIN_KEY
      required: false
      type: string
      description: >-
        The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I.
    - name: DELEGATED_USER_NAME
      required: false
      type: string
      description: >-
        The username of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation.
      indicators:
        - username
    - name: DELEGATED_USER_ID
      required: true
      type: string
      description: >-
        The 15-character ID of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation.
    - name: TIMESTAMP_DERIVED
      required: true
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
      description: >-
        The access time of Salesforce services in ISO8601-compatible format (YYYY-MM-DDTHH:MM:SS.sssZ). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT.
    - name: USER_ID_DERIVED
      required: false
      type: string
      description: >-
        The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0.
    - name: CLIENT_IP
      required: false
      type: string
      indicators:
        - ip
      description: >-
        The IP address of the client that’s using Salesforce services. A Salesforce internal IP (such as a login from Salesforce Workbench or AppExchange) is shown as "Salesforce.com IP". For example: 10.0.0.1.
    - name: URI_ID_DERIVED
      required: false
      type: string
      description: The 18-character case insensitive ID of the URI of the page that’s receiving the request.
    - name: DELEGATED_USER_ID_DERIVED
      required: false
      type: string
      description: >-
        The 18-character case-insensitive ID of the user who’s using Salesforce services through the UI or API. In this case, the user who’s doing the impersonation.
```

### Salesforce.Logout

Logout events contain details of user logouts.

Reference: [Salesforce Documentation on Logout Event Types.](https://developer.salesforce.com/docs/atlas.en-us.object\_reference.meta/object\_reference/sforce\_api\_objects\_eventlogfile\_logout.htm)

```yaml
schema: Salesforce.Logout
version: 0
referenceURL: https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_logout.htm
description: Logout events contain details of user logouts.
parser:
    csv:
        delimiter: ','
        hasHeader: true
        columns:
            - EVENT_TYPE
            - TIMESTAMP
            - REQUEST_ID
            - ORGANIZATION_ID
            - USER_ID
            - USER_TYPE
            - SESSION_TYPE
            - SESSION_LEVEL
            - BROWSER_TYPE
            - PLATFORM_TYPE
            - RESOLUTION_TYPE
            - APP_TYPE
            - CLIENT_VERSION
            - API_TYPE
            - API_VERSION
            - USER_INITIATED_LOGOUT
            - SESSION_KEY
            - LOGIN_KEY
            - TIMESTAMP_DERIVED
            - USER_ID_DERIVED
            - CLIENT_IP
fields:
    - name: EVENT_TYPE
      required: true
      type: string
      validate:
        allow: ['Logout']
      description: The type of event. The value is always Logout.
    - name: TIMESTAMP
      required: false
      type: timestamp
      timeFormat: '%Y%m%d%H%M%S.%f'
      description: 'The access time of Salesforce services in GMT. For example: 20130715233322.670.'
    - name: REQUEST_ID
      required: false
      type: string
      indicators:
        - trace_id
      description: >-
        The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV.
    - name: ORGANIZATION_ID
      required: true
      type: string
      description: 'The 15-character ID of the organization. For example: 00D000000000123.'
    - name: USER_ID
      required: true
      type: string
      description: >-
        The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943
    - name: USER_TYPE
      required: false
      type: string
      description: >-
        The category of user license of the user that logged out. Possible Values:





















          A: Automated Process
          b: High Volume Portal
          C: Customer Portal User
          D: External Who
          F: Self-Service
          G: Guest
          L: Package License Manager
          N: Salesforce to Salesforce
          n: CSN Only
          O: Power Custom
          o: Custom
          P: Partner
          p: Customer Portal Manager
          S: Standard
          X: Salesforce Administrator
    - name: SESSION_TYPE
      required: false
      type: string
      description: >-
        The session type that was used when logging out. Possible Values:





















          A: API
          I: APIOnlyUser
          N: ChatterNetworks
          Z: ChatterNetworksAPIOnly
          C: Content
          P: OauthApprovalUI
          O: Oauth2
          T: SiteStudio
          R: SitePreview
          S: SubstituteUser
          B: TempContentExchange
          G: TempOauthAccessTokenFrontdoor
          Y: TempVisualforceExchange
          F: TempUIFrontdoor
          U: UI
          E: UserSite
          V: Visualforce
          W: WDC_API
    - name: SESSION_LEVEL
      required: false
      type: string
      description: >-
        The security level of the session that was used when logging out. Possible Values: 1: Standard Session, 2: High-Assurance Session
    - name: BROWSER_TYPE
      required: false
      type: string
      description: >-
        The identifier string returned by the browser used at login. Example values are:





















            Go-http-client/1.1
            Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv%3A50.0) Gecko/20100101 Firefox/50.0
            Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.84 Safari/537.36
    - name: PLATFORM_TYPE
      required: false
      type: bigint
      description: >-
        The code for the client platform. If a timeout caused the logout, this field is null. Example Values:





















          1000: Windows
          2003: Macintosh/Apple OSX
          5005: Android
          5006: iPhone
          5007: iPad
    - name: RESOLUTION_TYPE
      required: false
      type: float
      description: The screen resolution of the client. If a timeout caused the logout, this field is null.
    - name: APP_TYPE
      required: false
      type: string
      description: >-
        The application type that was in use upon logging out. Example Values:





















          1007: SFDC Application
          1014: Chat
          2501: CTI
          2514: OAuth
          3475: SFDC Partner Portal
    - name: CLIENT_VERSION
      required: false
      type: float
      description: The version of the client that was in use upon logging out.
    - name: API_TYPE
      required: false
      type: string
      description: >-
        The type of API request. Possible values are:





















            D—Apex Class
            E—SOAP Enterprise
            I—SOAP Cross Instance
            M—SOAP Metadata
            O—Old SOAP
            P—SOAP Partner
            S—SOAP Apex
            T—SOAP Tooling
            X—XmlRPC
            f—Feed
            l—Live Agent
            p—SOAP ClientSync
    - name: API_VERSION
      required: false
      type: string
      description: 'The version of the API that’s being used. For example: 36.0.'
    - name: USER_INITIATED_LOGOUT
      required: false
      type: boolean
      description: >-
        The value is 1 if the user intentionally logged out of the organization by clicking the Logout button. If the user’s session timed out due to inactivity or another implicit logout action, the value is 0.
    - name: SESSION_KEY
      required: false
      type: string
      description: >-
        The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For example: d7DEq/ANa7nNZZVD.
    - name: LOGIN_KEY
      required: false
      type: string
      description: >-
        The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I.
    - name: TIMESTAMP_DERIVED
      required: true
      type: timestamp
      isEventTime: true
      timeFormat: rfc3339
      description: >-
        The access time of Salesforce services in ISO8601-compatible format (YYYY-MM-DDTHH:MM:SS.sssZ). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT.
    - name: USER_ID_DERIVED
      required: false
      type: string
      description: >-
        The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0.
    - name: CLIENT_IP
      required: false
      type: string
      indicators:
        - ip
      description: >-
        The IP address of the client that’s using Salesforce services. A Salesforce internal IP (such as a login from Salesforce Workbench or AppExchange) is shown as "Salesforce.com IP". For example: 10.0.0.1.
```

### Salesforce.URI

URI events contain details about user interaction with the web browser UI.

Reference: [Salesforce Documentation on URI Event Types.](https://developer.salesforce.com/docs/atlas.en-us.object\_reference.meta/object\_reference/sforce\_api\_objects\_eventlogfile\_uri.htm)

```yaml
schema: Salesforce.URI
version: 0
referenceURL: https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_eventlogfile_uri.htm
description: 'URI events contain details about user interaction with the web browser UI.'
parser:
    csv:
        delimiter: ','
        hasHeader: true
        columns:
            - EVENT_TYPE
            - TIMESTAMP
            - REQUEST_ID
            - ORGANIZATION_ID
            - USER_ID
            - RUN_TIME
            - CPU_TIME
            - URI
            - SESSION_KEY
            - LOGIN_KEY
            - REQUEST_STATUS
            - DB_TOTAL_TIME
            - DB_BLOCKS
            - DB_CPU_TIME
            - REFERRER_URI
            - TIMESTAMP_DERIVED
            - USER_ID_DERIVED
            - CLIENT_IP
            - URI_ID_DERIVED
            - USER_TYPE
fields:
    - name: EVENT_TYPE
      required: true
      type: string
      validate:
        allow: ['URI']
      description: The type of event. The value is always URI.
    - name: TIMESTAMP
      required: false
      type: timestamp
      timeFormat: '%Y%m%d%H%M%S.%f'
      description: 'The access time of Salesforce services in GMT. For example: 20130715233322.670.'
    - name: REQUEST_ID
      required: false
      type: string
      indicators:
        - trace_id
      description: >-
        The unique ID of a single transaction. A transaction can contain one or more events. Each event in a given transaction has the same REQUEST_ID. For example: 3nWgxWbDKWWDIk0FKfF5DV.
    - name: ORGANIZATION_ID
      required: true
      type: string
      description: 'The 15-character ID of the organization. For example: 00D000000000123.'
    - name: USER_ID
      required: false
      type: string
      description: >-
        The 15-character ID of the user who’s using Salesforce services through the UI or the API. For example: 00530000009M943
    - name: RUN_TIME
      required: false
      type: bigint
      description: 'The amount of time that the request took in milliseconds.'
    - name: CPU_TIME
      required: false
      type: bigint
      description: >-
        The CPU time in milliseconds used to complete the request. This field indicates the amount of activity taking place in the app server layer.
    - name: URI
      required: true
      type: string
      description: >-
        The URI of the page that’s receiving the request. For more granular URI information for Lightning Experience and the Salesforce app, see the Lightning Error, Lightning Interaction, Lightning Page View, and Lightning Performance event types. Examples: /aura (Lightning Experience), /lightning (Lightning Experience and the Salesforce app), /home/home.jsp (Salesforce Classic)
    - name: SESSION_KEY
      required: false
      type: string
      description: >-
        The user’s unique session ID. You can use this value to identify all user events within a session. When a user logs out and logs in again, a new session is started. For Login Event Type, this field is usually null because the event is captured before a session is created. Example d7DEq/ANa7nNZZVD
    - name: LOGIN_KEY
      required: false
      type: string
      description: >-
        The string that ties together all events in a given user’s login session. It starts with a login event and ends with either a logout event or the user session expiring. For example: GeJCsym5eyvtEK2I.
    - name: REQUEST_STATUS
      required: false
      type: string
      description: >-
        The status of the request for a page view or user interface action. Possible values are:





















          S—Success. Salesforce handled the request successfully. If an Apex controller throws an exception, this status is also returned.
          F—Failure. Typically 4xx or 5xx HTTP codes, such as no permission to view page, page took too long to render, page is read-only.
          U—Undefined
          A—Authorization Error
          R—Redirect. Typically a 3xx HTTP code, possibly initiated by an Apex controller in a Visualforce page.
          N—Not Found. 404 error.
    - name: DB_TOTAL_TIME
      required: false
      type: bigint
      description: >-
        The time in nanoseconds for a database round trip. Includes time spent in the JDBC driver, network to the database, and DB_CPU_TIME. Compare this field to CPU_TIME to determine whether performance issues are occurring in the database layer or in your own code.
    - name: DB_BLOCKS
      required: false
      type: bigint
      description: >-
        Indicates how much activity is occurring in the database. A high value for this field suggests that adding indexes or filters on your queries would benefit performance.
    - name: DB_CPU_TIME
      required: false
      type: bigint
      description: >-
        The CPU time in milliseconds to complete the request. Indicates the amount of activity taking place in the database layer during the request.
    - name: REFERRER_URI
      required: false
      type: string
      description: The referring URI of the page that’s receiving the request.
    - name: TIMESTAMP_DERIVED
      required: true
      type: timestamp
      timeFormat: rfc3339
      isEventTime: true
      description: >-
        The access time of Salesforce services in ISO8601-compatible format (YYYY-MM-DDTHH:MM:SS.sssZ). For example: 2015-07-27T11:32:59.555Z. Timezone is GMT.
    - name: USER_ID_DERIVED
      required: false
      type: string
      description: >-
        The 18-character case insensitive ID of the user who’s using Salesforce services through the UI or the API. For example: 00590000000I1SNIA0.
    - name: CLIENT_IP
      required: false
      type: string
      indicators:
        - ip
      description: >-
        The IP address of the client that’s using Salesforce services. A Salesforce internal IP (such as a login from Salesforce Workbench or AppExchange) is shown as "Salesforce.com IP". For example: 10.0.0.1.
    - name: URI_ID_DERIVED
      required: false
      type: string
      description: The 18-character case insensitive ID of the URI of the page that’s receiving the request.
    - name: USER_TYPE
      type: string
      description: >-
        The category of user license. Possible values are:





















            CsnOnly — Users whose access to the application is limited to Chatter. This user type includes Chatter Free and Chatter moderator users.
            CspLitePortal — CSP Lite Portal license. Users whose access is limited because they’re organization customers and access the application through a customer portal or an Experience Cloud site.
            CustomerSuccess — Customer Success license. Users whose access is limited because they’re organization customers and access the application through a customer portal.
            Guest — Users whose access is limited so that your customers can view and interact with your site without logging in.
            PowerCustomerSuccess — Power Customer Success license. Users whose access is limited because they’re organization customers and access the application through a customer portal. Users with this license type can view and edit data they directly own or data owned by or shared with users below them in the customer portal role hierarchy.
            PowerPartner — Power Partner license. Users whose access is limited because they’re partners and typically access the application through a partner portal or site.
            SelfService — Users whose access is limited because they’re organization customers and access the application through a self-service portal.
            Standard — Standard user license. This user type also includes Salesforce Platform and Salesforce Platform One user licenses, and admins for this org.
```
