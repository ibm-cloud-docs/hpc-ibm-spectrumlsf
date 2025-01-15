---

copyright:
  years: 2025
lastupdated: "2025-01-15"

keywords: 

subcollection: hpc-ibm-spectrumlsf

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

## Activity Tracker Event Routing
{: #activity-tracker-overview}

We do not support the creation of new Activity Tracker instances, as this feature has been deprecated and replaced by IBM Cloud Logs; however, Activity Tracker event routing remains supported.

As part of Activity Tracker event routing, we support two target types:

* Cloud Logs
* Cloud Object Storage(COS) Bucket

To use Cloud Logs as a target, a Cloud Logs instance is created and configured for Activity Tracker event routing. Even if the observability_logs_enable variable is not set to true, a Cloud Logs instance will still be created for Activity Tracker. 

When observability_logs_enable is set to true, the same Cloud Logs instance can be utilized as a target, enabling the filtering of management, compute, and Activity Tracker logs within a unified dashboard.

For COS buckets, you can provide an existing COS instance. Under this instance, we will create a COS bucket that will act as a target for Activity Tracker event routing.

To configure this Activity Tracker event routing, we have introduced two new variables:

1. `observability_atracker_enable`

    * Purpose: Configures Activity Tracker event routing to determine how auditing events are routed.

    * Usage: While multiple Activity Tracker event routings can be created, only one is needed to capture all events. If an existing Activity Tracker is already integrated with a COS bucket or Cloud Logs instance, set this value to false to avoid creating redundant trackers. All events can then be monitored and accessed through the existing tracker.

2. `observability_atracker_target_type`

    * Purpose: Determines where all events will be stored, based on user input.

    * Options: COS Bucket or Cloud Logs.

    * Usage: Select the desired target type to retrieve or ingest events into your system.
