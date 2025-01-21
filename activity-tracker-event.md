---

copyright:
  years: 2025
lastupdated: "2025-01-21"

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

# IBM Cloud Activity Tracker Event Routing
{: #activity-tracker-overview}

We do not support the creation of new {{site.data.keyword.at_short}} instances, as this feature has been deprecated and replaced by {{site.data.keyword.logs_full_notm}}, however the {{site.data.keyword.atracker_short}} is still supported.

Two target types are supported, as part of {{site.data.keyword.atracker_short}}:

* Cloud Object Storage(COS) bucket
* {{site.data.keyword.logs_full}}

To use {{site.data.keyword.logs_full}} as a target, a {{site.data.keyword.logs_full}} instance is created and configured for {{site.data.keyword.atracker_short}}. Even if the `observability_logs_enable` variable is not set to true, a {{site.data.keyword.logs_full}} instance is still created for {{site.data.keyword.at_short}}.

When `observability_logs_enable` is set to true, the same {{site.data.keyword.logs_full}} instance can be utilized as a target, enabling the filtering of management, compute, and {{site.data.keyword.at_short}} logs within a unified dashboard.

For COS bucket as a target, you can provide an existing COS instance as well. Under this instance, you will create a COS bucket that will act as a target for {{site.data.keyword.atracker_short}}.

If you will not provide any existing COS instance, we will create the new one by default.

There are two new variables to configure the {{site.data.keyword.atracker_short}}:

1. `observability_atracker_enable`

    * Purpose: Configures {{site.data.keyword.atracker_short}} to determine how auditing events are routed.

    * Usage: While multiple {{site.data.keyword.atracker_short}} can be created, only one is needed to capture all events. If an existing {{site.data.keyword.at_short}} is already integrated with a COS bucket or {{site.data.keyword.logs_full}} instance, set this value to "false" to avoid creating redundant trackers. All events can then be monitored and accessed through the existing tracker.

2. `observability_atracker_target_type`

    * Purpose: Determines where all events will be stored, based on user input.

    * Options: cloudlogs or cos

    * Usage: Select the desired target type to retrieve or ingest events into your system.
