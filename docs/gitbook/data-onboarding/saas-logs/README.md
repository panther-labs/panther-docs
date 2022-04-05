# SaaS Logs

Panther supports pulling logs directly from SaaS platforms such as Okta, CrowdStrike, Google Workspaces (G Suite), and more. By default, we poll for new logs every minute. &#x20;

Panther provides two mechanisms for various integrations:&#x20;

* Direct integrations by querying APIs &#x20;
* AWS EventBridge&#x20;

View the lists of Direct integrations and EventBridge integrations below.

## Direct

Panther offers direct integrations to pull in logs from the following service providers:&#x20;

* [1Password](1password.md)
* [Atlassian](https://docs.runpanther.io/data-onboarding/saas-logs/atlassian)
* [Box](box.md)
* [CrowdStrike](crowdstrike.md)
* [Duo](duo.md)
* [Google WorkSpaces (formerly G Suite](gsuite.md))
* [Github](github.md)
* [Okta](okta.md)
* [OneLogin](onelogin.md)
* [Salesforce](salesforce.md)
* [Slack](slack.md)
* [Microsoft 365](microsoft.md)
* [Zendesk](zendesk.md)
* [Zoom](zoom.md)
* More coming soon!

We will continue to build integrations for more services in the future. If you do not see the log source you want, [you can request support of a log source in the Panther Console](https://docs.panther.com/data-onboarding#request).

## EventBridge

Panther has direct support for pulling log data from AWS EventBridge, enabling real-time streaming, and simple ingestion of [supported SaaS integrations](https://aws.amazon.com/eventbridge/integrations/).

* [OneLogin](onelogin.md)
* _(Coming soon!)_ Auth0
* _(Coming soon!)_ NewRelic
