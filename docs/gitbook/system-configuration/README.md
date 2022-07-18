# System Configuration

## Overview

This section of Panther's documentation covers how to configure your Panther Console and your overall Panther deployment. Panther features several configurable settings that enable you to tailor the service for your needs.

## Panther Console Configurations and Features

### Role-Based Access Control

[Role-Based Access Control (RBAC)](../help/glossary.md#rbac-role-based-access-control) gives Panther deployments granular access control for its user accounts. All roles, including the three Panther default roles (Admin, Analyst, and AnalystReadOnly), are customizable by any user with `UserModify` permissions. For more information, see [Role-Based Access Control](rbac.md).

### SAML/SSO Integration

Integrate with SAML Identity Providers (IdP) to enable user login to the Panther Console via [SSO](../help/glossary.md#sso-single-sign-on). Panther integrates with the following providers:

* [G Suite](saml/gsuite.md)
* [OneLogin](saml/onelogin.md)
* [Okta](saml/okta.md)

In addition, Panther supports integrating with any SAML IdP via the [Generic SSO](saml/generic.md) integration.

For more information, see [SAML/SSO Integration](saml/).

### Panther Audit Logs (Beta)

Panther audit logs provide a read-only history of activity within your Panther deployment. You can write detections or query your [data lake](../help/glossary.md#security-data-lake) for audit logs the same way you would with any other security events ingested by Panther. For more information, see [Panther Audit Logs (Beta)](panther-audit-logs/).

### System Health Notifications

Panther's System Health Notifications alert you with a "System Error" when a part of the Panther platform is not functioning correctly. This includes the following types of notifications:

* [Log source health notifications](system-health-notifications/configuring-system-health-notification-alarms.md)
* [Log classification errors](system-health-notifications/log-classification-error.md)
* [Alert delivery failures](system-health-notifications/alert-delivery-failure.md)
* [Cloud security scanning failures](system-health-notifications/cloud-security-scanning-failure.md)

For more information, see [System Health Notifications.](system-health-notifications/)

### Panther Snowflake Integration

Panther is configured to write processed log data to an AWS-based [Snowflake](https://www.snowflake.com) database cluster. Using Panther with Snowflake enables Panther data to both integrate with your given Business Intelligence tools and to perform assessments of your organization's security posture. For more information, see [Snowflake Integration](snowflake-setup/).

