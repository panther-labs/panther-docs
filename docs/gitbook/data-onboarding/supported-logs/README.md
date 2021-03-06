---
description: Panther supports 100+ security log types across 36 different categories
---

# Supported Logs

## Overview

Panther has native schema support for all of the following sources, with different supported methods to ingest data depending on the log source.&#x20;

If you do not see a needed source listed as supported, you can either define your own log type via a [Custom Log](../custom-log-types/) entry or [request support of a new log source](../#request).

## Panther Supported Logs

* [1Password](1password.md)
  * [ItemUsage](https://docs.panther.com/data-onboarding/supported-logs/1password#onepassword.itemusage)
  * [SignInAttempt](https://docs.panther.com/data-onboarding/supported-logs/1password#onepassword.signinattempt)
* [Apache](apache.md)
  * [AccessCombined](https://docs.panther.com/data-onboarding/supported-logs/apache#apache.accesscombined)
  * [AccessCommon](https://docs.panther.com/data-onboarding/supported-logs/apache#apache.accesscommon)
* [Asana](asana.md)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/asana#asana.audit)
* [Atlassian](atlassian.md)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/atlassian#atlassian.audit)
* [AWS](aws.md)
  * [ALB](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.alb)
  * [AuroraMySQLAudit](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.auroramysqlaudit)
  * [CloudTrail](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.cloudtrail)
  * [CloudTrailDigest](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.cloudtraildigest)
  * [CloudTrailInsight](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.cloudtrailinsight)
  * [CloudWatchEvents](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.cloudwatchevents)
  * [GuardDuty](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.guardduty)
  * [S3ServerAccess](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.s3serveraccess)
  * [VPCDns](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.vpcdns)
  * [VPCFlow](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.vpcflow)
  * [WAFWebACL](https://docs.panther.com/data-onboarding/supported-logs/aws#aws.wafwebacl)
* [Box](box.md)
  * [Event](https://docs.panther.com/data-onboarding/supported-logs/box#box.event)
* [CiscoUmbrella](ciscoumbrella.md)
  * [CloudFirewall](https://docs.panther.com/data-onboarding/supported-logs/ciscoumbrella#ciscoumbrella.cloudfirewall)
  * [DNS](https://docs.panther.com/data-onboarding/supported-logs/ciscoumbrella#ciscoumbrella.dns)
  * [IP](https://docs.panther.com/data-onboarding/supported-logs/ciscoumbrella#ciscoumbrella.ip)
  * [Proxy](https://docs.panther.com/data-onboarding/supported-logs/ciscoumbrella#ciscoumbrella.proxy)
* [Cloudflare](cloudflare.md)
  * [Firewall](https://docs.panther.com/data-onboarding/supported-logs/cloudflare#cloudflare.firewall)
  * [HttpRequest](https://docs.panther.com/data-onboarding/supported-logs/cloudflare#cloudflare.httprequest)
  * [Spectrum](https://docs.panther.com/data-onboarding/supported-logs/cloudflare#cloudflare.spectrum)
* [CrowdStrike](crowdstrike.md)
  * [AIDMaster](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.aidmaster)
  * [ActivityAudit](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.activityaudit)
  * [AppInfo](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.appinfo)
  * [CriticalFile](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.criticalfile)
  * [DNSRequest](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.dnsrequest)
  * [DetectionSummary](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.detectionsummary)
  * [GroupIdentity](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.groupidentity)
  * [ManagedAssets](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.managedassets)
  * [NetworkConnect](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.networkconnect)
  * [NetworkListen](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.networklisten)
  * [NotManagedAssets](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.notmanagedassets)
  * [ProcessRollup2](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.processrollup2)
  * [ProcessRollup2Stats](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.processrollup2stats)
  * [SyntheticProcessRollup2](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.syntheticprocessrollup2)
  * [Unknown](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.unknown)
  * [UserIdentity](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.useridentity)
  * [UserInfo](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.userinfo)
  * [UserLogonLogoff](https://docs.panther.com/data-onboarding/supported-logs/crowdstrike#crowdstrike.userlogonlogoff)
* [Dropbox](dropbox.md)
  * [TeamEvent](dropbox.md#dropbox.teamevent)
* [Duo](duo.md)
  * [Administrator](https://docs.panther.com/data-onboarding/supported-logs/duo#duo.administrator)
  * [Authentication](https://docs.panther.com/data-onboarding/supported-logs/duo#duo.authentication)
  * [OfflineEnrollment](https://docs.panther.com/data-onboarding/supported-logs/duo#duo.offlineenrollment)
  * [Telephony](https://docs.panther.com/data-onboarding/supported-logs/duo#duo.telephony)
* [Fastly](fastly.md)
  * [Access](https://docs.panther.com/data-onboarding/supported-logs/fastly#fastly.access)
* [Fluentd](fluentd.md)
  * [Syslog3164](https://docs.panther.com/data-onboarding/supported-logs/fluentd#fluentd.syslog3164)
  * [Syslog5424](https://docs.panther.com/data-onboarding/supported-logs/fluentd#fluentd.syslog5424)
* [GCP](gcp.md)
  * [AuditLog](https://docs.panther.com/data-onboarding/supported-logs/gcp#gcp.auditlog)
* [Github](github.md)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/github#github.audit)
* [GitLab](gitlab.md)
  * [API](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.api)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.audit)
  * [Exceptions](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.exceptions)
  * [Git](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.git)
  * [Integrations](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.integrations)
  * [Production](https://docs.panther.com/data-onboarding/supported-logs/gitlab#gitlab.production)
* [G Suite](gsuite.md)
  * [EventActivity](https://docs.panther.com/data-onboarding/supported-logs/gsuite#gsuite.activityevent)
  * [Reports](https://docs.panther.com/data-onboarding/supported-logs/gsuite#gsuite.reports)
* [JAMF Pro](jamfpro.md)
  * [Login](jamfpro.md#jamfpro.login)
* [Juniper](juniper.md)
  * [Access](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.access)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.audit)
  * [Firewall](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.firewall)
  * [MWS](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.mws)
  * [Postgres](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.postgres)
  * [Security](https://docs.panther.com/data-onboarding/supported-logs/juniper#juniper.security)
* [Lacework](lacework.md)
  * [AgentManagement](lacework.md#lacework.agentmanagement)
  * [AllFiles](lacework.md#lacework.allfiles)
  * [ChangeFiles](lacework.md#lacework.changefiles)
  * [Cmdline](lacework.md#lacework.cmdline)
  * [Connections](lacework.md#lacework.connections)
  * [ContainerSummary](lacework.md#lacework.containersummary)
  * [ContainerVulnDetails](lacework.md#lacework.containervulndetails)
  * [DNSQuery](lacework.md#lacework.dnsquery)
  * [Events](lacework.md#lacework.events)
  * [HostVulnDetails](lacework.md#lacework.hostvulndetails)
  * [Image](lacework.md#lacework.image)
  * [Interfaces](lacework.md#lacework.interfaces)
  * [InternalIPA](lacework.md#lacework.internalipa)
  * [MachineDetails](lacework.md#lacework.machinedetails)
  * [MachineSummary](lacework.md#lacework.machinesummary)
  * [NewHashes](lacework.md#lacework.newhashes)
  * [Package](lacework.md#lacework.package)
  * [PodSummary](lacework.md#lacework.podsummary)
  * [ProcessSummary](lacework.md#lacework.processsummary)
  * [UserDetails](lacework.md#lacework.userdetails)
* [Microsoft 365](broken-reference)
  * [Audit.AzureActiveDirectory](https://docs.panther.com/data-onboarding/supported-logs/microsoft365#microsoft365.audit.azureactivedirectory)
  * [Audit.Exchange](https://docs.panther.com/data-onboarding/supported-logs/microsoft365#microsoft365.audit.exchange)
  * [Audit.General](https://docs.panther.com/data-onboarding/supported-logs/microsoft365#microsoft365.audit.general)
  * [Audit.SharePoint](https://docs.panther.com/data-onboarding/supported-logs/microsoft365#microsoft365.audit.sharepoint)
  * [DLP.All](https://docs.panther.com/data-onboarding/supported-logs/microsoft365#microsoft365.dlp.all)
* [Microsoft Graph (Beta)](microsoftgraph.md)
  * [SecurityAlert](microsoftgraph.md#microsoftgraph.securityalert)
* [Nginx](nginx.md)
  * [Access](https://docs.panther.com/data-onboarding/supported-logs/nginx#nginx.access)
* [Okta](okta.md)
  * [SystemLog](https://docs.panther.com/data-onboarding/supported-logs/okta#okta.systemlog)
* [OneLogin](onelogin.md)
  * [Events](https://docs.panther.com/data-onboarding/supported-logs/onelogin#onelogin.events)
* [Osquery](osquery.md)
  * [Batch](https://docs.panther.com/data-onboarding/supported-logs/osquery#osquery.batch)
  * [Differential](https://docs.panther.com/data-onboarding/supported-logs/osquery#osquery.differential)
  * [Snapshot](https://docs.panther.com/data-onboarding/supported-logs/osquery#osquery.snapshot)
  * [Status](https://docs.panther.com/data-onboarding/supported-logs/osquery#osquery.status)
* [OSSEC](ossec.md)
  * [EventInfo](https://docs.panther.com/data-onboarding/supported-logs/ossec#ossec.eventinfo)
* [Salesforce](salesforce.md)
  * [Login](https://docs.panther.com/data-onboarding/supported-logs/salesforce#salesforce.login)
  * [LoginAs](https://docs.panther.com/data-onboarding/supported-logs/salesforce#salesforce.loginas)
  * [Logout](https://docs.panther.com/data-onboarding/supported-logs/salesforce#salesforce.logout)
  * [URI](https://docs.panther.com/data-onboarding/supported-logs/salesforce#salesforce.uri)
* [Sophos](sophos.md)
  * [Central](https://docs.panther.com/data-onboarding/supported-logs/sophos#sophos.central)
* [Slack](slack.md)
  * [AccessLogs](https://docs.panther.com/data-onboarding/supported-logs/slack#slack.accesslogs)
  * [AuditLogs](https://docs.panther.com/data-onboarding/supported-logs/slack#slack.auditlogs)
  * [IntegrationLogs](https://docs.panther.com/data-onboarding/supported-logs/slack#slack.integrationlogs)
* [Suricata](suricata.md)
  * [Anomaly](https://docs.panther.com/data-onboarding/supported-logs/suricata#suricata.anomaly)
  * [DNS](https://docs.panther.com/data-onboarding/supported-logs/suricata#suricata.dns)
* [Syslog](syslog.md)
  * [RFC3164](https://docs.panther.com/data-onboarding/supported-logs/syslog#syslog.rfc3164)
  * [RFC5424](https://docs.panther.com/data-onboarding/supported-logs/syslog#syslog.rfc5424)
* [Teleport](teleport.md)
  * [TeleportAudit](https://docs.panther.com/data-onboarding/supported-logs/teleport#gravitational.teleportaudit)
* [Zeek](zeek.md)
  * [DNS](https://docs.panther.com/data-onboarding/supported-logs/zeek#zeek.dns)
* [Zoom](zoom.md)
  * [Activity](https://docs.panther.com/data-onboarding/supported-logs/zoom#zoom.activity)
  * [Operation](https://docs.panther.com/data-onboarding/supported-logs/zoom#zoom.operation)
* [Zendesk](zendesk.md)
  * [Audit](https://docs.panther.com/data-onboarding/supported-logs/zendesk#zendesk.audit)
