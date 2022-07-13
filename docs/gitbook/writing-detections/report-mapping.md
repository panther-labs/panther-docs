---
description: Map detections to compliance frameworks in Panther
---

# Report Mapping

Panther supports the ability to map rules, policies, and scheduled rules to compliance frameworks for the purposes of tracking coverage against that framework.&#x20;

Version 1.37 and newer allows you to directly map your Detection against the MITRE ATT\&CK Matrix. To learn how to assign Tactic and Technique combos to your Detections, see the documentation: [MITRE ATT\&CK Matrix](mitre-attack.md).

**To map a detection to a framework**:

1. Log in to the Panther Console.
2. In the left sidebar, click **Detections > All Detections**. Click the three dots icon in the upper right side of a Detection to view its details.
3. Click the Report Mapping tab.
4. Under Report Mapping:
   * Enter the framework name into the Report Key field.&#x20;
   * Enter the specific framework requirement name into the Report Values field.
     * You can enter multiple report values separated by a comma.\
       ![](<../.gitbook/assets/Screen Shot 2021-11-08 at 9.53.47 PM.png>)
5. Click **Update** in the upper right corner.

You can view the report mapping in the **Detection Details** page:

![](<../.gitbook/assets/Screen Shot 2021-11-08 at 10.00.16 PM.png>)
