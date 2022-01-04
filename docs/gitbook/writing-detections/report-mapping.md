# Report Mapping

{% hint style="info" %}
This feature is only available in version 1.25 and above
{% endhint %}

Panther supports the ability to map rules, policies, and scheduled rules to compliance frameworks for the purposes of tracking coverage against that framework. To map a detection to a framework, follow the steps below:

* Whether you're creating a new detection or editing an existing detection, you should be able to view the **"Report Mapping"** tab in the detection creation/edit form.
* Enter the framework name in the **"report key"** field and then the specific framework requirement name (e.g. 1.2.1) in the **"report values"** field. You can enter multiple report values by entering in a comma.

![](<../.gitbook/assets/Screen Shot 2021-11-08 at 9.53.47 PM.png>)

* Once completed, you can **Save** or **Update** the detection which will save the report mapping.
* You can then view the report mapping in the **Detection Details** page (see below).

![](<../.gitbook/assets/Screen Shot 2021-11-08 at 10.00.16 PM.png>)
