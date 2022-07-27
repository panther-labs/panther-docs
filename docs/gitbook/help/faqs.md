---
description: Commonly asked questions about using Panther.
---

# FAQ

## General Questions

### How do I get started with using Panther?

Panther's [Quick Start](../quick-start.md) documentation helps you get started with logging in, inviting users to your Panther Console, onboarding the logs you want to monitor, setting up [Alert destinations](glossary.md#alert-destination), and configuring [Detections](../writing-detections/) to alert for common security threats.

### What are the advantages of running Panther over a traditional Security Information and Events Manager ([SIEM](glossary.md#siem-security-information-and-event-manager))?&#x20;

Panther offers real-time threat detection on data, flexible detections using Python, fast queries on any size data set, a high-performance [security data lake](glossary.md#security-data-lake), and a server-less architecture with no operations overhead. To learn more, see [Panther Overview: Benefits](../#benefits).

## Writing Detections

### How does Panther handle errors on code/exceptions and do I have visibility when things fail?

If a [Rule](../writing-detections/rules.md) raises an exception, an Alert is sent with details about the exception to the same destination assigned to that Rule. The details of the exception are also pushed to the data lake, into a separate table from the Rule matches.

### What is the recommended way to do exception handling?

Detections should raise exceptions when either the logs are misconfigured (e.g., you believe a field MUST have a value and it doesn’t) or when an external call (e.g., an API call) fails. You can either raise an exception yourself or more commonly just allow the exception to happen and [review the Alert sent about the exception](faqs.md#how-does-panther-handle-errors-on-code-exceptions-and-do-we-get-visibility-when-things-fail). This does not affect the ability of other Rules to run.

### Is there any secret management functionality for Detections?

By default, the Rules and Policies engines can retrieve secrets from the AWS secrets manager. The permissions are scoped to secrets that have the prefix `panther-analysis` in the secret name.

## Managing Detections

### What is the best strategy for automating the deployment of Detections?

The best way for technical teams to manage Detections is with our [`panther_analysis_tool`](https://github.com/panther-labs/panther\_analysis\_tool) combined with a CI/CD pipeline. The [CLI](glossary.md#cli-command-line-interface) tool can run unit tests and upload your Python Detections directly to Panther. We recommend users fork our [public Detections repo](https://github.com/panther-labs/panther-analysis) and use the `panther_analysis_tool` in a CI pipeline that only allows merges when a unit test passes and automatically uploads changes to your deployment.

When you’re ready to pull our changes in, you can pull in our upstream changes and they will be deployed just like your changes.

### Do users often have a dev and prod environment for testing Detections?

Generally Panther users do not see a full dev/prod deployment just for testing detections. Most teams rely on the unit testing built into Panther to ensure their Detections are working properly. \
\
Some users create new Detections and assign their destination to a specific “dev” environment (e.g. a Slack channel that is muted or a dummy email that no one watches). Then after a few days, users check to see if has alerted on anything, and if so to what severity degree.

### If I still want to use a separate dev Panther approach, how do other Panther customers tackle development and testing?

The majority of our customers rely primarily on the unit testing capability combined with a CI pipeline that enforces passing unit tests with a minimum number of unit tests per Detection (with the `--minimum-tests` flag).
