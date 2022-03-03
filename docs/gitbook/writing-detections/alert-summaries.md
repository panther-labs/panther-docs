# Alert Summaries

Alert summaries are a Panther feature that will automatically help you instantly know the answers to the `Who` , `What`, `Where` questions when triaging matching events in a rule alert. This feature is extremely useful when a rule has generated large numbers of matching events making understanding the nature of the threat(s) difficult. The alert summaries provide a view over all of the matching events that are often sufficient to avoid manually reviewing each event individually.

When creating a rule there is the option to declare `Summary Attributes` (see lower right corner). When displaying an alert there is a `Summary` tab. Selecting the `Summary` tab will display the top five attributes for each declared Summary Attribute. You should pick attributes that will help you understand the nature of an alert at a glance.

For example, say we have written a rule to find "Sneaky" traffic hitting our load balancer. This rule runs against the`AWS.ALB` logs. If we pick the Panther standard field `p_any_ip_addresses` and `userAgent`, then when we view an alert we can quickly see the top five values in the matching events. This can significantly speed up alert triage.

![Summary Attributes](../../../.gitbook/assets/rule-with-summary-attributes.png)

In this example, the first summary is `p_any_ip_addreses`. Notice that when you click on a bar a `Copy` icon displays. Copying the attribute of interest can be very handy. For example, to paste into [Indicator Search](../data-analytics/indicator-search.md) and view all hits for that attribute in your data lake.

![Alert Summary 1](../../../.gitbook/assets/alert-summary-1.png)

Clicking the arrow above the chart allows you to navigate to the next summary.

![Alert Summary 2](../../../.gitbook/assets/alert-summary-2.png)

The dropdown selector on the right allows you to jump to a particular summary.

If a rule does not have any `Summary Attributes` defined, then summaries will be computed for all the Panther standard `p_any` fields associated with the target log types.
