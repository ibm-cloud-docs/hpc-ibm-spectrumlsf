---

copyright:
  years: 2025
lastupdated: "2025-01-17"

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

# IBM Cloud Logs
{: #cloud-logs-overview}

{{site.data.keyword.logs_full_notm}} is a scalable logging service designed to persist logs while providing users with robust capabilities for querying, tailing, and visualising their logs efficiently. This service supports enhanced observability for both management and compute resources in the {{site.data.keyword.cloud_notm}} environment. 

## Key features for {{site.data.keyword.logs_full_notm}}
{: #key-features}

Following are the key features of {{site.data.keyword.logs_full_notm}}:

* Scalable log storage and persistence.

* Advanced querying, tailing, and visualisation capabilities.

* Integration options for management and compute Virtual Server Instances (VSIs).

## Cloud Logs Functionality
{: #cloud-log-functionality}

Users can enable cloud logs to capture infrastructure and application logs from both management and compute nodes. To facilitate this, we have introduced four new variables:

* `observability_logs_enable_for_management`: Set false to disable {{site.data.keyword.logs_full_notm}} integration. If enabled, infrastructure and LSF application logs from Management Nodes will be ingested.

* `observability_logs_enable_for_compute`: Set false to disable {{site.data.keyword.logs_full_notm}} integration. If enabled, infrastructure and LSF application logs from Compute Nodes will be ingested.

* `observability_enable_platform_logs`: Setting this to true will create a tenant in the same region that the {{site.data.keyword.logs_full}} instance is provisioned to enable platform logs for that region. NOTE: You can only have 1 tenant per region in an account.

* `observability_logs_retention_period`: The number of days {{site.data.keyword.logs_full_notm}} will retain the logs data in Priority insights. Allowed values: 7, 14, 30, 60, 90.

## Verifying Log Flow
{: #verify-log-flow}

To ensure that logs are successfully flowing to the {{site.data.keyword.logs_full_notm}} instance, test messages are sent via userdata.

The resulting dashboard provides a visual confirmation of log ingestion and flow.

![Architecture diagram.](images/verifying_log_flow.png "Verifying the log flow"){: caption="Verifying the log flow" caption-side="bottom"}

## Using Filters
{: #using-filters}

Users can apply filters based on the subsystem and application to refine the logs:

* To view logs from management nodes only, select the **management** subsystem.

* To view logs from compute nodes, select the **compute** subsystem.

If your log instance is also configured as a target for Activity Tracker event routing, additional application names may appear besides "LSF". To exclude audit events, filter by the LSF application specifically.

## Supported Operating Systems
{: #supported-operating-sys}

The supported operating systems are RHEL and Ubuntu.

For more information on {{site.data.keyword.logs_full_notm}}, refer the documentation [here](/docs/cloud-logs?topic=cloud-logs-getting-started).
