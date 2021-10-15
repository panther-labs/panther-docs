# Detection Auxiliary Functions

## Functions

Panther Detections contain several Python3 functions that control the analysis logic, generated alert title, event grouping, routing of alerts, and metadata overrides.

Rules are very customizable and can import from standard Python libraries or [global helpers](globals.md). Additional functions, variables, or classes may also be defined outside of the functions defined below for advanced use or cleaner code.  Starting in version 1.24, these auxiliary functions will also be available to policies.

{% hint style="info" %}
Read [Runtime Environment](run-time.md) to learn more about the available libraries and how to add custom or third-party ones. 
{% endhint %}

Each function listed below takes a single argument of `event` (for rules) or `resource` (for policies).

| Name            | Description                                                                                                 | Return Value                           | Default Return Value                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------- | -------------------------------------- | ---------------------------------------------------------- |
| `title`         | The generated alert title                                                                                   | `String`                               | If not defined, the `Rule Display Name` or`RuleID` is used |
| `dedup`         | The string to group related events with, limited to 1000 characters                                         | `String`                               | If not defined, the `title`function output is used         |
| `alert_context` | Additional context to pass to the alert destination(s)                                                      | `Dict[String: Any]`                    | An empty `Dict`                                            |
| `severity`      | The level of urgency of the alert                                                                           | `INFO, LOW, MEDIUM, HIGH, or CRITICAL` | The severity as defined in the rule metadata               |
| `description`   | An explanation about why the rule exists                                                                    | `String`                               | The description as defined in the rule metadata            |
| `reference`     | A reference URL to an internal document or online resource about the rule                                   | `String`                               | The reference as defined in the rule metadata              |
| `runbook`       | A list of instructions to follow once the alert is generated                                                | `String`                               | The runbook as defined in the rule metadata                |
| `destinations`  | The label or ID of the destinations to specifically send alerts to. An empty list will suppress all alerts. | `List[Destination Name]`               | The destinations as defined in the rule metadata           |

##
