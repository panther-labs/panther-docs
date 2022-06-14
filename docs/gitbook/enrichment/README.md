---
description: >-
  Panther's Enrichment features, including Lookup Tables and GreyNoise Threat
  Intelligence
---

# Enrichment (Beta)

## Overview

Panther's Enrichment capabilities allow security teams to cut through background noise, improve alert fidelity, speed up analyst workflows and ensure prioritization of the most critical alerts.&#x20;

## How to use Enrichment features in Panther

### Lookup Tables (Beta)

Lookup Tables allow you to add important context to your detections and alerts for improved investigation workflows. Using Lookup Tables saves time by enhancing detections, reducing alert noise, and speeding up investigations

For setup instructions and examples, see Panther's [Lookup Tables documentation](./#lookup-tables-beta).&#x20;

### GreyNoise Threat Intelligence (Beta)

GreyNoise helps security analysts save time by revealing which events and alerts they can ignore, by curating data on IPs that saturate security tools with noise. Using GreyNoise can reduce false positives, while helping security teams feel more confident in their ability to protect their organizations.

For setup instructions and examples, see Panther's [GreyNoise documentation](greynoise/).

## Enrichment use case examples

### Common use cases for Lookup Tables

* Convert IPs to asset/user names, or geolocation details
* Group IPs by type (development vs. production for ex.)
* Append context to AWS Account IDs

### Common use cases for GreyNoise

* Modify an alert's severity depending on whether GreyNoise reports that an IP is malicious or benign
* Reduce alert noise and fatigue if an IP is known to belong to a common business service that is most definitely not being used to attack your services
* Enrich Panther alert context with GreyNoise data points
