# GreyNoise (BETA)

{% hint style="info" %}
This feature is available in version 1.32 and newer.

You will need to accept new Terms of Service for Panther and GreyNoise in the Panther Console to use this feature.\


GreyNoise integration is currently in beta. This means:&#x20;

* We would greatly appreciate feedback such as bug reports and feature requests.
* The feature will gain new functionality in future versions of Panther.&#x20;
* The UI is likely to significantly evolve over time.
{% endhint %}

Panther has partnered with [GreyNoise Intelligence](https://www.greynoise.io), a cybersecurity platform that collects and analyzes Internet-wide data, to provide integrated threat intelligence to Panther customers.&#x20;

Use Panther detection capabilities with GreyNoise threat intelligence data to reduce false-positive alerts by:

* Ruling out internet background noise from external event sources to ensure you're focused on most critical events first..
* Identifying potential opportunistic attacks that may have been allowed into your perimeter.
* Identifying emerging threats based on GreyNoise context data and tagging.

### Overview

The video below shows a demo of the GreyNoise functionality in Panther using the [Basic package](./#packaging-and-cost), which is available at no additional cost to all Panther customers.

{% embed url="https://panther.wistia.com/medias/tiotop7cil" %}
Overview of using GreyNoise data sets with Panther
{% endembed %}

GreyNoise helps security analysts save time by revealing which events and alerts they can ignore. They do this by curating data on IPs that saturate security tools with noise. This unique perspective helps analysts ignore irrelevant or harmless activity, creating more time to uncover and investigate true threats. This data is delivered through SIEM, SOAR and TIP integrations, API, command-line tool, bulk data and visualizer. GreyNoise is trusted by the DoD, Fortune 500 enterprises, top security vendors and thousands of threat researchers. For more information, please visit [greynoise.io](https://www.greynoise.io).

### GreyNoise Data Sets

GreyNoise data in Panther comes in two flavors: [**Noise** and **RIOT**.](https://docs.greynoise.io/docs/understanding-greynoise-data-sets)

The **Noise data set** features information from GreyNoise’s internet-wide sensor network that passively collects packets from hundreds of thousands of IPs seen scanning the internet every day. GreyNoise analyzes and enriches this data to identify behavior, methods, and intent, giving analysts the context they need to take action. Noise data is refreshed approximately every hour in Panther.

The **RIOT data set** contains IPs used by common business services that are not likely to be used to attack your services. RIOT enables security practitioners to quickly eliminate logs and events generated from common business services from their security telemetry to quickly rule them out. RIOT data is refreshed approximately every four hours in Panther.

### GreyNoise Packages in Panther

The native GreyNoise integration with Panther includes two different packages options: **Basic** and **Advanced**. Both packages include the Noise and RIOT data sets.

#### GreyNoise Basic Package

* Included with the Panther subscription for all customers for unlimited use
* Answers the question: “Is this internet background noise or a common business service IP?”
* [The data set is limited to a few fields of data](basic-vs.-advanced.md)

#### GreyNoise Advanced Package

* Available at additional cost (requires [GreyNoise Automate](https://www.greynoise.io/pricing) product)
  * 30-day free trial available upon request
* Provides full context details from GreyNoise for advanced filtering and hunting&#x20;

{% hint style="info" %}
Contact your Panther representative to get started with a free trial of GreyNoise Advanced.
{% endhint %}

### How Panther and GreyNoise Work Together

The following diagram describes the alert lifecycle in Panther, where native enrichment with GreyNoise and Lookup Tables is supported:&#x20;

![](<../../.gitbook/assets/image (7).png>)

* Alert events are automatically enriched with both custom [Lookup Tables](../lookup-tables/) and native GreyNoise data under the `p_enrichment` field in JSON events.
* GreyNoise data can be used in detections with [pre-built Python helpers](greynoise-helper-function-usage-and-methods.md) (and `deep_get`) to access enrichment information.
* GreyNoise data sets are stored as Panther-managed Lookup Tables in bulk, so there is no need to make API calls to leverage this enrichment in your detection logic or alerts.
