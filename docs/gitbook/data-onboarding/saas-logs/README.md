# SaaS Logs

Panther Enterprise supports pulling logs directly from SaaS platforms such as Okta, OneLogin, and more.

The two mechanisms used are direct integrations \(by querying APIs\) and AWS EventBridge.

## Direct

Supported direct SaaS integrations include:

* [Box](box.md)
* [G Suite](gsuite.md)
* [Okta](okta.md)
* [OneLogin](onelogin.md)
* [Slack](slack.md)
* [Crowdstrike](crowdstrike.md)
* [Duo](duo.md)
* [Salesforce](salesforce.md)
* [Microsoft Office 365](microsoft.md)
* More coming soon!

{% hint style="info" %}
By default, we poll for new logs every minute.
{% endhint %}

## EventBridge

Panther has direct support for pulling log data from AWS EventBridge, enabling real-time streaming and simple ingestion of [supported SaaS integrations](https://aws.amazon.com/eventbridge/integrations/).

* [OneLogin](onelogin.md)
* _\(Coming soon!\)_ Auth0
* _\(Coming soon!\)_ NewRelic
* _\(Coming soon!\)_ ZenDesk

