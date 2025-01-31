---

copyright:
  years: 2025
lastupdated: "2025-01-31"

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

Creation of new {{site.data.keyword.at_short}} instances is not supported, as this feature has been deprecated and replaced by {{site.data.keyword.logs_full_notm}}. However, the {{site.data.keyword.atracker_short}} is still supported.

There are two target types supported as part of {{site.data.keyword.atracker_short}}:

* Cloud Object Storage(COS) bucket
* {{site.data.keyword.logs_full}}

To use {{site.data.keyword.logs_full}} as a target, a {{site.data.keyword.logs_full}} instance is created and configured for {{site.data.keyword.atracker_short}}. Even if the `observability_logs_enable` variable is not set to true, a {{site.data.keyword.logs_full}} instance is still created for {{site.data.keyword.at_short}}.

When `observability_logs_enable` is set to true, the same {{site.data.keyword.logs_full}} instance can be utilized as a target, enabling the filtering of management, compute, and {{site.data.keyword.at_short}} logs within a unified dashboard.

For COS bucket as a target, you can provide an existing COS instance as well. Under this instance, automation will create a COS bucket that will act as a target for {{site.data.keyword.atracker_short}}.

If you will not provide any existing COS instance, then we will create the new one by default.

There are two new variables to configure the {{site.data.keyword.atracker_short}}:

1. `observability_atracker_enable`

    * Purpose: Configures {{site.data.keyword.atracker_short}} to determine how auditing events are routed.

    * Usage: While multiple {{site.data.keyword.atracker_short}} can be created, only one is needed to capture all events. If an existing {{site.data.keyword.at_short}} is already integrated with a COS bucket or {{site.data.keyword.logs_full}} instance, set this value to "false" to avoid creating redundant trackers. All events can then be monitored and accessed through the existing tracker.

2. `observability_atracker_target_type`

    * Purpose: Determines where all events will be stored, based on user input.

    * Options: cloudlogs or cos

    * Usage: Select the desired target type to retrieve or ingest events into your system.

## Validating Activity Tracker Event Routing
{: #activity-tracker-validate}

To validate the Activity Tracker event routing using the CLI, first install the atracker plugin:

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
Name:     nproba-atracker-route
ID:      5ba1ea49-3b93-4b48-a3e1-17da64ca81f1
CRN:      crn:v1:bluemix:public:atracker:global:a/ec1b082b25144a52bb1a269c883d5a00::route:5ba1ea49-3b93-4b48-a3e1-17da64ca81f1
Version:    0
Rules:     [[ceada6af-7381-4297-9a9d-ce4b9aac8cb2],[*,global]]
CreatedAt:   2025-01-29T07:40:42.854Z
UpdatedAt:   2025-01-29T07:40:42.854Z
API version:  2
```
{: pre}

### Validating an Activity Tracker Target
{: #activity-tracker-target}

Run the following command to check if a target is correctly configured for an IBM Cloud Activity Tracker Event Routing region:

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

If you set `observability_atracker_target_type` = cloudlogs, then the output will include a Cloud Logs Target CRN. If `observability_atracker_target_type` = cos, then the output will contain a COS Target CRN instead.
