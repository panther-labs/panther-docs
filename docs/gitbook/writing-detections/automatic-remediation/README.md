# Automatic Remediation

## Overview

Panther supports Automatic Remediation of non-compliant resources to ensure that your infrastructure is as secure as possible.

This works by:

* Associating a remediation with a given Policy
* When a Policy failure occurs, the resource in the target account is remediated according to the parameters passed

The following diagram shows how Panther supports Automatic Remediation:

![remediation diagram](../../.gitbook/assets/readme-overview%20%282%29.png)

## Setup

Enabling automatic remediation for a Cloud Security source is simple.

The only requirement is to check the `AWS Automatic Remediations` checkbox while onboarding a Cloud Security source and the prerequisite role will be deployed as part of the onboarding stack.

![enable remediations checkbox](../../.gitbook/assets/readme-setup%20%287%29.png)

To enable automatic remediation on an existing source, edit the source and check the "enable automatic remediation" box.

This will bring you to the same setup wizard as above with instructions on how to deploy the updated CloudFormation stack.

