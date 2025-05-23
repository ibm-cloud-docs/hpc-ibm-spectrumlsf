---

copyright:
  years: 2025
lastupdated: "2025-02-20"

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

{{site.data.keyword.atracker_short}} is a platform service, which manages the auditing events at the account-level by configuring targets and routes that define where auditing data is routed.

The {{site.data.keyword.at_short}} instances are not supported as this feature has been deprecated and replaced by {{site.data.keyword.logs_full_notm}}.
{: note}

Two target types are supported as part of {{site.data.keyword.atracker_short}}:

* Cloud Object Storage(COS) bucket
* {{site.data.keyword.logs_full}}

By default, `observability_atracker_target_type` is set to cloudlogs as a target type, which creates the Cloud Logs instance and configures it for Activity Tracker Event Routing. Even if the `observability_logs_enable_for_management` or `observability_logs_enable_for_compute` variable is not set to true, a {{site.data.keyword.logs_full}} instance is still created for {{site.data.keyword.at_short}}.

When `observability_logs_enable_for_management` or `observability_logs_enable_for_compute` is set to true, the same {{site.data.keyword.logs_full}} instance can be used as a target, enabling the filtering of management, compute, and {{site.data.keyword.at_short}} logs within a unified dashboard.

For Cloud Object Storage bucket as a target, you can provide an existing COS instance as well. Under this instance, automation creates a COS bucket that acts as a target for {{site.data.keyword.atracker_short}}.

If you do not provide any existing COS instance, then the solution creates the new one by default.

Two variables are required to configure the {{site.data.keyword.atracker_short}}:

1. `observability_atracker_enable`

    * Purpose: Configures {{site.data.keyword.atracker_short}} to determine how audit events routed.

    * Usage: While multiple {{site.data.keyword.atracker_short}} can be created, only one is needed to capture all events. If an existing {{site.data.keyword.at_short}} is already integrated with a COS bucket or {{site.data.keyword.logs_full_notm}} instance, set this value to "false" to avoid creating redundant trackers. All events can then be monitored and accessed through the existing tracker.

2. `observability_atracker_target_type`

    * Purpose: Determines where all events can be stored based on user input.

    * Options: cloudlogs or cos

    * Usage: Select the desired target type to retrieve or capture events into your system.

## Validating Activity Tracker Event Routing
{: #activity-tracker-validate}

To validate the Activity Tracker event routing by using the CLI, first install the atracker plug-in:

```
ibmcloud plugin install atracker
```
{: pre}

### Checking an Activity Tracker Route
{: #activity-tracker-check}

Run the following command to retrieve details about an Activity Tracker Event Routing route:

```
ibmcloud atracker route get --route ROUTE [--output FORMAT]
```
{: pre}

For example:

```
ibmcloud atracker route get --route nproba-atracker-route
```
{: pre}

Sample Output:

```
OK
Route
Name:         <cluster_prefix>-atracker-route
ID:           5ba1ea49-3b93-4b48-a3e1-17da64ca81f1
CRN:          crn:v1:bluemix:public:atracker:global:a/    ec1b082b25144a52bb1a269c883d5a00::route:5ba1ea49-3b93-4b48-a3e1-17da64ca81f1
Version:      0
Rules:        [[ceada6af-7381-4297-9a9d-ce4b9aac8cb2],[*,global]]
CreatedAt:    2025-01-29T07:40:42.854Z
UpdatedAt:    2025-01-29T07:40:42.854Z
API version:  2
```
{: pre}

### Validating an Activity Tracker Target
{: #activity-tracker-target}

Run the following command to check whether a target is correctly configured for an IBM Cloud Activity Tracker Event Routing region:

```
ibmcloud atracker target validate --target TARGET [--region REGION] [--output FORMAT]
```
{: pre}

For example:

```
ibmcloud atracker target validate --target ceada6af-7381-4297-9a9d-ce4b9aac8cb2
```
{: pre}

Sample Output:

```
OK
Target
Name:                    nproba-atracker-target
ID:                      ceada6af-7381-4297-9a9d-ce4b9aac8cb2
CRN:                     crn:v1:bluemix:public:atracker:us-east:a/ec1b082b25144a52bb1a269c883d5a00::target:ceada6af-7381-4297-9a9d-ce4b9aac8cb2
Region:                  us-east
Type:                    cloud_logs
Cloud Logs Target CRN:   crn:v1:bluemix:public:logs:us-east:a/ec1b082b25144a52bb1a269c883d5a00:12ebd90f-f9d6-4c94-a8db-1e0965f4637d::
Write Status:            success
CreatedAt:               2025-01-29T07:40:38.091Z
UpdatedAt:               2025-01-29T07:40:38.091Z
```
{: pre}

If you set `observability_atracker_target_type` to cloudlogs, then the output includes a Cloud Logs Target CRN. If the `observability_atracker_target_type` is set to cos, then the output contains a Cloud Object Storage Target CRN instead.
